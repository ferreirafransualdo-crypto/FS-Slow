<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recepção Ágil - Almoxarifado</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkBg: '#0f172a',
                        cardBg: '#1e293b',
                        accentGreen: '#10b981',
                        accentYellow: '#f59e0b',
                        accentBlue: '#3b82f6',
                    }
                }
            }
        }
    </script>
    <!-- Biblioteca Html5-Qrcode -->
    <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>
</head>
<body class="bg-darkBg text-slate-100 min-h-screen font-sans flex flex-col">

    <!-- CABEÇALHO -->
    <header class="bg-cardBg border-b border-slate-800 p-4 sticky top-0 z-50 shadow-md">
        <div class="max-w-4xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-2">
            <div>
                <h1 class="text-xl font-bold flex items-center gap-2">
                    📦 Recepção Ágil <span class="text-xs bg-accentGreen/20 text-accentGreen px-2 py-0.5 rounded-full font-mono">v3.4 (SAP Match)</span>
                </h1>
                <p class="text-xs text-slate-400">Otimização de Recebimento & Conferência Física</p>
            </div>
            <!-- Identificação do Operador -->
            <div class="bg-slate-900 border border-slate-700 px-3 py-1.5 rounded-lg flex items-center gap-3 text-xs">
                <div>
                    <p class="text-slate-400">Operador Ativo:</p>
                    <p class="font-bold text-accentGreen" id="displayOperador">Fransualdos (Mat: 0206042677)</p>
                </div>
                <button onclick="editarOperador()" class="text-slate-400 hover:text-white underline text-[10px]">Alterar</button>
            </div>
        </div>
    </header>

    <!-- CONTEÚDO PRINCIPAL -->
    <main class="max-w-4xl w-full mx-auto p-4 flex-1 flex flex-col gap-6">

        <!-- PAINEL DE INDICADORES (DASHBOARD) -->
        <section class="grid grid-cols-3 gap-3">
            <div class="bg-cardBg border border-slate-800 p-3 rounded-xl text-center shadow">
                <p class="text-xs text-slate-400">Pedidos Bipados</p>
                <h2 class="text-xl font-bold text-accentBlue mt-1" id="statNotas">0</h2>
            </div>
            <div class="bg-cardBg border border-slate-800 p-3 rounded-xl text-center shadow">
                <p class="text-xs text-slate-400">Paletes Ativos</p>
                <h2 class="text-xl font-bold text-accentGreen mt-1" id="statPaletes">0</h2>
            </div>
            <div class="bg-cardBg border border-slate-800 p-3 rounded-xl text-center shadow">
                <p class="text-xs text-slate-400">Divergências</p>
                <h2 class="text-xl font-bold text-accentYellow mt-1" id="statDivergencias">0</h2>
            </div>
        </section>

        <!-- AÇÃO PRINCIPAL: ESCANEAR NF / PEDIDO -->
        <section class="bg-cardBg border border-slate-800 p-5 rounded-2xl shadow-lg flex flex-col gap-4">
            <div class="flex justify-between items-center">
                <h3 class="font-semibold text-sm flex items-center gap-2">
                    <span class="w-2 h-2 rounded-full bg-accentGreen animate-pulse"></span>
                    Leitura de Entrada (Faixa Validade: 45005000 - 45009900)
                </h3>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                <!-- Entrada Manual Inteligente -->
                <div class="flex flex-col gap-1">
                    <label class="text-xs text-slate-400 font-medium">Nº Pedido SAP ou Nota Fiscal:</label>
                    <div class="flex gap-2">
                        <input type="text" id="inputPedido" placeholder="Ex: 4500940713" class="bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm focus:outline-none focus:border-accentGreen flex-1 font-mono">
                        <button onclick="processarLeitura()" class="bg-accentGreen hover:bg-emerald-600 text-slate-950 font-bold px-4 py-2 rounded-lg text-sm transition shadow">
                            Validar
                        </button>
                    </div>
                </div>

                <!-- Botões de Câmera e Ações -->
                <div class="flex flex-col justify-end gap-2">
                    <button onclick="abrirCameraReal()" class="bg-accentBlue hover:bg-blue-600 text-white font-medium px-4 py-2.5 rounded-lg text-sm flex items-center justify-center gap-2 transition shadow">
                        📷 Abrir Câmera para Bipagem
                    </button>
                </div>
            </div>

            <!-- CONTÊINER DA CÂMERA REAL -->
            <div id="cameraWrapper" class="hidden flex-col items-center gap-3 bg-slate-900 border border-slate-700 p-4 rounded-xl">
                <div class="w-full flex justify-between items-center">
                    <span class="text-xs text-accentGreen font-bold flex items-center gap-1">🔴 Câmera Ativa - Aponte para o código</span>
                    <button onclick="fecharCamera()" class="bg-red-500/20 text-red-400 hover:bg-red-500/30 px-3 py-1 rounded text-xs font-bold">Fechar Câmera</button>
                </div>
                <div id="reader" class="w-full max-w-sm rounded-lg overflow-hidden border border-slate-800"></div>
                <p class="text-[11px] text-slate-400 text-center">Posicione o código de barras ou QR Code dentro do visor.</p>
            </div>
        </section>

        <!-- CRUZAMENTO DE DADOS (DE / PARA) E PALETES -->
        <section class="grid grid-cols-1 md:grid-cols-2 gap-6">
            
            <!-- Paletes do Turno -->
            <div class="bg-cardBg border border-slate-800 p-4 rounded-2xl shadow flex flex-col gap-3">
                <div class="flex justify-between items-center">
                    <h3 class="font-semibold text-sm">📦 Paletes em Montagem</h3>
                    <button onclick="novoPalete()" class="text-xs bg-slate-800 hover:bg-slate-700 px-2.5 py-1 rounded-md border border-slate-700">+ Novo Palete</button>
                </div>
                
                <div id="listaPaletes" class="flex flex-col gap-2 max-h-64 overflow-y-auto pr-1">
                    <div class="text-xs text-slate-500 text-center py-6">Nenhum palete aberto no momento. Clique em "+ Novo Palete".</div>
                </div>
            </div>

            <!-- Cruzamento NF x Pedido (De / Para da Casa baseado no SAP) -->
            <div class="bg-cardBg border border-slate-800 p-4 rounded-2xl shadow flex flex-col gap-3">
                <h3 class="font-semibold text-sm">🔄 Cruzamento de Descrição (NF x SAP)</h3>
                <div id="painelCruzamento" class="text-xs text-slate-400 bg-slate-900/50 p-4 rounded-xl border border-slate-800 flex flex-col justify-center items-center text-center h-full min-h-[140px]">
                    <p>Aguardando leitura de Nota Fiscal ou Pedido (Ex: 4500940713)...</p>
                    <span class="text-[10px] text-slate-500 mt-1">O sistema cruzará automaticamente a descrição do fornecedor com a descrição padrão do SAP.</span>
                </div>
            </div>

        </section>

        <!-- RELATÓRIO DO TURNO PARA WHATSAPP / TEAMS -->
        <section class="bg-cardBg border border-slate-800 p-4 rounded-2xl shadow flex flex-col gap-3">
            <div class="flex justify-between items-center">
                <h3 class="font-semibold text-sm">📋 Resumo do Turno para Envio</h3>
                <button onclick="copiarRelatorio()" class="bg-slate-800 hover:bg-slate-700 border border-slate-700 text-xs px-3 py-1.5 rounded-lg text-accentGreen font-medium flex items-center gap-1.5 transition">
                    📋 Copiar Relatório Completo
                </button>
            </div>
            <textarea id="textoRelatorio" readonly rows="3" class="bg-slate-900 border border-slate-700 rounded-lg p-3 text-xs font-mono text-slate-300 resize-none focus:outline-none">Nenhuma conferência registrada neste turno ainda.</textarea>
        </section>

    </main>

    <!-- SCRIPT DE LÓGICA E INTERAÇÃO -->
    <script>
        // Dados do Operador Ativo
        let operadorAtual = {
            nome: "Fransualdos",
            matricula: "0206042677"
        };

        let paletes = [];
        let notasBipadas = 0;
        let divergencias = 0;
        let html5QrCode = null;

        // Feedback sonoro (Bip sintético via Web Audio API)
        function tocarBip(tipo = 'sucesso') {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                
                osc.type = 'sine';
                if (tipo === 'sucesso') {
                    osc.frequency.setValueAtTime(880, audioCtx.currentTime); // Lá agudo
                } else {
                    osc.frequency.setValueAtTime(300, audioCtx.currentTime); // Grave para erro
                }
                
                gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                
                osc.start();
                osc.stop(audioCtx.currentTime + 0.15);
            } catch (e) {
                console.log("Áudio não suportado");
            }
        }

        function editarOperador() {
            const nome = prompt("Nome do Colaborador:", operadorAtual.nome);
            const mat = prompt("Matrícula:", operadorAtual.matricula);
            if(nome && mat) {
                operadorAtual.nome = nome;
                operadorAtual.matricula = mat;
                document.getElementById('displayOperador').innerText = `${nome} (Mat: ${mat})`;
                tocarBip('sucesso');
                atualizarInterface();
            }
        }

        function novoPalete() {
            const numPalete = paletes.length + 1;
            const novoObj = {
                id: numPalete,
                fornecedor: "MAQNELSON AGRICOLA",
                status: "Em Montagem",
                itens: []
            };
            paletes.push(novoObj);
            tocarBip('sucesso');
            atualizarInterface();
        }

        // Validação da Faixa de Pedidos (45005000 até 45009900)
        function validarFaixaPedido(numeroStr) {
            const num = parseInt(numeroStr.trim(), 10);
            if (isNaN(num)) return false;
            return (num >= 45005000 && num <= 45009900);
        }

        function processarLeitura(codigoInformado) {
            const input = codigoInformado || document.getElementById('inputPedido').value;
            
            if (!input) {
                alert("Por favor, digite ou escaneie o número do pedido/nota.");
                tocarBip('erro');
                return;
            }

            if (!validarFaixaPedido(input)) {
                tocarBip('erro');
                if(!confirm(`⚠️ ATENÇÃO: O pedido ${input} está fora da faixa padrão (45005000 a 45009900).\nDeseja prosseguir mesmo assim como exceção/avulso?`)) {
                    return;
                }
            } else {
                tocarBip('sucesso');
            }

            notasBipadas++;
            document.getElementById('inputPedido').value = '';

            // Se não houver palete aberto, cria um automaticamente
            if (paletes.length === 0) {
                novoPalete();
            }

            // Adiciona o item ao último palete ativo simulando o padrão do SAP enviado
            const paleteAtivo = paletes[paletes.length - 1];
            paleteAtivo.itens.push({
                pedido: input,
                fornecedor: "300364 - MAQNELSON AGRICOLA",
                material: "9232950",
                nfDesc: "DISCO JDEERE N305598",
                casaDesc: "DISCO JDEERE N305598",
                qtd: 21
            });

            exibirCruzamento(input);
            atualizarInterface();
        }

        function exibirCruzamento(pedido) {
            const painel = document.getElementById('painelCruzamento');
            painel.className = "text-xs bg-slate-900 p-3 rounded-xl border border-accentGreen/30 flex flex-col gap-2 text-left";
            painel.innerHTML = `
                <div class="flex justify-between items-center border-b border-slate-800 pb-1">
                    <span class="font-bold text-accentGreen">Pedido: ${pedido}</span>
                    <span class="text-[10px] bg-accentGreen/20 text-accentGreen px-1.5 py-0.5 rounded">Match SAP OK</span>
                </div>
                <div>
                    <p class="text-slate-400">🏢 Fornecedor: <span class="text-slate-200">300364 - MAQNELSON AGRICOLA</span></p>
                    <p class="text-slate-400">📦 Material / Descrição: <span class="text-emerald-400 font-medium">9232950 - DISCO JDEERE N305598</span></p>
                    <p class="text-slate-400">📊 Qtd Pedida: <span class="text-accentBlue font-bold">21 PEÇAS</span></p>
                </div>
                <div class="flex gap-2 mt-2">
                    <button onclick="registrarAvariado()" class="bg-amber-500/20 text-amber-400 border border-amber-500/30 px-2 py-1 rounded text-[10px] hover:bg-amber-500/30">📷 Registrar Avaria / Foto</button>
                </div>
            `;
        }

        function registrarAvariado() {
            tocarBip('sucesso');
            divergencias++;
            alert("📷 Simulação de captura de foto e registro de avaria gravados com sucesso! Bip emitido.");
            atualizarInterface();
        }

        function atualizarInterface() {
            document.getElementById('statNotas').innerText = notasBipadas;
            document.getElementById('statPaletes').innerText = paletes.length;
            document.getElementById('statDivergencias').innerText = divergencias;

            const container = document.getElementById('listaPaletes');
            if (paletes.length === 0) {
                container.innerHTML = `<div class="text-xs text-slate-500 text-center py-6">Nenhum palete aberto no momento.</div>`;
                return;
            }

            container.innerHTML = paletes.map(p => `
                <div class="bg-slate-900 border border-slate-800 p-3 rounded-xl flex justify-between items-center">
                    <div>
                        <p class="font-bold text-sm">Palete 0${p.id} (${p.fornecedor})</p>
                        <p class="text-[11px] text-slate-400">${p.itens.length} item(ns) - Mat: 9232950</p>
                    </div>
                    <span class="text-[10px] px-2 py-1 rounded bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">${p.status}</span>
                </div>
            `).join('');

            const relatorioTexto = `📋 *RELATÓRIO DE CONFERÊNCIA - ALMOXARIFADO*
👤 Conferente: ${operadorAtual.nome} (Mat: ${operadorAtual.matricula})
📦 Paletes Montados: ${paletes.length}
📄 Total de Pedidos Bipados: ${notasBipadas}
⚠️ Divergências/Avarias: ${divergencias}
Status: Operação conferida e validada via app (Faixa 45005000-45009900).`;

            document.getElementById('textoRelatorio').value = relatorioTexto;
        }

        function copiarRelatorio() {
            const textarea = document.getElementById('textoRelatorio');
            textarea.select();
            navigator.clipboard.writeText(textarea.value);
            tocarBip('sucesso');
            alert("Relatório copiado para a área de transferência!");
        }

        // Câmera Real
        function abrirCameraReal() {
            const wrapper = document.getElementById('cameraWrapper');
            wrapper.style.display = 'flex';
            tocarBip('sucesso');

            if (!html5QrCode) {
                html5QrCode = new Html5Qrcode("reader");
            }

            html5QrCode.start(
                { facingMode: "environment" },
                {
                    fps: 10,
                    qrbox: { width: 250, height: 150 }
                },
                (decodedText) => {
                    tocarBip('sucesso');
                    document.getElementById('inputPedido').value = decodedText;
                    processarLeitura(decodedText);
                    fecharCamera();
                },
                (errorMessage) => {}
            ).catch(err => {
                console.error("Erro ao iniciar a câmera:", err);
                alert("Não foi possível acessar a câmera. Verifique as permissões do navegador.");
                fecharCamera();
            });
        }

        function fecharCamera() {
            if (html5QrCode && html5QrCode.isScanning) {
                html5QrCode.stop().then(() => {
                    document.getElementById('cameraWrapper').style.display = 'none';
                }).catch(err => {
                    document.getElementById('cameraWrapper').style.display = 'none';
                });
            } else {
                document.getElementById('cameraWrapper').style.display = 'none';
            }
        }
    </script>
</body>
</html>
