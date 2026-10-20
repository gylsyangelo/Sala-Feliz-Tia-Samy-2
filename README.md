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
                        <label class="block text-xs font-bold text-blue-900 uppercase tracking-wider mb-3">Comportamento e Acontecimentos:</label>
                        <div id="tagsContainer" class="flex flex-wrap gap-2">
                            <!-- Tags geradas via JS -->
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-blue-900 uppercase tracking-wider mb-2">Observações e Detalhes do Dia</label>
                        <textarea id="diaryNotes" rows="3" placeholder="Ex: Criança comeu bem, estava alegre e participou com entusiasmo das brincadeiras..." class="w-full bg-blue-50/30 border border-blue-200 rounded-2xl p-3.5 text-sm outline-none focus:ring-2 focus:ring-pink-400 focus:bg-white"></textarea>
                    </div>

                    <button type="submit" class="w-full bg-pink-500 hover:bg-pink-600 text-white font-bold py-3 rounded-2xl transition-all duration-200 shadow-md shadow-pink-200 flex justify-center items-center gap-2 text-sm">
                        <i data-lucide="check-circle" class="w-5 h-5"></i>
                        <span>Salvar Registro de Hoje</span>
                    </button>
                </form>
            </div>

            <!-- Últimas Anotações -->
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-blue-100">
                <h3 class="text-md font-bold text-blue-900 mb-4 flex items-center gap-2">
                    <i data-lucide="clock" class="w-4 h-4 text-pink-500"></i>
                    Histórico Recente
                </h3>
                <div id="recentLogs" class="space-y-3 max-h-80 overflow-y-auto">
                    <p class="text-sm text-slate-400 italic">Nenhum registro encontrado.</p>
                </div>
            </div>
        </section>

        <!-- ABA 2: RELATÓRIO MENSAL -->
        <section id="view-report" class="hidden space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-blue-100 no-print">
                <h2 class="text-lg font-bold text-blue-900 mb-4 flex items-center gap-2">
                    <i data-lucide="award" class="w-5 h-5 text-pink-500"></i>
                    Gerador de Relatório Mensal
                </h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-2">
                    <div>
                        <label class="block text-xs font-bold text-blue-900 uppercase tracking-wider mb-1">Aluno(a)</label>
                        <select id="reportStudentSelect" class="w-full bg-blue-50/50 border border-blue-200 rounded-xl px-3 py-2 text-sm font-medium text-slate-700 outline-none focus:ring-2 focus:ring-pink-400">
                            <option value="">-- Escolha o aluno --</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-blue-900 uppercase tracking-wider mb-1">Mês de Referência</label>
                        <input type="month" id="reportMonth" class="w-full bg-blue-50/50 border border-blue-200 rounded-xl px-3 py-2 text-sm font-medium text-slate-700 outline-none focus:ring-2 focus:ring-pink-400">
                    </div>
                    <div class="flex items-end">
                        <button onclick="generateReport()" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2.5 rounded-xl transition shadow-md shadow-blue-200 flex justify-center items-center gap-2 text-sm">
                            <i data-lucide="sparkles" class="w-4 h-4 text-pink-300"></i>
                            <span>Gerar Parecer</span>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Relatório para Visualização / Impressão -->
            <div id="printableReport" class="hidden bg-white p-8 rounded-3xl shadow-md border border-blue-100">
                <div class="border-b-2 border-pink-100 pb-4 mb-6 flex justify-between items-start">
                    <div>
                        <h2 id="repStudentName" class="text-2xl font-black text-blue-950">Nome do Aluno</h2>
                        <p id="repPeriod" class="text-sm font-bold text-pink-500">Período: Mês/Ano</p>
                    </div>
                    <button onclick="window.print()" class="no-print bg-slate-900 hover:bg-slate-800 text-white text-xs font-bold px-4 py-2.5 rounded-xl transition flex items-center gap-2">
                        <i data-lucide="printer" class="w-4 h-4 text-pink-400"></i>
                        <span>Imprimir / PDF</span>
                    </button>
                </div>

                <div class="space-y-4">
                    <h3 class="text-xs font-bold uppercase tracking-widest text-blue-800 border-b pb-1">Parecer Pedagógico Descritivo</h3>
                    <div id="repText" class="text-slate-700 leading-relaxed text-sm whitespace-pre-line bg-pink-50/30 p-5 rounded-2xl border border-pink-100">
                        Selecione um aluno e mês para gerar o relatório.
                    </div>
                </div>

                <div class="mt-8 space-y-4">
                    <h3 class="text-xs font-bold uppercase tracking-widest text-blue-800 border-b pb-1">Registros Diários do Mês</h3>
                    <div id="repLogList" class="space-y-3"></div>
                </div>
            </div>
        </section>

        <!-- ABA 3: ALUNOS -->
        <section id="view-students" class="hidden space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-blue-100">
                <h2 class="text-lg font-bold text-blue-900 mb-4 flex items-center gap-2">
                    <i data-lucide="user-plus" class="w-5 h-5 text-pink-500"></i>
                    Cadastrar Novo Aluno
                </h2>
                <form id="addStudentForm" class="flex gap-3">
                    <input type="text" id="studentNameInput" placeholder="Nome completo da criança..." required class="flex-1 bg-blue-50/50 border border-blue-200 rounded-xl px-4 py-2 text-sm outline-none focus:ring-2 focus:ring-pink-400">
                    <button type="submit" class="bg-pink-500 hover:bg-pink-600 text-white font-bold text-sm px-5 py-2 rounded-xl transition flex items-center gap-2">
                        <i data-lucide="plus" class="w-4 h-4"></i>
                        <span>Adicionar</span>
                    </button>
                </form>
            </div>

            <div class="bg-white p-6 rounded-3xl shadow-sm border border-blue-100">
                <h3 class="text-md font-bold text-blue-900 mb-4">Alunos Cadastrados na Turma</h3>
                <div id="studentsList" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3"></div>
            </div>
        </section>

    </main>

    <script>
        let students = JSON.parse(localStorage.getItem('app_students')) || ['Ana Silva', 'Lucas Gabriel', 'Sofia Costa'];
        let logs = JSON.parse(localStorage.getItem('app_logs')) || [];

        const availableTags = [
            { id: 'eat_good', label: '🍎 Comeu bem' },
            { id: 'eat_bad', label: '🥗 Não comeu bem / recusou' },
            { id: 'behavior_good', label: '⭐ Se comportou muito bem' },
            { id: 'behavior_issue', label: '⚠️ Fez bobeira / desobedeceu' },
            { id: 'sick', label: '🤒 Está doentinha / indisposta' },
            { id: 'dev_fast', label: '🚀 Está se desenvolvendo rápido' },
            { id: 'dev_diff', label: '🧩 Dificuldades em algo específico' },
            { id: 'nap_good', label: '😴 Dormiu / Descansou bem' }
             { id: 'nap_good', label: '😴 Não Dormiu / Descansou bem' }
        ];

        let selectedTags = [];

        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('diaryDate').value = new Date().toISOString().split('T')[0];
            document.getElementById('reportMonth').value = new Date().toISOString().slice(0, 7);

            renderTags();
            updateStudentDropdowns();
            renderStudentsList();
            renderRecentLogs();
            lucide.createIcons();
        });

        function switchTab(tabName) {
            ['diary', 'report', 'students'].forEach(t => {
                document.getElementById(`view-${t}`).classList.add('hidden');
                document.getElementById(`tabBtn-${t}`).className = "px-4 py-2 rounded-lg font-semibold text-xs transition-all duration-200 text-blue-100 hover:text-white hover:bg-blue-600/50 flex items-center gap-2";
            });

            document.getElementById(`view-${tabName}`).classList.remove('hidden');
            document.getElementById(`tabBtn-${tabName}`).className = "px-4 py-2 rounded-lg font-semibold text-xs transition-all duration-200 bg-pink-500 text-white shadow-md flex items-center gap-2";
        }

        function renderTags() {
            const container = document.getElementById('tagsContainer');
            container.innerHTML = availableTags.map(tag => `
                <button type="button" onclick="toggleTag('${tag.id}')" id="tagBtn-${tag.id}" 
                    class="px-3.5 py-1.5 rounded-xl text-xs font-semibold border border-blue-100 transition-all duration-150 flex items-center gap-1 bg-blue-50/60 text-blue-900 hover:bg-pink-100 hover:text-pink-700">
                    <span>${tag.label}</span>
                </button>
            `).join('');
        }

        function toggleTag(tagId) {
            const index = selectedTags.indexOf(tagId);
            const btn = document.getElementById(`tagBtn-${tagId}`);

            if (index === -1) {
                selectedTags.push(tagId);
                btn.classList.remove('bg-blue-50/60', 'text-blue-900', 'border-blue-100');
                btn.classList.add('bg-pink-500', 'text-white', 'border-pink-500', 'shadow-sm');
            } else {
                selectedTags.splice(index, 1);
                btn.classList.remove('bg-pink-500', 'text-white', 'border-pink-500', 'shadow-sm');
                btn.classList.add('bg-blue-50/60', 'text-blue-900', 'border-blue-100');
            }
        }

        document.getElementById('diaryForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const student = document.getElementById('diaryStudentSelect').value;
            const date = document.getElementById('diaryDate').value;
            const notes = document.getElementById('diaryNotes').value;

            if (!student) return alert('Por favor, selecione uma criança.');

            const newLog = { id: Date.now(), student, date, tags: [...selectedTags], notes };
            logs.unshift(newLog);
            localStorage.setItem('app_logs', JSON.stringify(logs));

            document.getElementById('diaryNotes').value = '';
            selectedTags = [];
            renderTags();
            renderRecentLogs();
            alert('Registro salvo com sucesso!');
        });

        function renderRecentLogs() {
            const container = document.getElementById('recentLogs');
            if (logs.length === 0) {
                container.innerHTML = `<p class="text-sm text-slate-400 italic">Nenhum registro encontrado.</p>`;
                return;
            }

            container.innerHTML = logs.slice(0, 5).map(log => {
                const tagLabels = log.tags.map(tid => {
                    const found = availableTags.find(t => t.id === tid);
                    return found ? `<span class="bg-pink-50 text-pink-700 text-[10px] font-bold px-2 py-0.5 rounded-md border border-pink-100">${found.label}</span>` : '';
                }).join(' ');

                return `
                    <div class="p-3.5 bg-blue-50/30 rounded-2xl border border-blue-100 text-sm space-y-1.5">
                        <div class="flex justify-between font-bold text-blue-950">
                            <span>${log.student}</span>
                            <span class="text-xs text-blue-400 font-medium">${log.date}</span>
                        </div>
                        <div class="flex flex-wrap gap-1">${tagLabels}</div>
                        ${log.notes ? `<p class="text-xs text-slate-600 mt-1 italic">"${log.notes}"</p>` : ''}
                    </div>
                `;
            }).join('');
        }

        function generateReport() {
            const student = document.getElementById('reportStudentSelect').value;
            const monthStr = document.getElementById('reportMonth').value;

            if (!student || !monthStr) return alert('Selecione o aluno e o mês.');

            const studentLogs = logs.filter(l => l.student === student && l.date.startsWith(monthStr));
            document.getElementById('repStudentName').innerText = student;
            document.getElementById('repPeriod').innerText = `Período: Mês ${monthStr.split('-')[1]}/${monthStr.split('-')[0]}`;
            
            document.getElementById('printableReport').classList.remove('hidden');

            if (studentLogs.length === 0) {
                document.getElementById('repText').innerText = "Não existem anotações registradas para este aluno no mês selecionado.";
                document.getElementById('repLogList').innerHTML = "<p class='text-xs text-slate-400'>Nenhum histórico encontrado para este período.</p>";
                return;
            }

            let tagCounts = {};
            studentLogs.forEach(l => {
                l.tags.forEach(t => { tagCounts[t] = (tagCounts[t] || 0) + 1; });
            });

            let narrative = `Relatório avaliativo mensal referente ao desenvolvimento da criança ${student}.\n\n`;
            if (tagCounts['eat_good'] > 1) narrative += `Durante o mês, a criança demonstrou ótima adaptação nas refeições, alimentando-se bem na maior parte dos dias. `;
            if (tagCounts['eat_bad'] > 0) narrative += `Em alguns momentos do mês apresentou certa recusa alimentar, sendo acompanhada com carinho pela equipe. `;
            if (tagCounts['behavior_good'] > 1) narrative += `Apresentou excelente comportamento geral, demonstrando bom convívio e empatia com os colegas. `;
            if (tagCounts['behavior_issue'] > 0) narrative += `Houve alguns episódios pontuais de desobediência ou agitação, nos quais trabalhamos o diálogo e a mediação. `;
            if (tagCounts['sick'] > 0) narrative += `Foram notados alguns dias de indisposição ou sintomas de saúde, exigindo maior atenção e cuidado no período de repouso. `;
            if (tagCounts['dev_fast'] > 0) narrative += `Destacou-se por apresentar um ritmo de desenvolvimento acelerado e muita facilidade nas atividades propostas. `;
            if (tagCounts['dev_diff'] > 0) narrative += `Em relação a algumas tarefas, foram identificadas dificuldades pontuais que continuaremos estimulando e apoiando de perto. `;

            narrative += `\n\nNo geral, o aluno(a) segue avançando de forma positiva e enriquecedora em sua jornada escolar ao longo deste mês.`;
            document.getElementById('repText').innerText = narrative;

            document.getElementById('repLogList').innerHTML = studentLogs.map(l => `
                <div class="p-3 bg-blue-50/40 rounded-xl text-xs border border-blue-100">
                    <span class="font-bold text-blue-900">${l.date}:</span> 
                    ${l.tags.map(tid => availableTags.find(t => t.id === tid)?.label).filter(Boolean).join(', ')}
                    ${l.notes ? `<span class="block text-slate-500 mt-1 italic">Obs: ${l.notes}</span>` : ''}
                </div>
            `).join('');
        }

        function updateStudentDropdowns() {
            const options = `<option value="">-- Escolha uma criança --</option>` + 
                students.map(s => `<option value="${s}">${s}</option>`).join('');
            document.getElementById('diaryStudentSelect').innerHTML = options;
            document.getElementById('reportStudentSelect').innerHTML = options;
        }

        function renderStudentsList() {
            const container = document.getElementById('studentsList');
            container.innerHTML = students.map((s, idx) => `
                <div class="flex justify-between items-center p-3 bg-blue-50/40 rounded-2xl border border-blue-100 text-sm font-medium">
                    <span class="text-blue-950 font-bold">${s}</span>
                    <button onclick="removeStudent(${idx})" class="text-pink-600 hover:text-pink-800 text-xs font-bold">Excluir</button>
                </div>
            `).join('');
        }

        document.getElementById('addStudentForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const input = document.getElementById('studentNameInput');
            const name = input.value.trim();
            if (name && !students.includes(name)) {
                students.push(name);
                localStorage.setItem('app_students', JSON.stringify(students));
                input.value = '';
                updateStudentDropdowns();
                renderStudentsList();
            }
        });

        function removeStudent(index) {
            if (confirm(`Deseja remover ${students[index]} da lista?`)) {
                students.splice(index, 1);
                localStorage.setItem('app_students', JSON.stringify(students));
                updateStudentDropdowns();
                renderStudentsList();
            }
        }
    </script>
</body>
</html>
