<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SOS Patinhas - Rastreamento</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        /* Tela de Login */
        .login-container {
            background-color: #ffffff;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 400px;
            text-align: center;
        }

        .login-container h1 {
            color: #ff6b6b;
            margin-bottom: 10px;
            font-size: 2rem;
        }

        .login-container p {
            color: #666;
            margin-bottom: 25px;
        }

        .input-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .input-group label {
            display: block;
            color: #333;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .input-group input {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 6px;
            font-size: 1rem;
            outline: none;
            transition: border-color 0.3s;
        }

        .input-group input:focus {
            border-color: #ff6b6b;
        }

        .btn-login {
            width: 100%;
            padding: 12px;
            background-color: #ff6b6b;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .btn-login:hover {
            background-color: #ee5253;
        }

        .error-message {
            color: #ee5253;
            margin-top: 15px;
            font-size: 0.9rem;
            display: none;
        }

        /* Painel Principal (Escondido inicialmente) */
        .dashboard-container {
            display: none;
            width: 100%;
            max-width: 1000px;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            overflow: hidden;
            margin: 20px;
        }

        header {
            background-color: #ff6b6b;
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h2 {
            font-size: 1.5rem;
        }

        .btn-logout {
            background-color: transparent;
            color: white;
            border: 2px solid white;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s;
        }

        .btn-logout:hover {
            background-color: white;
            color: #ff6b6b;
        }

        .content {
            display: grid;
            grid-template-columns: 1fr 2fr;
            height: 550px;
        }

        /* Lista de Animais */
        .animals-list {
            border-right: 1px solid #eee;
            overflow-y: auto;
            background: #fafafa;
        }

        .animal-item {
            padding: 20px;
            border-bottom: 1px solid #eee;
            cursor: pointer;
            transition: background 0.2s;
        }

        .animal-item:hover {
            background: #f1f2f6;
        }

        .animal-item.active {
            background: #ffeaa7;
            border-left: 5px solid #ff6b6b;
        }

        .animal-name {
            font-weight: bold;
            color: #2d3436;
            font-size: 1.1rem;
        }

        .animal-status {
            font-size: 0.85rem;
            color: #2ed573;
            margin-top: 5px;
        }

        /* Área do Mapa/Rastreamento */
        .tracking-view {
            padding: 30px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .map-placeholder {
            background-color: #dfe4ea;
            flex-grow: 1;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: #57606f;
            margin-bottom: 20px;
            border: 2px dashed #a4b0be;
            position: relative;
        }

        .map-grid {
            position: absolute;
            width: 100%;
            height: 100%;
            background-image: radial-gradient(#ced6e0 1px, transparent 1px);
            background-size: 20px 20px;
        }

        .pet-marker {
            width: 20px;
            height: 20px;
            background-color: #ff6b6b;
            border: 3px solid white;
            border-radius: 50%;
            position: absolute;
            box-shadow: 0 0 10px rgba(0,0,0,0.3);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0.7); }
            70% { transform: scale(1.1); box-shadow: 0 0 0 10px rgba(255,107,107,0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,107,0); }
        }

        .details-box {
            background: #f1f2f6;
            padding: 15px;
            border-radius: 8px;
        }

        .details-box h3 {
            margin-bottom: 8px;
            color: #2d3436;
        }

        .details-box p {
            font-size: 0.95rem;
            color: #57606f;
            line-height: 1.4;
        }
    </style>
</head>
<body>

    <div class="login-container" id="loginScreen">
        <h1>SOS Patinhas 🐾</h1>
        <p>Insira suas credenciais para acessar o rastreador</p>
        
        <div class="input-group">
            <label for="username">Usuário</label>
            <input type="text" id="username" placeholder="Digite seu usuário">
        </div>
        
        <div class="input-group">
            <label for="password">Senha</label>
            <input type="password" id="password" placeholder="Digite sua senha">
        </div>
        
        <button class="btn-login" onclick="validarLogin()">Entrar no Sistema</button>
        <div class="error-message" id="errorMessage">Usuário ou senha incorretos!</div>
    </div>

    <div class="dashboard-container" id="dashboardScreen">
        <header>
            <h2>SOS Patinhas 🐾 - Sistema de Rastreamento</h2>
            <button class="btn-logout" onclick="fazerLogout()">Sair</button>
        </header>
        
        <div class="content">
            <div class="animals-list" id="animalsList">
                </div>

            <div class="tracking-view">
                <div class="map-placeholder">
                    <div class="map-grid"></div>
                    <div class="pet-marker" id="petMarker" style="top: 50%; left: 50%;"></div>
                    <span style="position: relative; z-index: 1; font-weight: bold; background: rgba(255,255,255,0.8); padding: 5px 10px; border-radius: 20px;">
                        Visualização do GPS em Tempo Real
                    </span>
                </div>
                
                <div class="details-box">
                    <h3 id="detalheNome">Selecione um animal</h3>
                    <p id="detalheInfo">Clique em um dos animais da lista para ver os detalhes da coleira GPS e a última localização atualizada.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Dados de simulação
        const USUARIO_CORRETO = "admin";
        const SENHA_CORRETA = "1234";

        const animais = [
            { id: 1, nome: "Pipoca (Cão)", status: "Sinal Forte", local: "Rua das Flores, nº 120", bateria: "88%", top: "35%", left: "42%" },
            { id: 2, nome: "Mel (Gato)", status: "Sinal Médio", local: "Av. Central, Parque dos Pássaros", bateria: "45%", top: "65%", left: "70%" },
            { id: 3, nome: "Thor (Cão)", status: "Sinal Forte", local: "Alamedas Norte, Condomínio Verde", bateria: "95%", top: "20%", left: "25%" }
        ];

        // Função de validação do Login
        function validarLogin() {
            const user = document.getElementById("username").value;
            const pass = document.getElementById("password").value;
            const error = document.getElementById("errorMessage");

            if (user === USUARIO_CORRETO && pass === SENHA_CORRETA) {
                document.getElementById("loginScreen").style.display = "none";
                document.getElementById("dashboardScreen").style.display = "block";
                carregarAnimais();
            } else {
                error.style.display = "block";
            }
        }

        // Função para carregar a lista de pets
        function carregarAnimais() {
            const lista = document.getElementById("animalsList");
            lista.innerHTML = "";

            animais.forEach((anim, index) => {
                const item = document.createElement("div");
                item.className = `animal-item ${index === 0 ? 'active' : ''}`;
                item.onclick = () => selecionarAnimal(anim, item);
                
                item.innerHTML = `
                    <div class="animal-name">${anim.nome}</div>
                    <div class="animal-status">🟢 ${anim.status} | Bateria: ${anim.bateria}</div>
                `;
                lista.appendChild(item);
            });

            // Seleciona o primeiro por padrão
            selecionarAnimal(animais[0], lista.firstChild);
        }

        // Função ao clicar em um animal
        function selecionarAnimal(anim, elemento) {
            // Remove classe ativa dos outros
            document.querySelectorAll('.animal-item').forEach(el => el.classList.remove('active'));
            // Adiciona no atual
            elemento.classList.add('active');

            // Atualiza painel de informações
            document.getElementById("detalheNome").innerText = anim.nome;
            document.getElementById("detalheInfo").innerText = `Última localização vista: ${anim.local}. Status do rastreador: operacional com ${anim.bateria} de carga restante.`;

            // Move o marcador no "mapa" simulado
            const marcador = document.getElementById("petMarker");
            marcador.style.top = anim.top;
            marcador.style.left = anim.left;
        }

        // Função para deslogar
        function fazerLogout() {
            document.getElementById("username").value = "";
            document.getElementById("password").value = "";
            document.getElementById("errorMessage").style.display = "none";
            document.getElementById("dashboardScreen").style.display = "none";
            document.getElementById("loginScreen").style.display = "block";
        }
    </script>
</body>
</html>
