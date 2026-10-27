# VisionStock.github.io
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WMS Inteligente - Ecossistema Completo com IA, E-mail, Gráficos & PDF</title>
    <!-- Tailwind CSS para Design Moderno -->
    <script src="https://jsdelivr.net"></script>
    <!-- FontAwesome para Ícones -->
    <link rel="stylesheet" href="https://cloudflare.com">
    <!-- Chart.js para Painéis Gráficos Dinâmicos -->
    <script src="https://jsdelivr.net"></script>
    <!-- jsPDF para Exportação de Faturas e Relatórios Reais -->
    <script src="https://cloudflare.com"></script>
</head>
<body class="bg-gray-900 text-gray-100 min-h-screen font-sans antialiased">

    <!-- BARRA TOP: Status de Comunicação com o Cliente -->
    <header class="bg-gray-800 border-b border-gray-700 p-4 sticky top-0 z-50 shadow-md">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="bg-blue-600 p-2.5 rounded-lg text-white animate-pulse">
                    <i class="fa-solid fa-robot text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">WMS <span class="text-blue-500">LogIAn</span></h1>
                    <p class="text-xs text-gray-400">Plataforma Analítica Multimodal</p>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <!-- Botão para Exportar Relatório Geral do Armazém em PDF -->
                <button onclick="exportInventoryToPDF()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3 py-2 rounded-lg text-xs font-semibold flex items-center gap-1.5 transition-colors shadow-sm">
                    <i class="fa-solid fa-file-pdf"></i> Exportar Inventário (PDF)
                </button>

                <!-- Botão de Configuração de API -->
                <button onclick="toggleConfigModal()" class="bg-gray-700 hover:bg-gray-600 text-gray-200 px-3 py-2 rounded-lg text-xs font-semibold border border-gray-600 flex items-center gap-1.5 transition-colors">
                    <i class="fa-solid fa-key text-amber-400"></i> Configurar API IA
                </button>
                
                <!-- Painel de Notificações Ativas ao Cliente -->
                <div class="flex items-center gap-2 bg-gray-900/80 px-4 py-2 rounded-full border border-gray-700 text-sm">
                    <span class="flex h-2 w-2 relative">
                        <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
                        <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
                    </span>
                    <span id="status-comunicacao" class="text-green-400 font-semibold text-xs truncate max-w-[120px]">Sincronizado</span>
                </div>
            </div>
        </div>
    </header>

    <!-- CONTEÚDO PRINCIPAL -->
    <main class="max-w-7xl mx-auto p-4 grid grid-cols-1 lg:grid-cols-12 gap-6 pb-24">
        
        <!-- COLUNA ESQUERDA: Operação, Entrada com IA & Gráficos Analíticos (7 Colunas) -->
        <section class="lg:col-span-7 space-y-6">
            
            <!-- Janela de Entrada de Mercadoria Inteligente (IA) -->
            <div class="bg-gray-800 border border-gray-700 rounded-xl p-5 shadow-sm">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-lg font-bold text-gray-200 flex items-center gap-2">
                        <i class="fa-solid fa-file-invoice text-blue-400"></i> Entrada de Stock via IA
                    </h2>
                    <span class="bg-blue-500/10 text-blue-400 text-xs px-2.5 py-1 rounded-full border border-blue-500/20 font-medium">
                        Smart OCR & Visão
                    </span>
                </div>
                
                <!-- Área de Upload de Imagem/Fatura -->
                <div class="border-2 border-dashed border-gray-600 hover:border-blue-500 rounded-xl p-6 text-center transition-colors cursor-pointer bg-gray-900/50 relative mb-4" id="upload-container">
                    <input type="file" id="file-input" accept="image/*,.pdf" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
                    <div id="upload-prompt" class="space-y-2">
                        <i class="fa-solid fa-cloud-arrow-up text-3xl text-gray-400 mb-1"></i>
                        <p class="text-sm font-medium">Carregar foto do produto, fatura ou nota de compra</p>
                        <p class="text-xs text-gray-500">A IA vai extrair o Nome, Quantidade e sugerir o Stock Mínimo</p>
                    </div>
                    <div id="upload-loading" class="hidden space-y-3 py-2">
                        <i class="fa-solid fa-circle-notch text-3xl text-blue-500 animate-spin"></i>
                        <p class="text-sm text-blue-400 font-medium animate-pulse">Assistente IA a analisar imagem e a extrair dados textuais...</p>
                    </div>
                    <div id="upload-success" class="hidden space-y-2 text-green-400 py-1">
                        <i class="fa-solid fa-circle-check text-3xl"></i>
                        <p class="text-sm font-medium">Leitura Concluída com Sucesso pela IA!</p>
                        <p class="text-xs text-gray-400" id="detected-file-name"></p>
                    </div>
                </div>

                <!-- Formulário Técnico (Preenchido Manualmente ou via IA) -->
                <form id="product-form" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase tracking-wider mb-1">Nome do Produto</label>
                        <input type="text" id="prod-name" required placeholder="Ex: Cabo HDMI 2m" class="w-full bg-gray-900 border border-gray-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-blue-500 transition-colors">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase tracking-wider mb-1">Qtd. Unidades</label>
                        <input type="number" id="prod-qty" required min="1" placeholder="Ex: 50" class="w-full bg-gray-900 border border-gray-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-blue-500 transition-colors">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase tracking-wider mb-1">Stock Mínimo (Alerta)</label>
                        <input type="number" id="prod-min" required min="0" placeholder="Ex: 10" class="w-full bg-gray-900 border border-gray-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-blue-500 transition-colors">
                    </div>
                    <div class="md:col-span-3 flex justify-end gap-2 pt-2">
                        <button type="button" id="btn-clear" class="bg-gray-700 hover:bg-gray-600 text-gray-200 px-4 py-2 rounded-lg text-sm font-medium transition-colors">Limpar</button>
                        <button type="submit" class="bg-blue-600 hover:bg-blue-500 text-white px-5 py-2 rounded-lg text-sm font-semibold transition-colors flex items-center gap-2">
                            <i class="fa-solid fa-plus"></i> Confirmar Entrada no Stock
                        </button>
                    </div>
                </form>
            </div>

            <!-- Painel Gráfico de Rotação e Níveis de Volumetria -->
            <div class="bg-gray-800 border border-gray-700 rounded-xl p-5 shadow-sm">
                <h3 class="text-sm font-bold text-gray-300 flex items-center gap-2 mb-4">
                    <i class="fa-solid fa-chart-bar text-blue-400"></i> Monitor de Volumetria Física dos Produtos (Unidades em Stock)
                </h3>
                <div class="relative h-64 w-full bg-gray-900/40 p-2 rounded-lg border border-gray-700">
                    <canvas id="inventoryChart"></canvas>
                </div>
            </div>

            <!-- Tabela de Inventário Físico Atual -->
            <div class="bg-gray-800 border border-gray-700 rounded-xl shadow-sm overflow-hidden">
                <div class="p-5 border-b border-gray-700 flex justify-between items-center">
                    <h2 class="text-lg font-bold text-gray-200 flex items-center gap-2">
                        <i class="fa-solid fa-boxes-stacked text-amber-400"></i> Inventário Físico Realizado
                    </h2>
                    <span id="total-itens" class="bg-gray-900 text-gray-400 text-xs font-semibold px-2.5 py-1 rounded-full border border-gray-700">3 Itens Diferentes</span>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse text-sm">
                        <thead>
                            <tr class="bg-gray-900/60 text-gray-400 font-semibold border-b border-gray-700">
                                <th class="p-4">ID</th>
                                <th class="p-4">Produto</th>
                                <th class="p-4 text-center">Unidades Atuais</th>
                                <th class="p-4 text-center">Nível Mínimo</th>
                                <th class="p-4">Estado de Segurança</th>
                                <th class="p-4 text-center">Ações</th>
                            </tr>
                        </thead>
                        <tbody id="inventory-table-body" class="divide-y divide-gray-700/50">
