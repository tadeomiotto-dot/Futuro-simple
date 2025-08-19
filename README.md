[Qwen_html_20250818_2m57k3uvr.html](https://github.com/user-attachments/files/21845907/Qwen_html_20250818_2m57k3uvr.html)
<!DOCTYPE html>
<html>
<head>
    <title>Future Quest</title>
    <style>
        body {
            font-family: "Arial", sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #6dd5ed, #2193b0);
            color: white;
            padding: 20px;
            margin: 0;
            height: 100vh;
            overflow: hidden;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }
        h1, h2 {
            font-size: 2rem;
            margin-bottom: 20px;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .header div {
            font-size: 1.2rem;
            padding: 10px;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(5px);
        }
        .question {
            font-size: 1.5rem;
            margin-bottom: 20px;
        }
        button {
            background: #ff7f50;
            color: white;
            font-size: 1rem;
            padding: 10px 20px;
            margin: 10px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
        }
        button:hover {
            transform: scale(1.1);
            background: #ff6347;
        }
        .feedback {
            font-size: 1.2rem;
            margin-top: 20px;
            padding: 10px;
            border-radius: 10px;
            display: none;
        }
        .feedback.correct {
            background: rgba(0, 255, 0, 0.3);
        }
        .feedback.incorrect {
            background: rgba(255, 0, 0, 0.3);
        }
        .leaderboard table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        .leaderboard table th, .leaderboard table td {
            padding: 10px;
            border: 1px solid rgba(255, 255, 255, 0.5);
            text-align: center;
        }
        .back-button {
            background: #6a11cb;
            color: white;
            font-size: 1rem;
            padding: 10px 20px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
        }
        .back-button:hover {
            transform: scale(1.1);
            background: #471ca9;
        }
        input[type="text"] {
            padding: 10px;
            font-size: 1rem;
            border: none;
            border-radius: 10px;
            margin-bottom: 20px;
            width: 80%;
            background: rgba(255, 255, 255, 0.2);
            color: white;
        }
        input[type="text"]::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }
    </style>
</head>
<body>
    <!-- Pantalla de Inicio -->
    <div id="start-screen" class="container">
        <h1>Bienvenido a Future Quest</h1>
        <p>Este juego pondrá a prueba tu conocimiento sobre el futuro simple (will + verbo).</p>
        <p><strong>Instrucciones:</strong></p>
        <ul>
            <li>Responde 10 preguntas de opción múltiple.</li>
            <li>Cada respuesta correcta suma 100 puntos.</li>
            <li>Cada respuesta incorrecta resta 100 puntos.</li>
            <li>¡Completa todas las preguntas antes de que se acabe el tiempo!</li>
        </ul>
        <p><strong>Grupo:</strong> Alejo Gomez, Bautista Tubio, Bautista Guardia, Maximo Barboni, Tadeo Miotto</p>
        <input type="text" id="player-name" placeholder="Ingresa tu nombre" />
        <button onclick="startGame()">Comenzar Juego</button>
    </div>

    <!-- Juego Principal -->
    <div id="game" class="container" style="display:none;">
        <div class="header">
            <div id="timer">Tiempo: 00:00</div>
            <div id="score">Puntuación: 0</div>
        </div>
        <div id="question" class="question"></div>
        <div id="options"></div>
        <div id="feedback" class="feedback"></div>
    </div>

    <!-- Pantalla Final -->
    <div id="final-screen" class="container" style="display:none;">
        <h1>¡Muchas gracias por su atención!</h1>
        <h1>¡Un aplauso para el grupo n°6! 👏👏👏</h1>
        <p><strong>Puntuación final: <span id="final-score">0</span>/1000</strong></p>
        <div class="leaderboard">
            <h2>Tabla de Posiciones</h2>
            <table border="1" id="leaderboard-table">
                <tr>
                    <th>Lugar</th>
                    <th>Nombre</th>
                    <th>Puntuación</th>
                </tr>
            </table>
        </div>
        <button class="back-button" onclick="restartGame()">Volver al Inicio</button>
    </div>

    <script>
        const questions = [
            {
                q: "¿Qué harás mañana?",
                options: ["Iré a la escuela", "Iría al parque", "Visitaré a mi abuela", "Fui al centro comercial"],
                answer: 2
            },
            {
                q: "¿Ella te llamará más tarde?",
                options: ["Sí, ella me llama", "No, no lo hará", "Sí, está llamando", "No, no llamó"],
                answer: 1
            },
            {
                q: "¿Adónde viajarán el próximo verano?",
                options: ["Viajaron a España", "Viajarán a Japón", "Van a Francia", "Van a Italia"],
                answer: 1
            },
            {
                q: "¿Lloverá mañana?",
                options: ["Sí, llueve mucho", "No, no lloverá", "Sí, está lloviendo", "No, no llovió"],
                answer: 1
            },
            {
                q: "¿Quién te ayudará con la tarea?",
                options: ["Mi hermano me ayudará", "Mi hermano me ayuda", "Mi hermano está ayudando", "Mi hermano ayudó"],
                answer: 0
            },
            {
                q: "¿Cuándo empezará el concierto?",
                options: ["Empieza a las 8 PM", "Empezará a las 7 PM", "Empezó a las 7 PM", "Está empezando ahora"],
                answer: 1
            },
            {
                q: "¿Aprobarás el examen?",
                options: ["Sí, ya aprobé", "No, no estoy aprobando", "Sí, lo aprobaré", "No, no lo apruebo"],
                answer: 2
            },
            {
                q: "¿Cómo irás a la fiesta?",
                options: ["Voy en coche", "Iré en bicicleta", "Estoy caminando", "Fui en autobús"],
                answer: 1
            },
            {
                q: "¿Qué comerá él para la cena?",
                options: ["Come pizza", "Comerá sopa", "Está comiendo ahora", "Comió pasta"],
                answer: 1
            },
            {
                q: "¿Ganaremos el juego?",
                options: ["Sí, siempre ganamos", "No, perdimos", "¡Sí, lo ganaremos!", "No, estamos perdiendo"],
                answer: 2
            }
        ];

        let playerName = "";
        let current = 0;
        let score = 0;
        let timer = 0;
        let leaderboard = [];
        let interval;

        function startGame() {
            playerName = document.getElementById("player-name").value.trim();
            if (!playerName) {
                alert("Por favor, ingresa tu nombre.");
                return;
            }

            document.getElementById("start-screen").style.display = "none";
            document.getElementById("game").style.display = "block";

            score = 0;
            current = 0;
            timer = 0;
            updateScore();
            startTimer();
            showQuestion();
        }

        function startTimer() {
            interval = setInterval(() => {
                timer++;
                const minutes = Math.floor(timer / 60).toString().padStart(2, "0");
                const seconds = (timer % 60).toString().padStart(2, "0");
                document.getElementById("timer").innerText = `Tiempo: ${minutes}:${seconds}`;
            }, 1000);
        }

        function updateScore() {
            document.getElementById("score").innerText = `Puntuación: ${score}`;
        }

        function showQuestion() {
            const q = questions[current];
            document.getElementById("question").innerHTML = `<h2>${q.q}</h2>`;
            let opts = "";
            q.options.forEach((opt, i) => {
                opts += `<button onclick="check(${i})">${opt}</button>`;
            });
            document.getElementById("options").innerHTML = opts;
            document.getElementById("feedback").style.display = "none";
        }

        function check(choice) {
            const q = questions[current];
            const feedback = document.getElementById("feedback");
            if (choice === q.answer) {
                score += 100;
                feedback.className = "feedback correct";
                feedback.innerHTML = "¡Correcto! +100 puntos";
            } else {
                score -= 100;
                feedback.className = "feedback incorrect";
                feedback.innerHTML = "Incorrecto. Inténtalo de nuevo.";
            }
            feedback.style.display = "block";
            updateScore();

            setTimeout(() => {
                current++;
                if (current < questions.length) {
                    showQuestion();
                } else {
                    endGame();
                }
            }, 1500);
        }

        function endGame() {
            clearInterval(interval);
            document.getElementById("game").style.display = "none";
            document.getElementById("final-screen").style.display = "block";
            document.getElementById("final-score").innerText = score;

            // Agregar jugador al leaderboard
            leaderboard.push({ name: playerName, score: score });
            leaderboard.sort((a, b) => b.score - a.score); // Ordenar por puntuación

            // Mostrar leaderboard
            const table = document.getElementById("leaderboard-table");
            table.innerHTML = "<tr><th>Lugar</th><th>Nombre</th><th>Puntuación</th></tr>";
            leaderboard.forEach((player, index) => {
                const row = `<tr><td>${index + 1}</td><td>${player.name}</td><td>${player.score}</td></tr>`;
                table.innerHTML += row;
            });
        }

        function restartGame() {
            document.getElementById("final-screen").style.display = "none";
            document.getElementById("start-screen").style.display = "block";
            document.getElementById("player-name").value = "";
        }
    </script>
</body>
</html>
