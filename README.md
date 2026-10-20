<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Diário & Relatório Infantil</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            blue: '#2563eb',   /* Azul Principal */
                            blueDark: '#1d4ed8',
                            blueLight: '#dbeafe',
                            pink: '#ec4899',   /* Rosa Principal */
                            pinkHover: '#db2777',
                            pinkLight: '#fce7f3'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @media print {
            body * { visibility: hidden; }
            #printableReport, #printableReport * { visibility: visible; }
            #printableReport { position: absolute; left: 0; top: 0; width: 100%; }
            .no-print { display: none !important; }
        }
    </style>
</head>
<body class="bg-blue-50/40 text-slate-800 min-h-screen flex flex-col font-sans">

    <!-- Cabeçalho em Azul com Detalhes Rosa -->
    <header class="bg-gradient-to-r from-blue-600 to-indigo-600 text-white shadow-lg border-b-4 border-pink-400 no-print">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="bg-pink-500 p-2.5 rounded-2xl shadow-sm text-white">
                    <i data-lucide="heart-handshake" class="w-7 h-7"></i>
                </div>
                <div>
                    <h1 class="text-xl font-extrabold tracking-tight">Diário & Relatórios Infantis</h1>
                    <p class="text-xs text-blue-100 font-medium">Acompanhamento diário da turma</p>
                </div>
            </div>

            <!-- Navegação entre Abas -->
            <nav class="flex bg-blue-700/50 p-1 rounded-xl backdrop-blur-sm border border-blue-400/30">
                <button onclick="switchTab('diary')" id="tabBtn-diary" class="px-4 py-2 rounded-lg font-semibold text-xs transition-all duration-200 bg-pink-500 text-white shadow-md flex items-center gap-2">
                    <i data-lucide="pen-tool" class="w-4 h-4"></i>
                    <span>Registro Diário</span>
                </button>
                <button onclick="switchTab('report')" id="tabBtn-report" class="px-4 py-2 rounded-lg font-semibold text-xs transition-all duration-200 text-blue-100 hover:text-white hover:bg-blue-600/50 flex items-center gap-2">
                    <i data-lucide="file-text" class="w-4 h-4"></i>
                    <span>Relatório Mensal</span>
                </button>
                <button onclick="switchTab('students')" id="tabBtn-students" class="px-4 py-2 rounded-lg font-semibold text-xs transition-all duration-200 text-blue-100 hover:text-white hover:bg-blue-600/50 flex items-center gap-2">
                    <i data-lucide="users" class="w-4 h-4"></i>
                    <span>Alunos</span>
                </button>
            </nav>
        </div>
    </header>

    <!-- Conteúdo Principal -->
    <main class="flex-1 max-w-6xl w-full mx-auto p-4 md:p-6">

        <!-- ABA 1: REGISTRO DIÁRIO -->
        <section id="view-diary" class="space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-blue-100">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6 pb-4 border-b border-pink-100">
                    <div>
                        <h2 class="text-lg font-bold text-blue-900 flex items-center gap-2">
                            <i data-lucide="sparkles" class="w-5 h-5 text-pink-500"></i>
                            Anotação Noturna
                        </h2>
                        <p class="text-xs text-slate-500">Selecione o aluno e marque as opções referentes ao dia.</p>
                    </div>
                    <div class="bg-blue-50 p-2 rounded-2xl border border-blue-100">
                        <label class="block text-[10px] font-bold text-blue-600 uppercase mb-0.5">Data da Rotina</label>
                        <input type="date" id="diaryDate" class="bg-white border border-blue-200 rounded-xl px-3 py-1.5 text-xs font-medium text-slate-700 outline-none focus:ring-2 focus:ring-pink-400">
                    </div>
                </div>

                <form id="diaryForm" class="space-y-6">
                    <div>
                        <label class="block text-xs font-bold text-blue-900 uppercase tracking-wider mb-2">Selecione a Criança</label>
                        <select id="diaryStudentSelect" required class="w-full bg-blue-50/50 border border-blue-200 rounded-xl px-4 py-2.5 text-sm font-medium text-slate-700 outline-none focus:ring-2 focus:ring-pink-400 focus:bg-white">
                            <option value="">-- Escolha um aluno --</option>
                        </select>
                    </div>

                    <!-- Tags Rápidas em Tons Rosa / Azul -->
                    <div>
                        <label class="block text-xs font-bold
