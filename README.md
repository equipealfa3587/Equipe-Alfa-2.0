<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciamento de Ministério - Cronograma</title>
    <!-- Carrega Tailwind CSS para um design moderno e responsivo -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- O Trebuchet MS é um fonte de sistema (web-safe), não precisa de importação externa. -->
    <!-- Ícones Font Awesome para WhatsApp, Edição, Cruz e Música -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    
    <style>
        /* Define a fonte Trebuchet MS, conforme solicitado, com fallback sans-serif */
        body {
            font-family: 'Trebuchet MS', 'Lucida Grande', 'Lucida Sans Unicode', 'Lucida Sans', Tahoma, sans-serif;
            background-color: #f3f4f6; /* Fundo claro para o body */
        }
        
        /* ATUALIZADO: Estilo para o título "Equipe Alfa" com fonte Bradley Hand ITC */
        #equipe-alfa-title {
            font-family: 'Bradley Hand ITC', 'Bradley Hand', 'Comic Sans MS', cursive; /* Bradley Hand e fallbacks cursivos */
        }

        .app-bg {
            /* Fundo com gradiente AZUL suave (Deep Indigo to Blue) */
            background-image: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%); 
        }
        .card {
            background-color: white;
            border-radius: 16px; /* Mais arredondado */
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.05); /* Sombra mais profunda */
        }
        
        /* Estilo para Botão 3D/Press: Tema Azul */
        .btn-primary {
            background-color: #3b82f6; /* Blue 500 */
            box-shadow: 0 5px 0 #1e40af; /* Blue 800 (Sombra "3D") */
            transition: all 0.1s ease-in-out;
            position: relative;
            top: 0;
        }
        .btn-primary:hover {
            background-color: #2563eb; /* Blue 600 */
        }
        .btn-primary:active {
            top: 5px; /* Move para baixo ao ser clicado */
            box-shadow: 0 0 0 #1e40af; /* Remove a sombra "3D" */
        }
        
        /* Estilo para Botão 3D/Press: Tema Laranja (ADMIN) */
        .btn-admin {
            background-color: #f97316; /* Orange 500 */
            box-shadow: 0 5px 0 #c2410c; /* Orange 800 (Sombra "3D") */
            transition: all 0.1s ease-in-out;
            position: relative;
            top: 0;
        }
        .btn-admin:hover {
            background-color: #ea580c; /* Orange 600 */
        }
        .btn-admin:active {
            top: 5px; /* Move para baixo ao ser clicado */
            box-shadow: 0 0 0 #c2410c; /* Remove a sombra "3D" */
        }


        .item-card {
            border-left: 5px solid;
            border-radius: 8px;
            background-color: #f9fafb; /* Fundo levemente cinza para os itens */
        }
        .item-card:hover {
            background-color: #ffffff;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }

        /* Cores para o Cronograma */
        .harmony { border-left-color: #f59e0b; } /* Amarelo (Harmonia) */
        .vocal { border-left-color: #3b82f6; }   /* Azul (Vocal) */
        .music { border-left-color: #10b981; }   /* Verde (Música) */
        .other { border-left-color: #ef4444; }   /* Vermelho (Outro) */
        
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.95);
            z-index: 50;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }

        /* Botão flutuante do WhatsApp */
        .whatsapp-btn {
            position: fixed;
            bottom: 22px;
            right: 22px;
            z-index: 40;
            background-color: #25D366; /* Cor do WhatsApp */
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.08);
            transition: transform 0.2s ease;
        }
        .whatsapp-btn:hover {
            background-color: #128C7E;
            transform: scale(1.05);
        }
        
        /* Estilos para as áreas de logo clicáveis */
        .logo-area {
            position: relative;
            display: flex;
            align-items: center;
        }
        
        /* Estilo para inputs de data e hora para melhor visualização */
        input[type="date"], input[type="time"] {
            appearance: none;
            -webkit-appearance: none;
            -moz-appearance: none;
            cursor: pointer;
            padding-right: 1.5rem; /* Espaço para ícone de calendário/relógio */
        }
        
        /* Estilo 3D e arredondado para o título "Equipe Alfa" */
        .text-3d {
            /* Múltiplas sombras para simular profundidade e contorno arredondado */
            text-shadow: 
                1px 1px 0 #ffffff, /* Brilho/contorno branco (simula borda arredondada) */
                2px 2px 0 #b45309, /* Sombra dourada mais escura para profundidade */
                3px 3px 0 #a16207; /* Sombra mais profunda */
        }
        
        /* Novo estilo para o card de login */
        .login-card-admin {
            background-color: #fef3c7; /* Amarelo pálido */
            border: 2px solid #f59e0b; /* Borda laranja */
        }
    </style>
    <!-- Configuração do Tailwind para usar cores e classes adicionais -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        // Cores de foco e destaque agora serão Azuis
                        'blue-deep': '#1e3a8a',
                        // Novo tom escuro para o cabeçalho (embora não usado agora)
                        'indigo-950': '#1e1b4b',
                    }
                }
            }
        }
    </script>
