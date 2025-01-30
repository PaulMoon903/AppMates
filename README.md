<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aprende a Multiplicar y Dividir</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-r from-blue-400 to-purple-500 flex flex-col items-center justify-center min-h-screen p-5 text-white">
    
    <!-- Pantalla de selección -->
    <div id="menu" class="bg-white p-8 rounded-2xl shadow-lg text-center w-full max-w-md">
        <h1 class="text-3xl font-extrabold text-gray-800 mb-6">Elige un modo</h1>
        <button onclick="startGame('multiplicacion')" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg mb-3 transition">Multiplicación</button>
        <button onclick="startGame('division')" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-lg mb-3 transition">División</button>
        <button onclick="startGame('modoPro')" class="w-full bg-purple-600 hover:bg-purple-700 text-white font-bold py-3 rounded-lg transition">Modo Pro</button>
    </div>

    <!-- Pantalla de juego -->
    <div id="game" class="hidden bg-white p-8 rounded-2xl shadow-lg text-center w-full max-w-md">
        <h2 id="question" class="text-2xl font-bold text-gray-800 mb-6"></h2>
        <input type="number" id="answer" class="border-2 border-gray-300 rounded-lg p-3 text-xl text-center w-32 text-gray-900 focus:outline-none focus:ring-2 focus:ring-blue-400" placeholder="Tu respuesta">
        <button onclick="checkAnswer()" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg ml-2 transition">Enviar</button>
        <p id="feedback" class="text-lg mt-4 font-semibold"></p>
        <p id="score" class="text-xl font-bold text-gray-800 mt-4">Puntos: 0</p>
        <button onclick="nextQuestion()" class="hidden bg-gray-600 hover:bg-gray-800 text-white font-bold py-3 px-6 rounded-lg mt-4 transition">Siguiente</button>
        <button onclick="exitGame()" class="bg-red-500 hover:bg-red-700 text-white font-bold py-3 px-6 rounded-lg mt-4 transition">Salir</button>
    </div>

    <script>
        let currentOperation;
        let num1, num2, correctAnswer;
        let score = 0;
        let startTime;

        function startGame(mode) {
            document.getElementById("menu").classList.add("hidden");
            document.getElementById("game").classList.remove("hidden");

            currentOperation = mode;
            score = 0;
            document.getElementById("score").textContent = "Puntos: 0";
            generateQuestion();
        }

        function generateQuestion() {
            document.getElementById("feedback").textContent = "";
            document.getElementById("answer").value = "";
            document.querySelector("#game button:nth-of-type(2)").classList.add("hidden");

            num1 = Math.floor(Math.random() * 10) + 1;
            num2 = Math.floor(Math.random() * 10) + 1;

            if (currentOperation === "multiplicacion") {
                correctAnswer = num1 * num2;
                document.getElementById("question").textContent = `¿Cuánto es ${num1} × ${num2}?`;
            } else if (currentOperation === "division") {
                correctAnswer = num1;
                let result = num1 * num2;
                document.getElementById("question").textContent = `¿Cuánto es ${result} ÷ ${num2}?`;
            } else if (currentOperation === "modoPro") {
                if (Math.random() < 0.5) {
                    correctAnswer = num1 * num2;
                    document.getElementById("question").textContent = `¿Cuánto es ${num1} × ${num2}?`;
                } else {
                    correctAnswer = num1;
                    let result = num1 * num2;
                    document.getElementById("question").textContent = `¿Cuánto es ${result} ÷ ${num2}?`;
                }
            }

            startTime = new Date().getTime(); // Inicia el tiempo
        }

        function checkAnswer() {
            let userAnswer = parseInt(document.getElementById("answer").value);
            let feedback = document.getElementById("feedback");
            let nextButton = document.querySelector("#game button:nth-of-type(2)");

            let endTime = new Date().getTime();
            let timeTaken = Math.floor((endTime - startTime) / 1000); // Tiempo en segundos

            if (userAnswer === correctAnswer) {
                let pointsEarned = Math.max(100 - timeTaken, 10); // Máximo 100 puntos, mínimo 10
                score += pointsEarned;
                document.getElementById("score").textContent = `Puntos: ${score}`;
                feedback.textContent = `¡Correcto! 🎉 +${pointsEarned} puntos`;
                feedback.classList.remove("text-red-600");
                feedback.classList.add("text-green-600");
            } else {
                feedback.innerHTML = `❌ Incorrecto. La respuesta correcta es ${correctAnswer}.`;
                feedback.classList.remove("text-green-600");
                feedback.classList.add("text-red-600");
            }

            nextButton.classList.remove("hidden");
        }

        function nextQuestion() {
            generateQuestion();
        }

        function exitGame() {
            document.getElementById("game").classList.add("hidden");
            document.getElementById("menu").classList.remove("hidden");
        }
    </script>

</body>
</html>
