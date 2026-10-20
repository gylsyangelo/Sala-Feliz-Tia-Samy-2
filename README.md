<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Diário & Relatório da Educação Infantil</title>
    <!-- Tailwind CSS (Estilização Moderna e Cores) -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons (Ícones do Sistema) -->
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
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col font-sans">

    <!-- Cabeçalho Principal -->
    <header class="bg-indigo-600 text-white shadow-md no-print">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <i data-lucide="book-open-check" class="w-8 h-8 text-indigo-200"></i>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">Relatórios Infantis</h1>
                    <p class="text-xs text-indigo-200">Diário de Sala & Gerador Mensal</p>
                </div>
            </div>
            <!-- Navegação entre Abas -->
            <nav class="flex space-x-2">
                <button onclick="switchTab('diary')" id="tabBtn-diary" class="px-4 py-2 rounded-lg font-medium text-sm transition bg-indigo-700 text-white shadow-inner flex items-center gap-2">
                    <i data-lucide="pen-tool" class="w-4 h-4"></i>
                    <span>Registro Diário</span>
                </button>
                <button onclick="switchTab('report')" id="tabBtn-report" class="px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-indigo-500 text-indigo-100 flex items-center gap-2">
                    <i data-lucide="file-text" class="w-4 h-4"></i>
                    <span>Relatório Mensal</span>
                </button>
                <button onclick="switchTab('students')" id="tabBtn-students" class="px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-indigo-500 text-indigo-100 flex items-center gap-2">
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
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6 pb-4 border-b border-slate-100">
                    <div>
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i data-lucide="calendar" class="w-5 h-5 text-indigo-600"></i>
                            Registro Noturno da Turma
                        </h2>
                        <p class="text-sm text-slate-500">Selecione a criança, marque as opções do dia e adicione observações.</p>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Data da Rotina</label>
                        <input type="date" id="diaryDate" class="border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                    </div>
                </div>

                <form id="diaryForm" class="space-y-6">
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Selecione o Aluno(a)</label>
                        <select id="diaryStudentSelect" required class="w-full border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none bg-white">
                            <option value="">-- Escolha uma criança --</option>
                        </select>
                    </div>

                    <!-- Tags Rápidas -->
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-2">Acontecimentos do Dia (Clique nas opções):</label>
                        <div id="tagsContainer" class="flex flex-wrap gap-2">
                            <!-- Tags carregadas via JS -->
                        </div>
                    </div>

                    <!-- Observações -->
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Observações Adicionais / Detalhes do Dia</label>
                        <textarea id="diaryNotes" rows="3" placeholder="Ex: Criança comeu bem, participou das atividades mas apresentou dificuldades na hora do lanche..." class="w-full border border-slate-300 rounded-lg p-3 text-sm focus:ring-2 focus:ring-indigo-500 outline-none"></textarea>
                    </div>

                    <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2.5 rounded-lg transition shadow-md flex justify-center items-center gap-2">
                        <i data-lucide="save" class="w-4 h-4"></i>
                        <span>Salvar Registro de Hoje</span>
                    </button>
                </form>
            </div>

            <!-- Últimas Anotações -->
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <h3 class="text-md font-bold text-slate-800 mb-4 flex items-center gap-2">
                    <i data-lucide="history" class="w-4 h-4 text-indigo-600"></i>
                    Últimas Anotações Salvas
                </h3>
                <div id="recentLogs" class="space-y-3 max-h-80 overflow-y-auto">
                    <p class="text-sm text-slate-400 italic">Nenhum registro recente encontrado.</p>
                </div>
            </div>
        </section>

        <!-- ABA 2: RELATÓRIO MENSAL -->
        <section id="view-report" class="hidden space-y-6">
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 no-print">
                <h2 class="text-lg font-bold text-slate-800 mb-4 flex items-center gap-2">
                    <i data-lucide="file-check-2" class="w-5 h-5 text-indigo-600"></i>
                    Gerar Relatório Mensal por Aluno
                </h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Aluno(a)</label>
                        <select id="reportStudentSelect" class="w-full border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none bg-white">
                            <option value="">-- Escolha o aluno --</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 uppercase mb-1">Mês de Referência</label>
                        <input type="month" id="reportMonth" class="w-full border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                    </div>
                    <div class="flex items-end">
                        <button onclick="generateReport()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-medium py-2 rounded-lg transition shadow flex justify-center items-center gap-2 text-sm">
                            <i data-lucide="sparkles" class="w-4 h-4"></i>
                            <span>Gerar Relatório do Mês</span>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Visualização/Impressão do Relatório -->
            <div id="printableReport" class="hidden bg-white p-8 rounded-2xl shadow-md border border-slate-200">
                <div class="border-b pb-4 mb-6 flex justify-between items-start">
                    <div>
                        <h2 id="repStudentName" class="text-2xl font-bold text-slate-900">Nome do Aluno</h2>
                        <p id="repPeriod" class="text-sm font-medium text-indigo-600">Período: Mês/Ano</p>
                    </div>
                    <button onclick="window.print()" class="no-print bg-slate-800 hover:bg-slate-900 text-white text-xs px-3 py-2 rounded-lg transition flex items-center gap-2">
                        <i data-lucide="printer" class="w-4 h-4"></i>
                        <span>Imprimir / Salvar PDF</span>
                    </button>
                </div>

                <div class="space-y-4">
                    <h3 class="text-sm font-bold uppercase tracking-wider text-slate-500 border-b pb-1">Parecer Pedagógico Descritivo</h3>
                    <div id="repText" class="text-slate-700 leading-relaxed text-sm whitespace-pre-line bg-slate-50 p-4 rounded-xl border border-slate-100">
                        Selecione um aluno e mês para gerar o texto.
                    </div>
                </div>

                <div class="mt-8 space-y-4">
                    <h3 class="text-sm font-bold uppercase tracking-wider text-slate-500 border-b pb-1">Registros Diários do Mês</h3>
                    <div id="repLogList" class="space-y-3"></div>
                </div>
            </div>
        </section>

        <!-- ABA 3: ALUNOS -->
        <section id="view-students" class="hidden space-y-6">
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <h2 class="text-lg font-bold text-slate-800 mb-4 flex items-center gap-2">
                    <i data-lucide="user-plus" class="w-5 h-5 text-indigo-600"></i>
                    Cadastrar Novo Aluno
                </h2>
                <form id="addStudentForm" class="flex gap-3">
                    <input type="text" id="studentNameInput" placeholder="Nome completo da criança..." required class="flex-1 border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                    <button type="submit" class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition flex items-center gap-2">
                        <i data-lucide="plus" class="w-4 h-4"></i>
                        <span>Adicionar</span>
                    </button>
                </form>
            </div>

            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                <h3 class="text-md font-bold text-slate-800 mb-4">Alunos Cadastrados na Turma</h3>
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
                document.getElementById(`tabBtn-${t}`).className = "px-4 py-2 rounded-lg font-medium text-sm transition hover:bg-indigo-500 text-indigo-100 flex items-center gap-2";
            });

            document.getElementById(`view-${tabName}`).classList.remove('hidden');
            document.getElementById(`tabBtn-${tabName}`).className = "px-4 py-2 rounded-lg font-medium text-sm transition bg-indigo-700 text-white shadow-inner flex items-center gap-2";
        }

        function renderTags() {
            const container = document.getElementById('tagsContainer');
            container.innerHTML = availableTags.map(tag => `
                <button type="button" onclick="toggleTag('${tag.id}')" id="tagBtn-${tag.id}" 
                    class="px-3 py-1.5 rounded-full text-xs font-medium border border-slate-200 transition flex items-center gap-1 bg-slate-100 text-slate-700 hover:bg-slate-200">
                    <span>${tag.label}</span>
                </button>
            `).join('');
        }

        function toggleTag(tagId) {
            const index = selectedTags.indexOf(tagId);
            const btn = document.getElementById(`tagBtn-${tagId}`);

            if (index === -1) {
                selectedTags.push(tagId);
                btn.classList.remove('bg-slate-100', 'text-slate-700');
                btn.classList.add('bg-indigo-600', 'text-white', 'border-indigo-600');
            } else {
                selectedTags.splice(index, 1);
                btn.classList.remove('bg-indigo-600', 'text-white', 'border-indigo-600');
                btn.classList.add('bg-slate-100', 'text-slate-700');
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
            alert('Registro diário salvo com sucesso!');
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
                    return found ? `<span class="bg-indigo-50 text-indigo-700 text-[10px] px-2 py-0.5 rounded-full border border-indigo-100">${found.label}</span>` : '';
                }).join(' ');

                return `
                    <div class="p-3 bg-slate-50 rounded-xl border border-slate-100 text-sm space-y-1">
                        <div class="flex justify-between font-bold text-slate-700">
                            <span>${log.student}</span>
                            <span class="text-xs text-slate-400">${log.date}</span>
                        </div>
                        <div class="flex flex-wrap gap-1">${tagLabels}</div>
                        ${log.notes ? `<p class="text-xs text-slate-600 mt-1">"${log.notes}"</p>` : ''}
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
                <div class="p-3 bg-slate-50 rounded-lg text-xs border border-slate-200">
                    <span class="font-bold text-slate-700">${l.date}:</span> 
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
                <div class="flex justify-between items-center p-3 bg-slate-50 rounded-lg border border-slate-200 text-sm font-medium">
                    <span>${s}</span>
                    <button onclick="removeStudent(${idx})" class="text-rose-500 hover:text-rose-700 text-xs font-semibold">Excluir</button>
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