</head>
<body class="min-h-screen">

    <!-- Botão Flutuante do WhatsApp -->
    <a href="https://api.whatsapp.com/send?phone=5511999999999&text=Ol%C3%A1%2C%20quero%20falar%20sobre%20o%20ensaio!" 
       target="_blank" 
       class="whatsapp-btn p-4 rounded-full text-white flex items-center justify-center text-xl">
        <i class="fab fa-whatsapp"></i>
    </a>

    <!-- Sobreposição de Carregamento -->
    <div id="loading-overlay" class="loading-overlay hidden">
        <!-- Spinner e texto de carregamento com tema azul -->
        <svg class="animate-spin -ml-1 mr-3 h-10 w-10 text-blue-700" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        <p class="mt-4 text-lg font-semibold text-blue-700">Conectando ao Firestore...</p>
    </div>

    <!-- Layout Principal -->
    <div class="app-bg p-4 md:p-8 min-h-screen">
        <div class="max-w-4xl mx-auto">
            
            <!-- Cabeçalho DOURADO DEGRADÊ -->
            <header class="flex flex-col md:flex-row justify-between items-center mb-6 p-4 bg-gradient-to-r from-yellow-700 to-yellow-500 rounded-xl">
                
                <!-- Logo da Igreja (Área FIXA) -->
                <div id="church-logo-area" class="logo-area mb-4 md:mb-0 text-center md:text-left">
                    <div class="rounded-full mr-3 border-2 border-gray-900 w-14 h-14 flex items-center justify-center relative bg-yellow-600">
                        <i class="fas fa-cross text-white text-lg absolute z-10"></i>
                        <i class="fas fa-music text-gray-900 text-3xl opacity-70 absolute transform -rotate-12"></i>
                    </div>
                </div>
                
                <!-- Título Principal (AGORA EQUIPE ALFA - Com efeito 3D) -->
                <div class="text-center">
                    <h1 id="equipe-alfa-title" class="text-5xl font-black mb-1 text-gray-900 tracking-wider text-3d">
                        Equipe Alfa
                    </h1>
                </div>

                <!-- Nome do Grupo (LOUVOR 2.0) e Escopo -->
                <div class="text-center md:text-right flex flex-col items-center md:items-end mt-4 md:mt-0">
                    <span class="text-xl font-extrabold text-yellow-900 tracking-widest px-3 py-1 rounded-lg bg-white/60 mb-1">
                        LOUVOR 2.0
                    </span>
                    <p class="text-gray-700 text-sm font-medium">
                        Estudo Vocal e Rearmonização
                    </p>
                </div>

            </header>
            
            <!-- NOVO: Card de Controle de Acesso (Login) -->
            <div id="admin-login-card" class="card login-card-admin p-5 mb-8">
                <h2 class="text-xl font-bold text-gray-800 mb-4 flex items-center">
                    <i class="fas fa-user-shield mr-2 text-orange-600"></i> Controle de Acesso
                </h2>
                
                <div id="admin-status" class="mb-4 p-3 rounded-lg border">
                    <!-- Conteúdo será injetado pelo JS -->
                </div>
                
                <div id="admin-login-area" class="flex flex-col md:flex-row gap-3">
                    <input type="text" id="admin-key-input" placeholder="Cole seu ID de Admin para Agendar" class="flex-1 rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-orange-500 focus:border-orange-500 text-sm">
                    <button id="admin-login-btn" class="btn-admin text-white py-3 px-6 rounded-lg font-bold uppercase tracking-wider text-sm md:w-auto w-full">
                        Entrar como Admin
                    </button>
                </div>
            </div>


            <!-- Card de Agendamento (Cronograma) - VISÍVEL APENAS PARA ADMIN -->
            <div id="admin-schedule-card" class="card p-5 mb-8 hidden">
                <!-- Título Modificado: Agendar Atividade -->
                <h2 class="text-2xl font-bold text-gray-800 mb-5 border-b pb-2 flex items-center">
                    <i class="fas fa-calendar-alt mr-2 text-blue-600"></i> Agendar Atividade
                </h2>
                
                <form id="add-item-form" class="space-y-4">
                    <!-- Nome do Item -->
                    <div>
                        <label for="item-name" class="block text-sm font-semibold text-gray-700">Título / Assunto</label>
                        <!-- Focus ring com tema azul -->
                        <input type="text" id="item-name" required class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-blue-500 focus:border-blue-500" placeholder="Ex: Ensaio Geral (Louvor 19:30)">
                    </div>

                    <!-- Categoria, Data e Horário -->
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4 items-end mb-4">
                        <!-- Categoria -->
                        <div class="md:col-span-1">
                            <label for="item-category" class="block text-sm font-semibold text-gray-700">Foco</label>
                            <!-- Focus ring com tema azul -->
                            <select id="item-category" required class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-blue-500 focus:border-blue-500">
                                <option value="music">Música / Louvor</option>
                                <option value="vocal">Treinamento Vocal</option>
                                <option value="harmony">Harmonia / Teoria</option>
                                <option value="other">Outro / Avisos</option>
                            </select>
                        </div>
                        
                        <!-- Data -->
                        <div>
                            <label for="item-date" class="block text-sm font-semibold text-gray-700">Data</label>
                            <input type="date" id="item-date" required class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-blue-500 focus:border-blue-500">
                        </div>
                        
                        <!-- Horário -->
                        <div>
                            <label for="item-time" class="block text-sm font-semibold text-gray-700">Horário</label>
                            <input type="time" id="item-time" required class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-blue-500 focus:border-blue-500">
                        </div>

                        <!-- Botão de Agendar (com efeito 3D) -->
                        <div class="md:col-span-1">
                            <button type="submit" class="btn-primary w-full text-white py-3 px-4 rounded-lg font-bold uppercase tracking-wider">
                                Agendar
                            </button>
                        </div>
                    </div>
                    
                    <!-- Detalhes / Notas -->
                    <div>
                        <label for="item-details" class="block text-sm font-semibold text-gray-700">Detalhes / Notas (Opcional)</label>
                        <textarea id="item-details" rows="3" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-blue-500 focus:border-blue-500" placeholder="Ex: Músicas a ensaiar, foco da técnica vocal, tarefas para a próxima reunião."></textarea>
                    </div>
                </form>
            </div>

            <!-- Lista de Atividades Agendadas -->
            <div class="card p-5 mb-10">
                <div class="flex justify-start items-center mb-4 border-b pb-3">
                    <h2 class="text-2xl font-bold text-gray-800">Próximos Ensaio/Atividades</h2>
                </div>

                <!-- Lista de Itens do Cronograma -->
                <div id="monogram-list" class="space-y-4">
                    <!-- Itens serão injetados aqui pelo JavaScript -->
                    <p id="empty-state" class="text-center text-gray-500 italic p-6 border-2 border-dashed border-gray-200 rounded-lg">
                        <i class="fas fa-calendar-alt mr-2"></i> Nenhuma atividade agendada.
                    </p>
                </div>
            </div>
            
            <!-- Exibição do ID do Usuário para Persistência - Tema Azul -->
            <footer class="mt-4 text-center text-blue-200 text-sm pb-10">
                <p>ID de Persistência (Privado): <span id="user-id-display" class="font-mono text-blue-100 bg-blue-600 px-2 py-0.5 rounded-md inline-block mt-1">Carregando...</span></p>
            </footer>
        </div>
    </div>

    <!-- Modal de Seleção de Nome / Justificativa (Centralizado) -->
    <div id="name-selection-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden">
        <div class="card p-6 w-11/12 max-w-md">
            <!-- Título do Modal será atualizado via JS -->
            <h3 id="modal-title" class="text-xl font-bold text-gray-800 mb-4 border-b pb-2 flex items-center">
                <i class="fas fa-user-tag mr-2 text-blue-600"></i> Selecione Seu Nome
            </h3>
            <p id="modal-status-text" class="text-sm text-gray-600 mb-4">Escolha o nome com o qual você está confirmando sua presença na atividade:</p>
            
            <!-- Container de Seleção de Nomes (Visível por padrão) -->
            <div id="names-list-container" class="grid grid-cols-2 gap-3 max-h-60 overflow-y-auto p-2">
                <!-- Botões de nome e de cancelamento serão injetados aqui -->
            </div>

            <!-- Container de Justificativa (Oculto por padrão, aparece se action=absent) -->
            <div id="justification-container" class="hidden flex flex-col space-y-4 mt-4">
                <div>
                    <label for="justification-input" class="block text-sm font-semibold text-gray-700">Justificativa da Ausência (Opcional)</label>
                    <textarea id="justification-input" rows="3" class="mt-1 block w-full rounded-lg border-gray-300 shadow-sm p-3 border focus:ring-red-500 focus:border-red-500" placeholder="Ex: Problemas de saúde, viagem de trabalho, etc."></textarea>
                </div>
                <!-- Botão Finalizar Ausência -->
                <button id="final-absent-btn" class="bg-red-600 hover:bg-red-700 text-white py-3 px-4 rounded-lg font-bold transition duration-200">
                    <i class="fas fa-save mr-2"></i> Finalizar Ausência
                </button>
                <!-- Botão Voltar -->
                <button id="back-to-names-btn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 py-2 px-4 rounded-lg font-bold transition duration-200">
                    <i class="fas fa-arrow-left mr-2"></i> Voltar para Seleção de Nome
                </button>
            </div>

            <button id="close-modal-btn" class="mt-4 w-full bg-red-500 hover:bg-red-600 text-white py-2 px-4 rounded-lg font-bold transition duration-200">
                Fechar
            </button>
        </div>
    </div>

    <!-- Importações do Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, collection, addDoc, deleteDoc, onSnapshot, query, updateDoc, getDoc, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // *** INÍCIO DA CONFIGURAÇÃO FIREBASE (MODO DE DESENVOLVIMENTO) ***
        
        // As variáveis a seguir são fornecidas automaticamente pelo ambiente de desenvolvimento (Canvas):
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        // --- NOVO: Variável de ambiente do Admin. Se o token inicial estiver presente, o usuário é Admin. ---
        let isAdmin = false; 
        
        // --- NOVO: Chave Admin (UID) para testes locais, se necessário. Se este ID estiver logado, ele é Admin. ---
        let ADMIN_UID_KEY = initialAuthToken ? "ADMIN_TOKEN_USER" : null; 

        // *** FIM DA CONFIGURAÇÃO FIREBASE ***

        let app;
        let db;
        let auth;
        let userId = null;
        let isAuthReady = false;
        let cronogramaItems = []; // Cache global para os itens do cronograma
        
        // NOVO: Contexto de RSVP para o fluxo de Justificativa
        let currentRsvpContext = { docId: null, action: null, selectedName: null };

        // Lista de nomes para seleção
        const availableNames = [
            'Junior', 'PR. Marcos', 'PR. Davi', 'Dejailson', 'Renato', 
            'Ricardo', 'Lyedson', 'Robson', 'Marlene', 'Marilena', 'Patrícia'
        ];

        // Elementos do Cronograma e UI
        const monogramList = document.getElementById('monogram-list');
        const emptyState = document.getElementById('empty-state');
        const userIdDisplay = document.getElementById('user-id-display');
        const loadingOverlay = document.getElementById('loading-overlay');
        // Novos Elementos Admin
        const adminScheduleCard = document.getElementById('admin-schedule-card');
        const adminStatus = document.getElementById('admin-status');
        const adminKeyInput = document.getElementById('admin-key-input');
        const adminLoginBtn = document.getElementById('admin-login-btn');
        
        // Elementos do Modal
        const nameSelectionModal = document.getElementById('name-selection-modal');
        const modalTitle = document.getElementById('modal-title');
        const namesListContainer = document.getElementById('names-list-container');
        const modalStatusText = document.getElementById('modal-status-text');
        // Elementos para Justificativa
        const justificationContainer = document.getElementById('justification-container');
        const justificationInput = document.getElementById('justification-input');
        const finalAbsentBtn = document.getElementById('final-absent-btn');
        const backToNamesBtn = document.getElementById('back-to-names-btn');


        // Configura o nível de log do Firestore
        setLogLevel('Debug');

        /**
         * Objeto de mapeamento para cores, classes e nomes de categoria do Cronograma.
         */
        const categoryMap = {
            harmony: { name: 'Harmonia/Teoria', class: 'harmony', color: '#f59e0b', icon: 'fas fa-wave-square' },
            vocal: { name: 'Treinamento Vocal', class: 'vocal', color: '#3b82f6', icon: 'fas fa-microphone' },
            music: { name: 'Música/Louvor', class: 'music', color: '#10b981', icon: 'fas fa-music' },
            other: { name: 'Outro/Avisos', class: 'other', color: '#ef4444', icon: 'fas fa-bell' },
        };
        
        /**
         * Define a data de hoje e um horário padrão nos campos de input.
         */
        function setDefaultDateTime() {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            
            const defaultTime = "19:00"; 
            
            const dateInput = document.getElementById('item-date');
            const timeInput = document.getElementById('item-time');

            if (dateInput) dateInput.value = `${yyyy}-${mm}-${dd}`;
            if (timeInput) timeInput.value = defaultTime;
        }


        // --- Funções de Inicialização e Autenticação/Admin ---

        /**
         * Atualiza o estado de isAdmin e a UI.
         */
        function updateAdminStatus(user) {
            // Se o usuário foi autenticado com o token inicial (Admin no ambiente Canvas)
            if (initialAuthToken && user.uid !== 'default-user-id') { 
                isAdmin = true;
                ADMIN_UID_KEY = user.uid; // Define o UID Admin com base no token
                adminStatus.innerHTML = `
                    <p class="text-sm text-green-700 font-semibold"><i class="fas fa-check-circle mr-1"></i> Status: Administrador Logado.</p>
                    <p class="text-xs text-gray-600 mt-1">Você pode agendar e deletar atividades.</p>
                `;
                adminKeyInput.value = user.uid; // Preenche o campo de input com o UID Admin
            } 
            // Se o usuário atual corresponde ao ADMIN_UID_KEY inserido manualmente ou detectado
            else if (user.uid === ADMIN_UID_KEY) { 
                isAdmin = true;
                adminStatus.innerHTML = `
                    <p class="text-sm text-green-700 font-semibold"><i class="fas fa-check-circle mr-1"></i> Status: Administrador Logado.</p>
                    <p class="text-xs text-gray-600 mt-1">Você pode agendar e deletar atividades.</p>
                `;
            } else {
                isAdmin = false;
                adminStatus.innerHTML = `
                    <p class="text-sm text-blue-700 font-semibold"><i class="fas fa-user mr-1"></i> Status: Membro da Equipe.</p>
                    <p class="text-xs text-gray-600 mt-1">Você só pode confirmar sua presença ou ausência nas atividades.</p>
                `;
            }
            
            // Alterna a visibilidade da interface de Admin
            toggleAdminFeatures(); 
            // Re-renderiza a lista para atualizar a visibilidade dos botões de deletar
            renderMonogram(cronogramaItems); 
        }

        /**
         * Alterna a visibilidade dos elementos de UI exclusivos do Admin.
         */
        function toggleAdminFeatures() {
            if (isAdmin) {
                adminScheduleCard.classList.remove('hidden');
                adminLoginBtn.textContent = 'Logout Admin';
                adminLoginBtn.classList.remove('btn-admin');
                adminLoginBtn.classList.add('bg-gray-500', 'hover:bg-gray-600', 'shadow-none');
            } else {
                adminScheduleCard.classList.add('hidden');
                adminLoginBtn.textContent = 'Entrar como Admin';
                adminLoginBtn.classList.add('btn-admin');
                adminLoginBtn.classList.remove('bg-gray-500', 'hover:bg-gray-600', 'shadow-none');
            }
        }


        /**
         * Inicializa o Firebase e a autenticação.
         */
        async function initializeFirebase() {
            if (!firebaseConfig) {
                 console.error("ERRO DE CONFIGURAÇÃO: A configuração do Firebase está ausente.");
                 loadingOverlay.innerHTML = '<p class="text-red-600">ERRO: Falha na configuração do Firebase. (Faltam chaves de ambiente).</p>';
                 loadingOverlay.classList.remove('hidden');
                 return;
            }

            setDefaultDateTime();

            loadingOverlay.classList.remove('hidden');

            try {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                // Autenticação: tenta com o token de Admin, senão usa anônimo
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }

                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        userId = user.uid;
                        userIdDisplay.textContent = userId;
                        isAuthReady = true;
                        
                        // Atualiza o estado Admin
                        updateAdminStatus(user);
                        
                        setupCronogramaFirestoreListener(); 
                    } else {
                        userId = null;
                        userIdDisplay.textContent = 'Não Autenticado';
                        isAuthReady = true;
                        updateAdminStatus({ uid: null }); // Limpa o status
                    }
                    loadingOverlay.classList.add('hidden');
                });

            } catch (error) {
                console.error("Erro ao inicializar ou autenticar no Firebase:", error);
                loadingOverlay.innerHTML = `<p class="text-red-600">Falha na Conexão: ${error.message}</p>`;
            }
        }

        // --- Funções de Caminho da Coleção ---

        /**
         * Retorna o caminho da coleção para os itens do cronograma (dados privados).
         */
        function getCronogramaCollectionPath() {
            // Caminho privado: /artifacts/{appId}/users/{userId}/cronograma_items
            return `artifacts/${appId}/users/${userId}/cronograma_items`; 
        }
        
        // --- Funções de Fluxo do Modal (Seleção/Justificativa) ---
        
        /**
         * Alterna a visualização do modal para a seleção de nomes.
         */
        function showNameSelectionView(docId, requestedAction) {
            // Limpa o contexto
            currentRsvpContext = { docId: docId, action: requestedAction, selectedName: null };

            namesListContainer.classList.remove('hidden');
            justificationContainer.classList.add('hidden');
            document.getElementById('close-modal-btn').classList.remove('hidden'); // Exibe o botão Fechar principal
            
            // Renderiza os nomes
            renderNamesList();
        }

        /**
         * Alterna a visualização do modal para a entrada de justificativa.
         */
        function showJustificationView(selectedName) {
            // Atualiza o contexto com o nome selecionado
            currentRsvpContext.selectedName = selectedName;
            
            // Atualiza a UI para o modo Justificativa
            modalTitle.innerHTML = `<i class="fas fa-user-times mr-2 text-red-600"></i> Justificar Ausência de ${selectedName}`;
            modalStatusText.textContent = `Por favor, insira o motivo da ausência de ${selectedName}. (Opcional)`;
            justificationInput.value = ''; // Limpa o campo para o novo uso
            
            namesListContainer.classList.add('hidden');
            justificationContainer.classList.remove('hidden');
            document.getElementById('close-modal-btn').classList.add('hidden'); // Esconde o botão Fechar principal

        }
        
        /**
         * Renderiza a lista de nomes e status no modal, independente da ação.
         */
        function renderNamesList() {
            const docId = currentRsvpContext.docId;
            const requestedAction = currentRsvpContext.action;
            
            namesListContainer.innerHTML = '';
            
            const item = cronogramaItems.find(i => i.id === docId);
            if (!item) return;

            const confirmedUsers = item.confirmedBy || [];
            const absentUsers = item.absentBy || [];

            const actionTitle = requestedAction === 'confirm' ? 'Confirmar Presença' : 'Informar Ausência';
            modalTitle.innerHTML = `<i class="fas fa-user-tag mr-2 text-blue-600"></i> Selecione Seu Nome para ${actionTitle}`;
            modalStatusText.textContent = `Escolha o nome para registrar ${actionTitle}.`;

            // 1. Renderiza os botões de seleção de nome
            const nameButtons = availableNames.map(name => {
                // Checa o status do nome (a chave principal agora é o nome)
                const isConfirmed = confirmedUsers.some(user => user.name === name);
                const isAbsent = absentUsers.some(user => user.name === name);

                let buttonClass = 'bg-blue-600 hover:bg-blue-700';
                let statusText = '';
                
                if (isConfirmed) {
                    buttonClass = 'bg-green-600 ring-4 ring-green-300';
                    statusText = '(Confirmado)';
                } else if (isAbsent) {
                    buttonClass = 'bg-red-600 ring-4 ring-red-300';
                    statusText = '(Ausente)';
                }

                return `
                    <button data-name="${name}" 
                            data-action="${requestedAction}"
                            class="select-name-btn ${buttonClass} text-white py-3 px-3 rounded-lg font-semibold shadow-md transition duration-200 hover:scale-105 transform text-center flex flex-col items-center">
                        <i class="fas fa-user-circle mr-1 text-lg"></i> 
                        ${name}
                        <span class="text-xs font-normal block pt-1">${statusText}</span>
                    </button>
                `;
            }).join('');
            
            namesListContainer.insertAdjacentHTML('beforeend', nameButtons);

            // 2. Renderiza botões de Cancelamento (apenas para nomes registrados pelo userId atual)
            const namesRegisteredByThisUser = [...confirmedUsers, ...absentUsers].filter(user => user.id === userId);
            
            if (namesRegisteredByThisUser.length > 0) {
                // Filtra nomes unicos (caso o mesmo nome apareça em ConfirmedBy e AbsentBy)
                const uniqueNamesRegistered = Array.from(new Set(namesRegisteredByThisUser.map(item => item.name)));

                namesListContainer.insertAdjacentHTML('beforeend', `
                    <p class="col-span-2 mt-4 text-sm font-semibold text-gray-700">Cancelar Seus Registros:</p>
                `);

                uniqueNamesRegistered.forEach(name => {
                    // Determina o status atual para o nome a ser cancelado
                    const isConfirmed = confirmedUsers.some(user => user.name === name);
                    const status = isConfirmed ? 'Presença' : 'Ausência';
                    
                    namesListContainer.insertAdjacentHTML('beforeend', `
                        <button data-name="${name}" 
                                class="col-span-2 mt-2 bg-gray-500 hover:bg-gray-600 text-white py-3 px-4 rounded-lg font-bold transition duration-200 cancel-name-btn text-sm">
                            <i class="fas fa-times-circle mr-2"></i> Cancelar ${status} para ${name}
                        </button>
                    `);
                });
            }
            
            nameSelectionModal.classList.remove('hidden');
        }

        // --- Funções de Manipulação do Firestore para CRONOGRAMA ---
        
        /**
         * Adiciona, remove ou altera o status de presença/ausência do usuário.
         * @param {string} docId - ID do documento do cronograma.
         * @param {string} currentUserId - O ID do usuário atual.
         * @param {string} selectedName - O nome da pessoa (chave única da presença/ausência).
         * @param {'confirm' | 'absent' | 'cancel'} action - A ação a ser executada.
         * @param {string} [justification=''] - Opcional, justificativa da ausência.
         */
        async function updateAttendance(docId, currentUserId, selectedName, action, justification = '') {
            if (!currentUserId || !selectedName) {
                console.warn("Dados incompletos. Não é possível atualizar a presença.");
                return;
            }
            
            let retries = 3;
            let delay = 1000;
            
            for (let i = 0; i < retries; i++) {
                try {
                    const docRef = doc(db, getCronogramaCollectionPath(), docId);
                    const docSnap = await getDoc(docRef);

                    if (docSnap.exists()) {
                        const item = docSnap.data();
                        // ConfirmedBy e AbsentBy são arrays de objetos {id: userId, name: nome_pessoa, justification: motivo}
                        let confirmedUsers = item.confirmedBy || []; 
                        let absentUsers = item.absentBy || []; 

                        // 1. Remove QUALQUER registro existente para este NOME em AMBAS as listas.
                        confirmedUsers = confirmedUsers.filter(user => user.name !== selectedName);
                        absentUsers = absentUsers.filter(user => user.name !== selectedName);

                        const userProfile = { id: currentUserId, name: selectedName };

                        if (action === 'confirm') {
                            // Ação: Confirmar Presença (adiciona o novo registro)
                            confirmedUsers.push(userProfile);
                        } else if (action === 'absent') {
                            // Ação: Informar Ausência (adiciona o novo registro com justificativa opcional)
                            userProfile.justification = justification.trim(); // Adiciona a justificativa
                            absentUsers.push(userProfile);
                        }
                        // Se action é 'cancel', o nome foi removido de ambas as listas e nada é adicionado.

                        // Atualiza o documento no Firestore
                        await updateDoc(docRef, { 
                            confirmedBy: confirmedUsers,
                            absentBy: absentUsers 
                        });
                        return;
                    } else {
                        console.error("Documento de cronograma não encontrado:", docId);
                        return;
                    }
                } catch (e) {
                    if (e.code === 'unavailable' || e.code === 'internal') {
                        console.warn(`Tentativa ${i + 1} falhou. Tentando novamente em ${delay / 1000}s...`);
                        await new Promise(resolve => setTimeout(resolve, delay));
                        delay *= 2; 
                    } else {
                        console.error("Erro ao alternar presença:", e);
                        return;
                    }
                }
            }
            console.error("Falha ao atualizar presença após múltiplas tentativas.");
        }


        /**
         * Envia um novo item para o Firestore. (APENAS ADMIN)
         */
        async function saveCronogramItem(item) {
            if (!userId || !isAdmin) {
                console.warn("Apenas Administradores podem agendar itens.");
                return;
            }
            try {
                const colRef = collection(db, getCronogramaCollectionPath());
                // Inicializa confirmedBy e absentBy como arrays vazios
                const itemWithTimestamp = { 
                    ...item, 
                    confirmedBy: [], 
                    absentBy: [],
                    createdAt: new Date().getTime() 
                }; 
                await addDoc(colRef, itemWithTimestamp);
            } catch (e) {
                console.error("Erro ao adicionar item do cronograma: ", e);
            }
        }

        /**
         * Remove um item do Firestore. (APENAS ADMIN)
         */
        async function deleteCronogramItem(docId) {
            if (!userId || !isAdmin) {
                console.warn("Apenas Administradores podem deletar itens.");
                return;
            }
            try {
                const docRef = doc(db, getCronogramaCollectionPath(), docId);
                await deleteDoc(docRef);
            } catch (e) {
                console.error("Erro ao remover item do cronograma: ", e);
            }
        }

        /**
         * Configura o listener em tempo real para o Cronograma.
         */
        function setupCronogramaFirestoreListener() {
            if (!isAuthReady || !userId || !db) return;

            const colRef = collection(db, getCronogramaCollectionPath());
            const q = query(colRef); 

            onSnapshot(q, (snapshot) => {
                const items = [];

                snapshot.forEach(doc => {
                    items.push({ id: doc.id, ...doc.data() });
                });
                
                // 1. Ordena os itens localmente por Data e Horário (do mais antigo para o mais novo)
                items.sort((a, b) => {
                    const dateTimeA = `${a.date}T${a.time}`;
                    const dateTimeB = `${b.date}T${b.time}`;

                    if (dateTimeA < dateTimeB) return -1;
                    if (dateTimeA > dateTimeB) return 1;

                    // 2. Fallback para createdAt se data/hora forem idênticas
                    return a.createdAt - b.createdAt;
                });
                
                // ATUALIZA CACHE GLOBAL
                cronogramaItems = items;

                renderMonogram(items);

            }, (error) => {
                console.error("Erro no listener do Firestore (Cronograma):", error);
            });
        }
        
        // --- Funções de Renderização e UI ---

        /**
         * Formata a data de YYYY-MM-DD para DD/MM/YYYY.
         */
        function formatDisplayDate(dateString) {
            if (!dateString) return 'Data Desconhecida';
            try {
                const [year, month, day] = dateString.split('-');
                return `${day}/${month}/${year}`;
            } catch {
                return dateString;
            }
        }
        
        /**
         * Renderiza a lista de itens do cronograma.
         */
        function renderMonogram(items) {
            monogramList.innerHTML = '';

            if (items.length === 0) {
                emptyState.classList.remove('hidden');
            } else {
                emptyState.classList.add('hidden');
                items.forEach(item => {
                    const categoryInfo = categoryMap[item.category] || categoryMap.other;
                    const hasDetails = item.details && item.details.trim().length > 0;
                    
                    const confirmedUsers = item.confirmedBy || [];
                    const absentUsers = item.absentBy || [];

                    const confirmationCount = confirmedUsers.length;
                    const absenceCount = absentUsers.length;

                    // Status visual do usuário atual (baseado em quem o userId registrou)
                    const namesConfirmedByMe = confirmedUsers.filter(user => user.id === userId).map(user => user.name);
                    const namesAbsentByMe = absentUsers.filter(user => user.id === userId).map(user => user.name);

                    let visualConfirmationStatus = '';
                    if (namesConfirmedByMe.length > 0 && namesAbsentByMe.length === 0) {
                        visualConfirmationStatus = `<span class="text-green-600 font-bold ml-2"><i class="fas fa-check-circle mr-1"></i> Você confirmou como: ${namesConfirmedByMe.join(', ')}.</span>`;
                    } else if (namesAbsentByMe.length > 0 && namesConfirmedByMe.length === 0) {
                        visualConfirmationStatus = `<span class="text-red-600 font-bold ml-2"><i class="fas fa-user-times mr-1"></i> Você justificou ausência como: ${namesAbsentByMe.join(', ')}.</span>`;
                    } else if (namesConfirmedByMe.length > 0 || namesAbsentByMe.length > 0) {
                        visualConfirmationStatus = `<span class="text-orange-600 font-bold ml-2"><i class="fas fa-exclamation-triangle mr-1"></i> Você registrou: Conf. (${namesConfirmedByMe.join(', ')}) / Aus. (${namesAbsentByMe.join(', ')}).</span>`;
                    } else {
                        visualConfirmationStatus = `<span class="text-gray-500 font-medium ml-2"><i class="fas fa-question-circle mr-1"></i> Resposta Pendente (Seu ID).</span>`;
                    }
                    
                    // Lista de nomes confirmados para exibição
                    const confirmedNamesList = confirmedUsers.map(user => 
                        `<span class="${user.id === userId ? 'font-bold text-blue-700 bg-blue-100 ring-2 ring-blue-500' : 'text-gray-700 bg-gray-100'} px-2 py-0.5 rounded-full inline-block mr-2 mb-1 text-xs">
                            <i class="fas fa-user-check mr-1"></i> ${user.name}
                        </span>`
                    ).join('');
                    
                    // Lista de nomes ausentes para exibição (AGORA INCLUI JUSTIFICATIVA)
                    const absentNamesList = absentUsers.map(user => {
                        const justificationHtml = user.justification ? 
                            `<span class="text-xs italic text-gray-500 block">Justificativa: ${user.justification}</span>` : 
                            '';
                        
                        return `
                        <div class="inline-block mr-3 mb-1">
                            <span class="${user.id === userId ? 'font-bold text-red-700 bg-red-100 ring-2 ring-red-500' : 'text-gray-700 bg-gray-100'} px-2 py-0.5 rounded-full inline-block mr-2 mb-1 text-xs">
                                <i class="fas fa-user-times mr-1"></i> ${user.name}
                            </span>
                            ${justificationHtml}
                        </div>
                        `;
                    }).join('');
                    
                    
                    // Data attributes para o botão de Presença
                    const confirmDataAttrs = `data-id="${item.id}" data-action="confirm"`;
                    // Data attributes para o botão de Ausência
                    const absentDataAttrs = `data-id="${item.id}" data-action="absent"`;
                    
                    // Botão de Deletar (APENAS PARA ADMIN)
                    const deleteButtonHtml = isAdmin ? `
                        <button data-id="${item.id}" data-type="cronogram" class="delete-btn text-red-500 hover:text-red-700 p-2 rounded-full transition duration-150 ease-in-out" title="Deletar Atividade">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    ` : '';


                    const itemHtml = `
                        <div id="item-${item.id}" class="item-card ${categoryInfo.class} p-4 flex flex-col transition duration-150 ease-in-out">
                            
                            <!-- Linha Principal (Título e Data/Hora) -->
                            <div class="flex justify-between items-start w-full mb-3">
                                <div class="flex-1 min-w-0">
                                    <p class="text-lg font-bold text-gray-800">${item.name}</p>
                                    <div class="text-sm flex items-center mt-1 flex-wrap">
                                        <i class="fas fa-calendar-alt mr-2 text-gray-500"></i>
                                        <span class="text-gray-700 font-medium mr-3 whitespace-nowrap">${formatDisplayDate(item.date)} às ${item.time}</span>
                                        
                                        <span class="text-gray-500 font-medium hidden md:inline">|</span>
                                        <i class="${categoryInfo.icon} mx-2 hidden md:inline" style="color:${categoryInfo.color};"></i>
                                        <span style="color:${categoryInfo.color};" class="font-semibold hidden md:inline whitespace-nowrap">${categoryInfo.name}</span>
                                    </div>
                                </div>
                            </div>

                            <!-- Linha de Confirmação e Ações -->
                            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center pt-3 border-t border-gray-200">
                                <!-- Status de Confirmação -->
                                <div class="flex flex-col text-sm font-medium text-gray-700 mb-3 sm:mb-0">
                                    <div class="flex items-center text-base">
                                        <!-- Contagem de Confirmados -->
                                        <i class="fas fa-users mr-1 text-green-600"></i>
                                        <span class="font-bold text-lg text-green-600">${confirmationCount}</span>
                                        <span class="ml-1 mr-4">Confirmado(s)</span>
                                        
                                        <!-- Contagem de Ausentes -->
                                        <i class="fas fa-user-times mr-1 text-red-600"></i>
                                        <span class="font-bold text-lg text-red-600">${absenceCount}</span>
                                        <span class="ml-1">Ausente(s)</span>
                                        
                                        <!-- Botão de Detalhes de Presença (sempre visível) -->
                                        <button data-id="${item.id}" data-type="toggle-details-presence" class="toggle-details-presence-btn text-blue-500 hover:text-blue-700 p-2 rounded-full transition duration-150 ease-in-out ml-2" title="Ver quem confirmou/justificou">
                                            <i class="fas fa-eye"></i>
                                        </button>
                                    </div>
                                    <!-- Status Pessoal -->
                                    <div class="mt-1">${visualConfirmationStatus}</div>
                                </div>

                                <!-- Botões de Ação -->
                                <div class="flex items-center space-x-2 flex-wrap justify-end">
                                    <!-- Botão Confirmar Presença -->
                                    <button ${confirmDataAttrs} class="attendance-btn bg-green-600 hover:bg-green-700 text-white py-2 px-3 rounded-full text-sm font-semibold transition duration-200 hover:scale-105 shadow-md">
                                        <i class="fas fa-user-check mr-1"></i> Confirmar
                                    </button>
                                    
                                    <!-- Botão Informar Ausência -->
                                    <button ${absentDataAttrs} class="attendance-btn bg-red-600 hover:bg-red-700 text-white py-2 px-3 rounded-full text-sm font-semibold transition duration-200 hover:scale-105 shadow-md">
                                        <i class="fas fa-user-times mr-1"></i> Ausente
                                    </button>
                                    
                                    <!-- Botão de Detalhes da Atividade -->
                                    ${hasDetails ? `
                                        <button data-id="${item.id}" data-type="toggle-details-activity" class="toggle-details-activity-btn text-blue-500 hover:text-blue-700 p-2 rounded-full transition duration-150 ease-in-out" title="Mostrar Detalhes da Atividade">
                                            <i class="fas fa-info-circle"></i>
                                        </button>
                                    ` : ''}
                                    
                                    <!-- Botão de Deletar (Condicional) -->
                                    ${deleteButtonHtml}
                                </div>
                            </div>

                            <!-- Área de Detalhes da Atividade (Colapsável) -->
                            <div id="details-activity-${item.id}" class="hidden mt-3 pt-3 border-t border-gray-200 w-full">
                                <p class="text-xs font-semibold text-gray-600 mb-1">Detalhes da Atividade:</p>
                                <div class="bg-blue-50 p-3 rounded-lg text-gray-800 whitespace-pre-wrap text-sm border border-blue-200">
                                    ${item.details.trim()}
                                </div>
                            </div>
                            
                            <!-- Área de Detalhes de Presença (Colapsável) -->
                            <div id="details-presence-${item.id}" class="hidden mt-3 pt-3 border-t border-gray-200 w-full">
                                <p class="text-xs font-semibold text-gray-600 mb-2">Respostas:</p>
                                
                                <p class="text-sm font-medium text-green-700 mb-1">Confirmados (${confirmationCount}):</p>
                                <div class="bg-green-50 p-3 rounded-lg border border-green-200 flex flex-wrap gap-2 mb-3">
                                    ${confirmedNamesList || '<span class="text-gray-500 italic">Ninguém confirmou ainda.</span>'}
                                </div>
                                
                                <p class="text-sm font-medium text-red-700 mb-1">Ausentes (${absenceCount}):</p>
                                <div class="bg-red-50 p-3 rounded-lg border border-red-200 flex flex-wrap gap-2">
                                    ${absentNamesList || '<span class="text-gray-500 italic">Ninguém informou ausência ainda.</span>'}
                                </div>
                            </div>
                        </div>
                    `;
                    monogramList.insertAdjacentHTML('beforeend', itemHtml);
                });
            }
        }
        
        // --- Listeners de Eventos ---
        
        // Listener para o botão de Login/Logout Admin
        adminLoginBtn.addEventListener('click', async () => {
            const enteredKey = adminKeyInput.value.trim();

            if (isAdmin) {
                // Se já é Admin, faz Logout (volta para anônimo)
                await signInAnonymously(auth);
                ADMIN_UID_KEY = null; 
            } else if (enteredKey && enteredKey.length > 5) {
                // Tenta Logar como Admin (salva o UID inserido como a chave temporária)
                ADMIN_UID_KEY = enteredKey;
                
                // Força a reavaliação do status de autenticação/Admin
                if(auth.currentUser.uid === enteredKey) {
                    updateAdminStatus(auth.currentUser);
                } else {
                    // Se o ID inserido for diferente do atual, forçamos um novo login anônimo
                    // e reavaliamos o status. (Geralmente, um ID longo serve como "senha" temporária)
                    await signInAnonymously(auth); 
                }
            } else {
                alert("Por favor, insira um ID de Administrador válido para acesso.");
            }
        });


        // Listener para o formulário de agendamento (Cronograma)
        document.getElementById('add-item-form').addEventListener('submit', (e) => {
            e.preventDefault();
            
            if (!isAdmin) {
                console.error("Acesso negado. Apenas Administradores podem agendar.");
                return;
            }

            const nameInput = document.getElementById('item-name');
            const categoryInput = document.getElementById('item-category');
            const dateInput = document.getElementById('item-date');
            const timeInput = document.getElementById('item-time');
            const detailsInput = document.getElementById('item-details'); 

            const newItem = {
                name: nameInput.value.trim(),
                category: categoryInput.value,
                date: dateInput.value, 
                time: timeInput.value, 
                details: detailsInput.value.trim(), 
            };

            if (newItem.name && newItem.date && newItem.time) {
                saveCronogramItem(newItem);
                nameInput.value = ''; 
                detailsInput.value = ''; // Limpa o campo de detalhes
                nameInput.focus();
            }
        });

        // Lógica para mostrar/ocultar um painel de detalhes
        function toggleDetailsPanel(docId, type, buttonElement) {
            const detailsDiv = document.getElementById(`details-${type}-${docId}`);
            if (detailsDiv) {
                const isHidden = detailsDiv.classList.toggle('hidden');
                
                // Troca o ícone para indicar expansão/contração
                const icon = buttonElement.querySelector('i');
                if (icon) {
                     const defaultIcon = (type === 'activity') ? 'fa-info-circle' : 'fa-eye';
                     const expandedIcon = 'fa-chevron-up';

                     if (isHidden) {
                         icon.classList.remove(expandedIcon);
                         icon.classList.add(defaultIcon);
                     } else {
                         icon.classList.remove(defaultIcon);
                         icon.classList.add(expandedIcon);
                     }
                }
            }
        }


        // Listener de eventos de clique para os botões de ação e modal (delegação)
        document.addEventListener('click', (e) => {
            const deleteButton = e.target.closest('.delete-btn');
            const activityDetailsButton = e.target.closest('.toggle-details-activity-btn');
            const presenceDetailsButton = e.target.closest('.toggle-details-presence-btn');
            const attendanceButton = e.target.closest('.attendance-btn'); // Seletor unificado
            
            // Botões dentro do modal
            const selectNameButton = e.target.closest('.select-name-btn');
            const cancelButton = e.target.closest('.cancel-name-btn'); 
            const closeModalBtn = e.target.closest('#close-modal-btn');
            const finalAbsentBtnClick = e.target.closest('#final-absent-btn');
            const backToNamesBtnClick = e.target.closest('#back-to-names-btn');


            // Lógica para Deletar (APENAS ADMIN)
            if (deleteButton) {
                const docId = deleteButton.dataset.id;
                const docType = deleteButton.dataset.type;
                
                if (isAdmin && docId && docType === 'cronogram') {
                    deleteCronogramItem(docId);
                } else if (!isAdmin) {
                    // Feedback visual se não for Admin
                    console.error("Acesso negado: Somente Administradores podem deletar atividades.");
                }
            }

            // Lógica para Mostrar/Ocultar Detalhes da ATIVIDADE
            if (activityDetailsButton) {
                const docId = activityDetailsButton.dataset.id;
                toggleDetailsPanel(docId, 'activity', activityDetailsButton);
            }
            
            // Lógica para Mostrar/Ocultar Detalhes da PRESENÇA (Quem Confirmou)
            if (presenceDetailsButton) {
                const docId = presenceDetailsButton.dataset.id;
                toggleDetailsPanel(docId, 'presence', presenceDetailsButton);
            }
            
            // Lógica para Abrir Modal de Confirmação/Ausência (attendance-btn)
            if (attendanceButton) {
                const docId = attendanceButton.dataset.id;
                const action = attendanceButton.dataset.action; // 'confirm' ou 'absent'
                
                if (docId && userId && (action === 'confirm' || action === 'absent')) {
                    // Inicializa o modal na visualização de seleção de nomes
                    showNameSelectionView(docId, action); 
                }
            }

            // Lógica para Seleção de Nome (dentro do modal - PASSO 1)
            if (selectNameButton) {
                const docId = currentRsvpContext.docId; // Pega do contexto global
                const selectedName = selectNameButton.dataset.name;
                const action = selectNameButton.dataset.action; 
                
                if (action === 'confirm') {
                    // 1. Confirmar: Salva imediatamente, sem justificativa
                    updateAttendance(docId, userId, selectedName, 'confirm');
                    nameSelectionModal.classList.add('hidden');
                } else if (action === 'absent') {
                    // 2. Ausente: Transiciona para a tela de justificativa
                    showJustificationView(selectedName);
                }
            }
            
            // Lógica para FINALIZAR AUSÊNCIA (PASSO 2, após inserir justificativa)
            if (finalAbsentBtnClick) {
                const docId = currentRsvpContext.docId;
                const selectedName = currentRsvpContext.selectedName;
                const justification = justificationInput.value;
                
                if (docId && userId && selectedName) {
                    // Salva com a justificativa (que pode ser vazia)
                    updateAttendance(docId, userId, selectedName, 'absent', justification); 
                    nameSelectionModal.classList.add('hidden');
                }
            }

            // Lógica para VOLTAR do modal de Justificativa
            if (backToNamesBtnClick) {
                const docId = currentRsvpContext.docId;
                const action = currentRsvpContext.action;
                if(docId && action) {
                    showNameSelectionView(docId, action); // Volta para a tela de seleção
                }
            }

            // Lógica para Cancelar (dentro do modal)
            if (cancelButton) {
                 const docId = currentRsvpContext.docId;
                 const nameToCancel = cancelButton.dataset.name;
                 
                 // Ação 'cancel' remove a entrada do nome em ambas as listas, sem justificativa
                 if (docId && userId && nameToCancel) {
                     updateAttendance(docId, userId, nameToCancel, 'cancel'); 
                     nameSelectionModal.classList.add('hidden');
                 }
            }
            
            // Lógica para Fechar o Modal
            if (closeModalBtn) {
                 nameSelectionModal.classList.add('hidden');
            }
        });

        // Fechar modal ao clicar fora (no overlay)
        nameSelectionModal.addEventListener('click', (e) => {
            if (e.target === nameSelectionModal) {
                 // Apenas fecha se estiver na visualização de seleção de nome (evita fechar no meio da justificativa)
                if (namesListContainer.classList.contains('hidden')) return; 
                nameSelectionModal.classList.add('hidden');
            }
        });

        // --- Início do Aplicativo ---
        window.onload = initializeFirebase;

    </script>
</body>
</html>
