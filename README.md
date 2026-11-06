<!DOCTYPE html>
<html lang="pt-BR" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Recepção Ágil</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkbg: '#090d16',
                        cardbg: '#131c31',
                        cardhover: '#1e293b',
                        accent: '#3b82f6',
                        accenthover: '#2563eb'
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome para ícones -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-darkbg text-slate-100 h-full flex flex-col antialiased selection:bg-blue-500 selection:text-white">

    <!-- Toast / Notificação flutuante -->
    <div id="toast" class="fixed top-5 left-1/2 -translate-x-1/2 z-50 transition-all duration-300 opacity-0 pointer-events-none transform -translate-y-4">
        <div id="toast-content" class="flex items-center gap-3 px-4 py-3 rounded-xl shadow-2xl border font-medium text-sm">
            <i id="toast-icon" class="text-lg"></i>
            <span id="toast-message">Mensagem de alerta</span>
        </div>
    </div>

    <!-- Cabeçalho Fixo -->
    <header class="bg-cardbg/90 backdrop-blur border-b border-slate-800/80 sticky top-0 z-40 px-4 py-3 flex items-center justify-between">
        <div class="flex items-center gap-3">
            <div class="bg-blue-600/20 text-blue-400 p-2 rounded-xl border border-blue-500/30">
                <i class="fa-solid fa-boxes-stacked text-lg"></i>
            </div>
            <div>
                <h1 class="font-bold text-base leading-tight tracking-tight">Recepção Ágil</h1>
                <p class="text-xs text-slate-400">Almoxarifado & Logística</p>
            </div>
        </div>
        <div class="flex items-center gap-2">
            <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span>
                Operacional
            </span>
        </div>
    </header>

    <!-- Conteúdo Dinâmico (Abas) -->
    <main class="flex-1 overflow-y-auto pb-28 px-4 pt-4 max-w-md mx-auto w-full">

        <!-- TAB 1: DASHBOARD -->
        <section id="tab-dashboard" class="space-y-5">
            <!-- Boas-vindas -->
            <div class="bg-gradient-to-r from-blue-950/60 to-slate-900/60 p-4 rounded-2xl border border-blue-800/30 flex items-center justify-between shadow-lg">
                <div>
                    <p class="text-xs text-slate-400">Turno da Manhã • Doca 04</p>
                    <h2 class="text-lg font-bold text-slate-100">Painel de Recepção</h2>
                </div>
                <div class="text-right">
                    <span id="current-time" class="text-xs bg-slate-800/80 px-2.5 py-1 rounded-lg border border-slate-700 text-slate-300 font-mono">08:00</span>
                </div>
            </div>

            <!-- Indicadores Rápidos -->
            <div class="grid grid-cols-3 gap-3">
                <div class="bg-cardbg p-3.5 rounded-2xl border border-slate-800/80 flex flex-col items-center text-center shadow-md">
                    <div class="w-8 h-8 rounded-xl bg-blue-500/10 text-blue-400 flex items-center justify-center mb-1.5 border border-blue-500/20">
                        <i class="fa-solid fa-file-invoice text-xs"></i>
                    </div>
                    <span id="stat-nfs" class="text-lg font-bold text-slate-100">18</span>
                    <span class="text-[11px] text-slate-400 font-medium leading-tight">NFs Recebidas</span>
                </div>
                <div class="bg-cardbg p-3.5 rounded-2xl border border-slate-800/80 flex flex-col items-center text-center shadow-md">
                    <div class="w-8 h-8 rounded-xl bg-emerald-500/10 text-emerald-400 flex items-center justify-center mb-1.5 border border-emerald-500/20">
                        <i class="fa-solid fa-pallet text-xs"></i>
                    </div>
                    <span id="stat-paletes" class="text-lg font-bold text-slate-100">7</span>
                    <span class="text-[11px] text-slate-400 font-medium leading-tight">Paletes Ativos</span>
                </div>
                <div class="bg-cardbg p-3.5 rounded-2xl border border-slate-800/80 flex flex-col items-center text-center shadow-md">
                    <div class="w-8 h-8 rounded-xl bg-amber-500/10 text-amber-400 flex items-center justify-center mb-1.5 border border-amber-500/20">
                        <i class="fa-solid fa-triangle-exclamation text-xs"></i>
                    </div>
                    <span id="stat-divergencias" class="text-lg font-bold text-slate-100">1</span>
                    <span class="text-[11px] text-slate-400 font-medium leading-tight">Divergências</span>
                </div>
            </div>

            <!-- Botão Gigante de Escaneamento -->
            <button onclick="switchTab('scanner')" class="w-full group relative overflow-hidden bg-gradient-to-br from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 p-5 rounded-2xl shadow-xl shadow-blue-600/25 border border-blue-400/30 flex flex-col items-center justify-center text-center transition-all duration-200 active:scale-[0.98]">
                <div class="absolute -right-6 -bottom-6 w-28 h-28 bg-white/10 rounded-full blur-xl group-hover:scale-150 transition-transform"></div>
                <div class="w-14 h-14 bg-white/10 rounded-2xl flex items-center justify-center mb-3 backdrop-blur-md border border-white/20 text-white shadow-inner">
                    <i class="fa-solid fa-barcode text-2xl"></i>
                </div>
                <span class="text-lg font-bold text-white tracking-wide">Escanear Nova NF</span>
                <span class="text-xs text-blue-100/80 mt-0.5">Leitura de código de barras ou digitação</span>
            </button>

            <!-- Ações Secundárias / Atalhos -->
            <div class="grid grid-cols-2 gap-3">
                <button onclick="switchTab('paletes')" class="bg-cardbg hover:bg-cardhover p-4 rounded-2xl border border-slate-800 flex items-center gap-3.5 transition-colors text-left group shadow-md">
                    <div class="w-10 h-10 rounded-xl bg-blue-500/10 text-blue-400 flex items-center justify-center border border-blue-500/20 group-hover:scale-105 transition-transform">
                        <i class="fa-solid fa-boxes-packing text-sm"></i>
                    </div>
                    <div>
                        <h3 class="text-sm font-semibold text-slate-200">Paletes do Dia</h3>
                        <p class="text-[11px] text-slate-400">Gerenciar cargas</p>
                    </div>
                </button>
                <button onclick="switchTab('historico')" class="bg-cardbg hover:bg-cardhover p-4 rounded-2xl border border-slate-800 flex items-center gap-3.5 transition-colors text-left group shadow-md">
                    <div class="w-10 h-10 rounded-xl bg-purple-500/10 text-purple-400 flex items-center justify-center border border-purple-500/20 group-hover:scale-105 transition-transform">
                        <i class="fa-solid fa-clock-rotate-left text-sm"></i>
                    </div>
                    <div>
                        <h3 class="text-sm font-semibold text-slate-200">Histórico / Relatório</h3>
                        <p class="text-[11px] text-slate-400">Exportar resumo</p>
                    </div>
                </button>
            </div>

            <!-- Atividade Recente -->
            <div class="bg-cardbg p-4 rounded-2xl border border-slate-800 shadow-md">
                <div class="flex items-center justify-between mb-3">
                    <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Últimas Notas Processadas</h3>
                    <button onclick="switchTab('historico')" class="text-xs text-blue-400 hover:text-blue-300 font-medium">Ver todas</button>
                </div>
                <div id="recent-list" class="space-y-2.5">
                    <!-- Preenchido via JS -->
                </div>
            </div>
        </section>

        <!-- TAB 2: LEITURA E VÍNCULO (SCANNER SIMULADO) -->
        <section id="tab-scanner" class="hidden space-y-4">
            <div class="flex items-center justify-between">
                <button onclick="switchTab('dashboard')" class="text-xs text-slate-400 hover:text-white flex items-center gap-1.5 px-3 py-1.5 bg-cardbg rounded-xl border border-slate-800">
                    <i class="fa-solid fa-arrow-left"></i> Voltar
                </button>
                <h2 class="font-bold text-sm text-slate-200">Leitura de DANFE / NF</h2>
                <div class="w-12"></div>
            </div>

            <!-- Visor de Câmera Simulado -->
            <div class="bg-slate-950 rounded-2xl border border-slate-800 p-6 flex flex-col items-center justify-center relative overflow-hidden min-h-[190px] shadow-inner">
                <div class="absolute inset-0 opacity-20 bg-[radial-gradient(#3b82f6_1px,transparent_1px)] [background-size:16px_16px]"></div>
                <!-- Linha de laser escaneando -->
                <div class="absolute inset-x-0 h-0.5 bg-red-500 shadow-[0_0_12px_#ef4444] animate-bounce top-1/2 -translate-y-1/2"></div>
                
                <div class="w-16 h-16 rounded-2xl bg-blue-600/20 border border-blue-500/30 flex items-center justify-center text-blue-400 mb-3 relative z-10">
                    <i class="fa-solid fa-camera text-2xl animate-pulse"></i>
                </div>
                <p class="text-xs text-slate-300 font-medium relative z-10 text-center">Câmera ativa para bipagem de DANFE</p>
                <p class="text-[10px] text-slate-500 mt-1 relative z-10">Utilize os atalhos abaixo para simular recebimento</p>
            </div>

            <!-- Formulário de Vínculo da NF -->
            <div class="bg-cardbg p-4 rounded-2xl border border-slate-800 space-y-4 shadow-md">
                <div>
                    <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1.5">Número da NF / Chave de Acesso</label>
                    <div class="flex gap-2">
                        <input type="text" id="input-nf-num" placeholder="Ex: NF-84920" class="flex-1 bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-slate-100 placeholder:text-slate-600 focus:outline-none focus:border-blue-500 font-mono">
                        <button onclick="preencherAutomaticoSimulado()" class="bg-slate-800 hover:bg-slate-700 text-slate-200 px-3 py-2.5 rounded-xl text-xs font-semibold border border-slate-700 transition-colors">
                            Auto
                        </button>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1.5">Fornecedor</label>
                    <input type="text" id="input-fornecedor" placeholder="Nome do Fornecedor / Transportadora" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-slate-100 placeholder:text-slate-600 focus:outline-none focus:border-blue-500">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1.5">Nº Pedido (SAP)</label>
                        <input type="text" id="input-pedido" placeholder="Ex: PED-90123" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-slate-100 placeholder:text-slate-600 focus:outline-none focus:border-blue-500 font-mono">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1.5">Qtd Volumes</label>
                        <input type="number" id="input-volumes" value="10" min="1" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-slate-100 focus:outline-none focus:border-blue-500 font-mono">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1.5">Selecione o Palete de Destino</label>
                    <select id="select-palete-destino" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2.5 text-sm text-slate-100 focus:outline-none focus:border-blue-500 font-medium">
                        <option value="PAL-01">PAL-01 (Setor A - Docas)</option>
                        <option value="PAL-02">PAL-02 (Setor B - Matéria Prima)</option>
                        <option value="PAL-03">PAL-03 (Setor A - Docas)</option>
                        <option value="PAL-04">PAL-04 (Setor C - Químicos)</option>
                        <option value="NOVO">+ Criar Novo Palete</option>
                    </select>
                </div>

                <button onclick="confirmarRecebimentoNF()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-semibold py-3.5 rounded-xl transition-colors shadow-lg shadow-emerald-600/25 flex items-center justify-center gap-2 text-sm">
                    <i class="fa-solid fa-check"></i> Registrar Nota & Alocar no Palete
                </button>

                <!-- Simulações Rápidas -->
                <div class="pt-3 border-t border-slate-800 flex flex-wrap gap-2">
                    <span class="text-[11px] text-slate-400 w-full mb-0.5">Teste rápido de leitura:</span>
                    <button onclick="carregarExemploLeitura('NF-84920', 'Metalúrgica Santa Rita', 'PED-90123', 14)" class="text-xs bg-slate-900 hover:bg-slate-800 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700 transition-colors">
                        📦 Santa Rita (14 vols)
                    </button>
                    <button onclick="carregarExemploLeitura('NF-39102', 'Distribuidora Global', 'PED-90124', 8)" class="text-xs bg-slate-900 hover:bg-slate-800 text-slate-200 px-3 py-1.5 rounded-lg border border-slate-700 transition-colors">
                        📦 Global (8 vols)
                    </button>
                </div>
            </div>
        </section>

        <!-- TAB 3: GESTÃO DE PALETES E CARGAS -->
        <section id="tab-paletes" class="hidden space-y-4">
            <div class="flex items-center justify-between">
                <button onclick="switchTab('dashboard')" class="text-xs text-slate-400 hover:text-white flex items-center gap-1.5 px-3 py-1.5 bg-cardbg rounded-xl border border-slate-800">
                    <i class="fa-solid fa-arrow-left"></i> Voltar
                </button>
                <h2 class="font-bold text-sm text-slate-200">Gestão de Paletes</h2>
                <button onclick="abrirNovoPaleteModal()" class="text-xs text-blue-400 hover:text-blue-300 font-medium px-3 py-1.5 bg-blue-500/10 rounded-xl border border-blue-500/20 flex items-center gap-1">
                    <i class="fa-solid fa-plus"></i> Novo
                </button>
            </div>

            <!-- Filtros de Status -->
            <div class="flex gap-2 overflow-x-auto pb-1">
                <button onclick="filtrarPaletes('todos')" class="px-3 py-1.5 rounded-xl bg-blue-600 text-white text-xs font-semibold whitespace-nowrap shadow-md shadow-blue-600/20">Todos (7)</button>
                <button onclick="filtrarPaletes('conferencia')" class="px-3 py-1.5 rounded-xl bg-cardbg border border-slate-800 text-slate-300 text-xs font-medium whitespace-nowrap hover:bg-cardhover">🟡 Em Conferência (2)</button>
                <button onclick="filtrarPaletes('concluido')" class="px-3 py-1.5 rounded-xl bg-cardbg border border-slate-800 text-slate-300 text-xs font-medium whitespace-nowrap hover:bg-cardhover">🟢 Conferidos (4)</button>
                <button onclick="filtrarPaletes('divergencia')" class="px-3 py-1.5 rounded-xl bg-cardbg border border-slate-800 text-slate-300 text-xs font-medium whitespace-nowrap hover:bg-cardhover">🔴 Divergência (1)</button>
            </div>

            <!-- Lista de Paletes -->
            <div id="paletes-lista-container" class="space-y-3">
                <!-- Preenchido via JS -->
            </div>
        </section>

        <!-- TAB 4: CONFERÊNCIA DE ITENS DO PALETE -->
        <section id="tab-conferencia" class="hidden space-y-4">
            <div class="flex items-center justify-between">
                <button onclick="switchTab('paletes')" class="text-xs text-slate-400 hover:text-white flex items-center gap-1.5 px-3 py-1.5 bg-cardbg rounded-xl border border-slate-800">
                    <i class="fa-solid fa-arrow-left"></i> Voltar
                </button>
                <h2 id="conf-palete-titulo" class="font-bold text-sm text-slate-200">Conferência - PAL-01</h2>
                <div class="w-12"></div>
            </div>

            <!-- Resumo do Palete Selecionado -->
            <div class="bg-cardbg p-4 rounded-2xl border border-slate-800 space-y-3 shadow-md">
                <div class="flex items-center justify-between">
                    <span id="conf-setor" class="text-xs font-mono bg-blue-500/10 text-blue-400 px-2.5 py-1 rounded-lg border border-blue-500/20 font-semibold">Setor A - Docas</span>
                    <span id="conf-status-badge" class="text-xs font-medium px-2.5 py-1 rounded-lg bg-amber-500/10 text-amber-400 border border-amber-500/20">🟡 Em conferência</span>
                </div>
                <div>
                    <h3 class="text-xs text-slate-400 uppercase tracking-wider font-semibold">Notas Vinculadas:</h3>
                    <p id="conf-nfs-lista" class="text-sm font-bold text-slate-100 mt-0.5">NF-84920, NF-84921</p>
                </div>
            </div>

            <!-- Lista de Itens para Conferência Física -->
            <div class="space-y-3">
                <div class="flex items-center justify-between px-1">
                    <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Itens e Volumes do Palete</h3>
                    <button onclick="adicionarItemConferenciaPrompt()" class="text-xs text-blue-400 hover:text-blue-300 font-medium flex items-center gap-1">
                        <i class="fa-solid fa-plus-circle"></i> Adicionar Item
                    </button>
                </div>
                <div id="itens-conferencia-lista" class="space-y-2.5">
                    <!-- Preenchido via JS -->
                </div>
            </div>

            <button onclick="finalizarConferenciaPalete()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-semibold py-3.5 rounded-xl transition-colors shadow-lg shadow-emerald-600/25 flex items-center justify-center gap-2 text-sm mt-4">
                <i class="fa-solid fa-clipboard-check"></i> Concluir Conferência & Liberar Carga
            </button>
        </section>

        <!-- TAB 5: HISTÓRICO E RELATÓRIO SIMPLES -->
        <section id="tab-historico" class="hidden space-y-4">
            <div class="flex items-center justify-between">
                <button onclick="switchTab('dashboard')" class="text-xs text-slate-400 hover:text-white flex items-center gap-1.5 px-3 py-1.5 bg-cardbg rounded-xl border border-slate-800">
                    <i class="fa-solid fa-arrow-left"></i> Voltar
                </button>
                <h2 class="font-bold text-sm text-slate-200">Relatório do Turno</h2>
                <div class="w-12"></div>
            </div>

            <!-- Card de Ações de Cópia / Relatório -->
            <div class="bg-gradient-to-br from-blue-950/40 to-slate-900/60 p-4 rounded-2xl border border-blue-800/30 space-y-3 shadow-md">
                <div class="flex items-center justify-between">
                    <div>
                        <h3 class="font-bold text-sm text-slate-100">Resumo Estruturado</h3>
                        <p class="text-xs text-slate-400">Pronto para envio por WhatsApp ou e-mail</p>
                    </div>
                    <button onclick="copiarRelatorioTexto()" class="bg-blue-600 hover:bg-blue-500 text-white px-3.5 py-2 rounded-xl text-xs font-semibold transition-colors shadow-lg shadow-blue-600/20 flex items-center gap-1.5">
                        <i class="fa-solid fa-copy"></i> Copiar Relatório
                    </button>
                </div>
                <!-- Caixa de Texto Estilizada do Relatório -->
                <div class="bg-slate-950 p-3 rounded-xl border border-slate-800 font-mono text-[11px] text-slate-300 leading-relaxed max-h-48 overflow-y-auto select-all" id="relatorio-texto-box">
                    <!-- Gerado via JS -->
                </div>
            </div>

            <!-- Lista Completa de Histórico -->
            <div class="bg-cardbg p-4 rounded-2xl border border-slate-800 space-y-3 shadow-md">
                <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Notas Registradas no Turno</h3>
                <div id="historico-completo-lista" class="space-y-2.5">
                    <!-- Preenchido via JS -->
                </div>
            </div>
        </section>

    </main>

    <!-- Modal Novo Palete -->
    <div id="modal-novo-palete" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-cardbg border border-slate-800 rounded-2xl w-full max-w-sm p-5 space-y-4 shadow-2xl">
            <div class="flex items-center justify-between">
                <h3 class="font-bold text-sm text-slate-100 flex items-center gap-2">
                    <i class="fa-solid fa-pallet text-blue-400"></i> Criar Novo Palete
                </h3>
                <button onclick="fecharNovoPaleteModal()" class="text-slate-400 hover:text-white">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Identificação do Palete</label>
                    <input type="text" id="novo-palete-id" value="PAL-08" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2 text-sm text-slate-100 focus:outline-none focus:border-blue-500 font-mono">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Setor de Destino</label>
                    <select id="novo-palete-setor" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3.5 py-2 text-sm text-slate-100 focus:outline-none focus:border-blue-500">
                        <option value="Setor A - Docas">Setor A - Docas</option>
                        <option value="Setor B - Matéria Prima">Setor B - Matéria Prima</option>
                        <option value="Setor C - Químicos">Setor C - Químicos</option>
                    </select>
                </div>
                <button onclick="salvarNovoPalete()" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-semibold py-2.5 rounded-xl text-sm transition-colors shadow-lg shadow-blue-600/20">
                    Criar Palete
                </button>
            </div>
        </div>
    </div>

    <!-- Navegação Inferior (Navbar Mobile) -->
    <nav class="fixed bottom-0 inset-x-0 bg-cardbg/95 backdrop-blur border-t border-slate-800 py-2.5 px-4 flex justify-around items-center z-40 max-w-md mx-auto shadow-2xl">
        <button onclick="switchTab('dashboard')" id="nav-dashboard" class="flex flex-col items-center gap-1 text-blue-400 transition-colors">
            <i class="fa-solid fa-house text-base"></i>
            <span class="text-[10px] font-medium">Início</span>
        </button>
        <button onclick="switchTab('scanner')" id="nav-scanner" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-200 transition-colors">
            <i class="fa-solid fa-barcode text-base"></i>
            <span class="text-[10px] font-medium">Escanear</span>
        </button>
        <button onclick="switchTab('paletes')" id="nav-paletes" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-200 transition-colors">
            <i class="fa-solid fa-pallet text-base"></i>
            <span class="text-[10px] font-medium">Paletes</span>
        </button>
        <button onclick="switchTab('historico')" id="nav-historico" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-200 transition-colors">
            <i class="fa-solid fa-clock-rotate-left text-base"></i>
            <span class="text-[10px] font-medium">Relatório</span>
        </button>
    </nav>

    <!-- JavaScript da Aplicação -->
    <script>
        // Dados centrais da aplicação
        let paletesData = [
            { 
                id: 'PAL-01', 
                setor: 'Setor A - Docas', 
                status: 'conferencia', 
                statusTexto: '🟡 Em conferência', 
                nfs: ['NF-84920', 'NF-84921'], 
                volumes: 20,
                itens: [
                    { desc: 'Chapa de Aço 3mm (Cx 10)', esperado: 10, conferido: 10, status: 'ok' },
                    { desc: 'Perfil U Alumínio 6m', esperado: 10, conferido: 8, status: 'divergencia', obs: 'Faltaram 2 unidades' }
                ]
            },
            { 
                id: 'PAL-02', 
                setor: 'Setor B - Matéria Prima', 
                status: 'concluido', 
                statusTexto: '🟢 Conferido', 
                nfs: ['NF-39102'], 
                volumes: 8,
                itens: [
                    { desc: 'Bobina Cobre 1/2 Pol', esperado: 8, conferido: 8, status: 'ok' }
                ]
            },
            { 
                id: 'PAL-03', 
                setor: 'Setor A - Docas', 
                status: 'concluido', 
                statusTexto: '🟢 Conferido', 
                nfs: ['NF-38910'], 
                volumes: 32,
                itens: [
                    { desc: 'Parafuso Sextavado M8', esperado: 32, conferido: 32, status: 'ok' }
                ]
            },
            { 
                id: 'PAL-04', 
                setor: 'Setor C - Químicos', 
                status: 'divergencia', 
                statusTexto: '🔴 Divergência', 
                nfs: ['NF-10492'], 
                volumes: 22,
                itens: [
                    { desc: 'Lubrificante Industrial 20L', esperado: 22, conferido: 20, status: 'avaria', obs: '2 bombonas amassadas/vazando' }
                ]
            },
            { 
                id: 'PAL-05', 
                setor: 'Setor B - Matéria Prima', 
                status: 'concluido', 
                statusTexto: '🟢 Conferido', 
                nfs: ['NF-12893'], 
                volumes: 15,
                itens: [
                    { desc: 'Conexão Latão 1 Pol', esperado: 15, conferido: 15, status: 'ok' }
                ]
            },
            { 
                id: 'PAL-06', 
                setor: 'Setor A - Docas', 
                status: 'conferencia', 
                statusTexto: '🟡 Em conferência', 
                nfs: ['NF-55829'], 
                volumes: 10,
                itens: [
                    { desc: 'Correia Industrial A-40', esperado: 10, conferido: 10, status: 'ok' }
                ]
            },
            { 
                id: 'PAL-07', 
                setor: 'Setor C - Químicos', 
                status: 'concluido', 
                statusTexto: '🟢 Conferido', 
                nfs: ['NF-77492'], 
                volumes: 18,
                itens: [
                    { desc: 'Selante PU Alta Fixação', esperado: 18, conferido: 18, status: 'ok' }
                ]
            }
        ];

        let historicoNFs = [
            { nf: 'NF-84920', fornecedor: 'Metalúrgica Santa Rita', pedido: 'PED-90123', volumes: 14, hora: '08:42', palete: 'PAL-01' },
            { nf: 'NF-39102', fornecedor: 'Distribuidora Global', pedido: 'PED-90124', volumes: 8, hora: '08:15', palete: 'PAL-02' },
            { nf: 'NF-10492', fornecedor: 'Componentes do Brasil', pedido: 'PED-90125', volumes: 22, hora: '07:50', palete: 'PAL-04' }
        ];

        let paleteEmConferenciaId = 'PAL-01';

        // Inicialização
        window.onload = function() {
            atualizarRelogio();
            setInterval(atualizarRelogio, 10000);
            renderizarDashboard();
            renderizarPaletes('todos');
            renderizarHistorico();
            atualizarTextoRelatorio();
        };

        function atualizarRelogio() {
            const agora = new Date();
            const horaStr = agora.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
            document.getElementById('current-time').innerText = horaStr;
        }

        // Sistema de Troca de Abas
        function switchTab(tabId) {
            ['dashboard', 'scanner', 'paletes', 'conferencia', 'historico'].forEach(t => {
                document.getElementById(`tab-${t}`).classList.add('hidden');
            });
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            // Atualizar navegação inferior
            ['dashboard', 'scanner', 'paletes', 'historico'].forEach(n => {
                const el = document.getElementById(`nav-${n}`);
                if(el) {
                    el.classList.remove('text-blue-400');
                    el.classList.add('text-slate-400');
                }
            });
            if(tabId !== 'conferencia') {
                const navEl = document.getElementById(`nav-${tabId}`);
                if(navEl) {
                    navEl.classList.remove('text-slate-400');
                    navEl.classList.add('text-blue-400');
                }
            }

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Sistema de Toast
        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const content = document.getElementById('toast-content');
            const icon = document.getElementById('toast-icon');
            const msg = document.getElementById('toast-message');

            msg.innerText = message;
            
            if(type === 'success') {
                content.className = "flex items-center gap-3 px-4 py-3 rounded-xl shadow-2xl border font-medium text-sm bg-emerald-950/95 text-emerald-200 border-emerald-500/40 backdrop-blur";
                icon.className = "fa-solid fa-circle-check text-emerald-400 text-lg";
            } else if(type === 'error') {
                content.className = "flex items-center gap-3 px-4 py-3 rounded-xl shadow-2xl border font-medium text-sm bg-red-950/95 text-red-200 border-red-500/40 backdrop-blur";
                icon.className = "fa-solid fa-circle-exclamation text-red-400 text-lg";
            }

            toast.classList.remove('opacity-0', 'pointer-events-none', '-translate-y-4');
            toast.classList.add('opacity-100', 'translate-y-0');

            setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'pointer-events-none', '-translate-y-4');
            }, 3000);
        }

        // Renderizar Dashboard
        function renderizarDashboard() {
            document.getElementById('stat-nfs').innerText = historicoNFs.length + 15;
            document.getElementById('stat-paletes').innerText = paletesData.length;
            
            const divCount = paletesData.filter(p => p.status === 'divergencia').length;
            document.getElementById('stat-divergencias').innerText = divCount;

            const container = document.getElementById('recent-list');
            container.innerHTML = '';
            
            historicoNFs.slice(0, 3).forEach(item => {
                container.innerHTML += `
                    <div class="bg-slate-900/60 p-3 rounded-xl border border-slate-800/80 flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div class="w-8 h-8 rounded-lg bg-blue-500/10 text-blue-400 flex items-center justify-center font-bold text-xs">
                                <i class="fa-solid fa-file-lines"></i>
                            </div>
                            <div>
                                <h4 class="text-xs font-bold text-slate-200">${item.nf} - ${item.fornecedor}</h4>
                                <p class="text-[10px] text-slate-400">${item.volumes} vols • Palete: <span class="text-blue-400 font-medium">${item.palete}</span></p>
                            </div>
                        </div>
                        <span class="text-[10px] text-slate-500 font-mono">${item.hora}</span>
                    </div>
                `;
            });
        }

        // Simulador de Leitura de NF
        function carregarExemploLeitura(nf, fornecedor, pedido, volumes) {
            document.getElementById('input-nf-num').value = nf;
            document.getElementById('input-fornecedor').value = fornecedor;
            document.getElementById('input-pedido').value = pedido;
            document.getElementById('input-volumes').value = volumes;
            showToast(`Dados da ${nf} carregados no leitor!`);
        }

        function preencherAutomaticoSimulado() {
            const randomNum = Math.floor(10000 + Math.random() * 90000);
            document.getElementById('input-nf-num').value = `NF-${randomNum}`;
            document.getElementById('input-fornecedor').value = 'Indústria Mecânica Nacional';
            document.getElementById('input-pedido').value = `PED-90${Math.floor(100 + Math.random() * 900)}`;
            document.getElementById('input-volumes').value = Math.floor(5 + Math.random() * 20);
            showToast('Nota gerada automaticamente!');
        }

        function confirmarRecebimentoNF() {
            const nf = document.getElementById('input-nf-num').value.trim();
            const fornecedor = document.getElementById('input-fornecedor').value.trim();
            const pedido = document.getElementById('input-pedido').value.trim();
            const volumes = parseInt(document.getElementById('input-volumes').value) || 1;
            let paleteDestino = document.getElementById('select-palete-destino').value;

            if(!nf || !fornecedor) {
                showToast('Preencha o número da NF e o Fornecedor!', 'error');
                return;
            }

            if(paleteDestino === 'NOVO') {
                paleteDestino = `PAL-0${paletesData.length + 1}`;
                paletesData.push({
                    id: paleteDestino,
                    setor: 'Setor A - Docas',
                    status: 'conferencia',
                    statusTexto: '🟡 Em conferência',
                    nfs: [nf],
                    volumes: volumes,
                    itens: [
                        { desc: `Volumes Diversos (${fornecedor})`, esperado: volumes, conferido: volumes, status: 'ok' }
                    ]
                });
            } else {
                const p = paletesData.find(item => item.id === paleteDestino);
                if(p) {
                    if(!p.nfs.includes(nf)) p.nfs.push(nf);
                    p.volumes += volumes;
                    p.itens.push({ desc: `Carga NF ${nf} (${fornecedor})`, esperado: volumes, conferido: volumes, status: 'ok' });
                }
            }

            // Adicionar ao histórico
            historicoNFs.unshift({
                nf: nf,
                fornecedor: fornecedor,
                pedido: pedido || 'PED-99999',
                volumes: volumes,
                hora: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                palete: paleteDestino
            });

            renderizarDashboard();
            renderizarPaletes('todos');
            renderizarHistorico();
            atualizarTextoRelatorio();

            showToast(`Nota ${nf} vinculada ao ${paleteDestino} com sucesso!`);
            switchTab('paletes');
        }

        // Renderizar Paletes
        function renderizarPaletes(filtro) {
            const container = document.getElementById('paletes-lista-container');
            container.innerHTML = '';

            let dadosFiltrados = paletesData;
            if(filtro !== 'todos') {
                dadosFiltrados = paletesData.filter(p => p.status === filtro);
            }

            dadosFiltrados.forEach(p => {
                container.innerHTML += `
                    <div class="bg-cardbg p-4 rounded-2xl border border-slate-800 space-y-3 shadow-md">
                        <div class="flex items-center justify-between">
                            <div class="flex items-center gap-2.5">
                                <div class="w-9 h-9 rounded-xl bg-slate-900 text-blue-400 flex items-center justify-center border border-slate-700">
                                    <i class="fa-solid fa-pallet text-sm"></i>
                                </div>
                                <div>
                                    <h3 class="font-bold text-sm text-slate-100">${p.id}</h3>
                                    <p class="text-[11px] text-slate-400">${p.setor}</p>
                                </div>
                            </div>
                            <span class="text-xs font-medium px-2.5 py-1 rounded-lg bg-slate-900 border border-slate-700">${p.statusTexto}</span>
                        </div>
                        <div class="pt-2 border-t border-slate-800/80 flex items-center justify-between text-xs">
                            <span class="text-slate-400">NFs: <span class="text-slate-200 font-medium">${p.nfs.join(', ')}</span></span>
                            <span class="text-slate-400"><i class="fa-solid fa-box-open mr-1"></i> ${p.volumes} vols</span>
                        </div>
                        <div class="pt-1 flex gap-2">
                            <button onclick="abrirConferenciaPalete('${p.id}')" class="flex-1 bg-blue-600/20 hover:bg-blue-600/30 text-blue-400 border border-blue-500/30 py-2 rounded-xl font-semibold text-xs transition-colors flex items-center justify-center gap-1.5">
                                <i class="fa-solid fa-clipboard-list"></i> Conferir Itens
                            </button>
                        </div>
                    </div>
                `;
            });
        }

        function filtrarPaletes(tipo) {
            renderizarPaletes(tipo);
        }

        // Gestão de Novo Palete Modal
        function abrirNovoPaleteModal() {
            document.getElementById('modal-novo-palete').classList.remove('hidden');
        }

        function fecharNovoPaleteModal() {
            document.getElementById('modal-novo-palete').classList.add('hidden');
        }

        function salvarNovoPalete() {
            const id = document.getElementById('novo-palete-id').value.trim() || `PAL-0${paletesData.length + 1}`;
            const setor = document.getElementById('novo-palete-setor').value;

            if(paletesData.some(p => p.id === id)) {
                showToast('Já existe um palete com este identificador!', 'error');
                return;
            }

            paletesData.unshift({
                id: id,
                setor: setor,
                status: 'conferencia',
                statusTexto: '🟡 Em conferência',
                nfs: [],
                volumes: 0,
                itens: []
            });

            fecharNovoPaleteModal();
            renderizarPaletes('todos');
            showToast(`Palete ${id} criado com sucesso!`);
        }

        // Módulo de Conferência Física de Itens
        function abrirConferenciaPalete(paleteId) {
            paleteEmConferenciaId = paleteId;
            const palete = paletesData.find(p => p.id === paleteId);
            if(!palete) return;

            document.getElementById('conf-palete-titulo').innerText = `Conferência - ${palete.id}`;
            document.getElementById('conf-setor').innerText = palete.setor;
            document.getElementById('conf-status-badge').innerText = palete.statusTexto;
            document.getElementById('conf-nfs-lista').innerText = palete.nfs.length > 0 ? palete.nfs.join(', ') : 'Nenhuma NF vinculada';

            renderizarItensConferencia();
            switchTab('conferencia');
        }

        function renderizarItensConferencia() {
            const palete = paletesData.find(p => p.id === paleteEmConferenciaId);
            const container = document.getElementById('itens-conferencia-lista');
            container.innerHTML = '';

            if(!palete || palete.itens.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-500 text-center py-4 bg-cardbg rounded-xl border border-slate-800">Nenhum item cadastrado neste palete.</p>`;
                return;
            }

            palete.itens.forEach((item, index) => {
                let badgeColor = 'bg-emerald-500/10 text-emerald-400 border-emerald-500/20';
                if(item.status === 'divergencia') badgeColor = 'bg-amber-500/10 text-amber-400 border-amber-500/20';
                if(item.status === 'avaria') badgeColor = 'bg-red-500/10 text-red-400 border-red-500/20';

                container.innerHTML += `
                    <div class="bg-cardbg p-3.5 rounded-xl border border-slate-800 space-y-2 shadow-md">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-bold text-slate-200">${item.desc}</span>
                            <span class="text-[10px] font-medium px-2 py-0.5 rounded-lg border ${badgeColor}">
                                ${item.status.toUpperCase()}
                            </span>
                        </div>
                        <div class="flex items-center justify-between text-xs pt-1 border-t border-slate-800/60">
                            <span class="text-slate-400">Esperado: <strong class="text-slate-200">${item.esperado}</strong> | Conferido: <strong class="text-blue-400">${item.conferido}</strong></span>
                            <div class="flex gap-1.5">
                                <button onclick="alterarStatusItem('${index}', 'ok')" class="px-2 py-1 rounded bg-emerald-600/20 text-emerald-400 hover:bg-emerald-600/30 text-[10px] font-semibold">OK</button>
                                <button onclick="alterarStatusItem('${index}', 'divergencia')" class="px-2 py-1 rounded bg-amber-600/20 text-amber-400 hover:bg-amber-600/30 text-[10px] font-semibold">Divergência</button>
                                <button onclick="alterarStatusItem('${index}', 'avaria')" class="px-2 py-1 rounded bg-red-600/20 text-red-400 hover:bg-red-600/30 text-[10px] font-semibold">Avaria</button>
                            </div>
                        </div>
                        ${item.obs ? `<p class="text-[11px] text-amber-300 bg-amber-950/30 px-2 py-1 rounded border border-amber-500/20">Obs: ${item.obs}</p>` : ''}
                    </div>
                `;
            });
        }

        function alterarStatusItem(index, novoStatus) {
            const palete = paletesData.find(p => p.id === paleteEmConferenciaId);
            if(palete && palete.itens[index]) {
                palete.itens[index].status = novoStatus;
                if(novoStatus === 'divergencia' || novoStatus === 'avaria') {
                    palete.itens[index].obs = prompt('Informe a observação ou quantidade divergente:', 'Divergência apontada na conferência física') || 'Apontamento registrado';
                    palete.status = 'divergencia';
                    palete.statusTexto = '🔴 Divergência';
                } else {
                    palete.itens[index].obs = '';
                    palete.status = 'conferencia';
                    palete.statusTexto = '🟡 Em conferência';
                }
                document.getElementById('conf-status-badge').innerText = palete.statusTexto;
                renderizarItensConferencia();
                renderizarPaletes('todos');
                showToast('Status do item atualizado!');
            }
        }

        function adicionarItemConferenciaPrompt() {
            const desc = prompt('Descrição do novo item/volume:');
            if(!desc) return;
            const qtd = parseInt(prompt('Quantidade esperada:', '1')) || 1;

            const palete = paletesData.find(p => p.id === paleteEmConferenciaId);
            if(palete) {
                palete.itens.push({ desc: desc, esperado: qtd, conferido: qtd, status: 'ok' });
                renderizarItensConferencia();
                showToast('Item adicionado ao palete!');
            }
        }

        function finalizarConferenciaPalete() {
            const palete = paletesData.find(p => p.id === paleteEmConferenciaId);
            if(palete) {
                if(palete.status !== 'divergencia') {
                    palete.status = 'concluido';
                    palete.statusTexto = '🟢 Conferido';
                }
                renderizarPaletes('todos');
                renderizarDashboard();
                showToast(`Palete ${palete.id} concluído com sucesso!`);
                switchTab('paletes');
            }
        }

        // Histórico e Relatório Textual
        function renderizarHistorico() {
            const container = document.getElementById('historico-completo-lista');
            container.innerHTML = '';

            historicoNFs.forEach(item => {
                container.innerHTML += `
                    <div class="bg-slate-900/60 p-3.5 rounded-xl border border-slate-800 flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div class="w-8 h-8 rounded-lg bg-purple-500/10 text-purple-400 flex items-center justify-center font-bold text-xs">
                                <i class="fa-solid fa-file-invoice"></i>
                            </div>
                            <div>
                                <h4 class="text-xs font-bold text-slate-200">${item.nf} - ${item.fornecedor}</h4>
                                <p class="text-[10px] text-slate-400">SAP: ${item.pedido} • ${item.volumes} vols • Palete: <span class="text-blue-400 font-semibold">${item.palete}</span></p>
                            </div>
                        </div>
                        <span class="text-[10px] text-slate-400 font-mono">${item.hora}</span>
                    </div>
                `;
            });
        }

        function atualizarTextoRelatorio() {
            let texto = `📦 *RELATÓRIO DE RECEBIMENTO - ALMOXARIFADO*\n`;
            texto += `⏰ Turno da Manhã | Doca 04\n`;
            texto += `----------------------------------------\n`;
            texto += `📊 *Indicadores Atuais:*\n`;
            texto += `• Total NFs Registradas: ${historicoNFs.length + 15}\n`;
            texto += `• Paletes Ativos: ${paletesData.length}\n`;
            texto += `• Divergências Apontadas: ${paletesData.filter(p => p.status === 'divergencia').length}\n\n`;
            texto += `📋 *Últimas Cargas Processadas:*\n`;
            
            historicoNFs.slice(0, 5).forEach((item, idx) => {
                texto += `${idx + 1}. *${item.nf}* (${item.fornecedor}) - ${item.volumes}vols -> Destino: *${item.palete}*\n`;
            });
            
            texto += `----------------------------------------\n`;
            texto += `_Gerado automaticamente pelo app Recepção Ágil_`;

            document.getElementById('relatorio-texto-box').innerText = texto;
        }

        function copiarRelatorioTexto() {
            const texto = document.getElementById('relatorio-texto-box').innerText;
            if(navigator.clipboard && navigator.clipboard.writeText) {
                navigator.clipboard.writeText(texto).then(() => {
                    showToast('Relatório copiado para a área de transferência!');
                }).catch(() => {
                    copiarFallback(texto);
                });
            } else {
                copiarFallback(texto);
            }
        }

        function copiarFallback(texto) {
            const textarea = document.createElement('textarea');
            textarea.value = texto;
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                showToast('Relatório copiado com sucesso!');
            } catch (err) {
                showToast('Erro ao copiar relatório.', 'error');
            }
            document.body.removeChild(textarea);
        }
    </script>
</body>
</html>
