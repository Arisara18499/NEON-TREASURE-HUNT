<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Treasure Hunt Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(45deg, #0a0a0a, #1a1a2e, #16213e);
            min-height: 100vh;
            color: #fff;
            overflow-x: hidden;
        }

        .neon-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 20% 50%, #ff006e22 0%, transparent 50%),
                        radial-gradient(circle at 80% 20%, #00f5ff22 0%, transparent 50%),
                        radial-gradient(circle at 40% 80%, #8338ec22 0%, transparent 50%);
            z-index: -1;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            position: relative;
        }

        .header {
            text-align: center;
            margin-bottom: 20px;
        }

        .title {
            font-size: 2.5rem;
            font-weight: bold;
            text-shadow: 0 0 20px #00f5ff, 0 0 40px #00f5ff;
            margin-bottom: 10px;
            animation: pulse 2s ease-in-out infinite alternate;
        }

        @keyframes pulse {
            from { text-shadow: 0 0 20px #00f5ff, 0 0 40px #00f5ff; }
            to { text-shadow: 0 0 10px #00f5ff, 0 0 20px #00f5ff; }
        }

        .subtitle {
            font-size: 1.1rem;
            color: #ff006e;
            text-shadow: 0 0 10px #ff006e;
        }

        .team-selection {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 2px solid #00f5ff;
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 0 30px rgba(0, 245, 255, 0.3);
        }

        .team-input-section {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
        }

        .team-input {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #8338ec;
            border-radius: 10px;
            padding: 10px 15px;
            color: #fff;
            font-size: 1rem;
            outline: none;
            width: 150px;
        }

        .team-input::placeholder {
            color: #aaa;
        }

        .team-input:focus {
            border-color: #00f5ff;
            box-shadow: 0 0 20px rgba(0, 245, 255, 0.5);
        }

        .diamonds-input {
            width: 100px;
        }

        .add-team-btn {
            background: linear-gradient(45deg, #8338ec, #ff006e);
            border: none;
            border-radius: 10px;
            padding: 10px 20px;
            color: #fff;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .add-team-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px rgba(131, 56, 236, 0.5);
        }

        .teams-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-bottom: 15px;
        }

        .team-btn {
            background: linear-gradient(45deg, #1a1a2e, #16213e);
            border: 2px solid #8338ec;
            border-radius: 10px;
            padding: 12px;
            color: #fff;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            position: relative;
            font-size: 0.9rem;
        }

        .team-btn:hover {
            transform: scale(1.02);
            box-shadow: 0 0 15px rgba(131, 56, 236, 0.5);
        }

        .team-btn.selected {
            background: linear-gradient(45deg, #8338ec, #ff006e);
            box-shadow: 0 0 25px rgba(131, 56, 236, 0.8);
        }

        .team-name {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .team-diamonds {
            font-size: 1rem;
            color: #00f5ff;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .treasure-card {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 2px solid #8338ec;
            border-radius: 15px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .treasure-card:hover {
            transform: scale(1.02);
            box-shadow: 0 0 25px rgba(131, 56, 236, 0.6);
        }

        .treasure-card.answered {
            background: linear-gradient(45deg, #00f5ff22, #ff006e22);
            border-color: #00f5ff;
        }

        .card-number {
            position: absolute;
            top: 5px;
            left: 5px;
            background: linear-gradient(45deg, #ff006e, #8338ec);
            width: 25px;
            height: 25px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 0.9rem;
        }

        .card-topic {
            font-size: 1.2rem;
            font-weight: bold;
            color: #00f5ff;
            text-shadow: 0 0 10px #00f5ff;
        }

        .card-icon {
            font-size: 2rem;
            margin: 10px 0;
        }

        .game-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 2px solid #00f5ff;
            border-radius: 12px;
            padding: 12px;
            margin-bottom: 15px;
            font-size: 1rem;
        }

        .current-team {
            font-weight: bold;
            color: #ff006e;
            text-shadow: 0 0 10px #ff006e;
        }

        .message-area {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 2px solid #8338ec;
            border-radius: 12px;
            padding: 12px;
            text-align: center;
            margin-bottom: 15px;
            min-height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .message {
            font-size: 1rem;
            font-weight: bold;
        }

        .success {
            color: #00ff00;
            text-shadow: 0 0 10px #00ff00;
        }

        .error {
            color: #ff0040;
            text-shadow: 0 0 10px #ff0040;
        }

        .leaderboard {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 2px solid #ff006e;
            border-radius: 15px;
            padding: 15px;
            margin-top: 20px;
        }

        .leaderboard h3 {
            text-align: center;
            margin-bottom: 12px;
            color: #ff006e;
            text-shadow: 0 0 10px #ff006e;
            font-size: 1.3rem;
        }

        /* Timer Styles */
        .timer-container {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.1);
            border: 3px solid #ff006e;
            border-radius: 15px;
            padding: 15px;
            text-align: center;
            min-width: 120px;
        }

        .timer-display {
            font-size: 2rem;
            font-weight: bold;
            color: #00f5ff;
            text-shadow: 0 0 15px #00f5ff;
            margin-bottom: 5px;
        }

        .timer-display.warning {
            color: #ff0040;
            text-shadow: 0 0 15px #ff0040;
            animation: timerWarning 0.5s ease-in-out infinite alternate;
        }

        @keyframes timerWarning {
            from { transform: scale(1); }
            to { transform: scale(1.1); }
        }

        .timer-text {
            font-size: 0.9rem;
            color: #fff;
        }

        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background: linear-gradient(45deg, #1a1a2e, #16213e);
            margin: 5% auto;
            padding: 30px;
            border: 3px solid #00f5ff;
            border-radius: 20px;
            width: 90%;
            max-width: 600px;
            text-align: center;
            box-shadow: 0 0 50px rgba(0, 245, 255, 0.5);
            animation: modalSlideIn 0.3s ease-out;
            position: relative;
        }

        @keyframes modalSlideIn {
            from {
                transform: translateY(-50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .question-text {
            font-size: 1.4rem;
            color: #00f5ff;
            text-shadow: 0 0 15px #00f5ff;
            margin-bottom: 30px;
            font-weight: bold;
        }

        .answer-options {
            display: grid;
            gap: 15px;
            margin-bottom: 20px;
        }

        .answer-option {
            background: linear-gradient(45deg, #8338ec, #ff006e);
            border: none;
            border-radius: 15px;
            padding: 15px 20px;
            color: #fff;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .answer-option:hover {
            transform: scale(1.05);
            box-shadow: 0 0 25px rgba(131, 56, 236, 0.8);
        }

        .answer-option:active {
            transform: scale(0.95);
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            position: absolute;
            top: 15px;
            right: 25px;
        }

        .close:hover {
            color: #ff006e;
        }

        /* Result Modal */
        .result-modal {
            display: none;
            position: fixed;
            z-index: 1001;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.9);
            backdrop-filter: blur(10px);
        }

        .result-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            padding: 40px;
            border-radius: 20px;
            min-width: 400px;
        }

        .result-text {
            font-size: 3rem;
            font-weight: bold;
            margin-bottom: 20px;
            text-shadow: 0 0 30px currentColor;
            animation: resultPulse 0.6s ease-in-out;
        }

        .result-correct {
            color: #00ff00;
            background: radial-gradient(circle, rgba(0, 255, 0, 0.2) 0%, transparent 70%);
        }

        .result-wrong {
            color: #ff0040;
            background: radial-gradient(circle, rgba(255, 0, 64, 0.2) 0%, transparent 70%);
        }

        .result-timeout {
            color: #ff8800;
            background: radial-gradient(circle, rgba(255, 136, 0, 0.2) 0%, transparent 70%);
        }

        @keyframes resultPulse {
            0% { transform: scale(0.5); opacity: 0; }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); opacity: 1; }
        }

        .result-details {
            font-size: 1.3rem;
            margin-top: 20px;
            color: #fff;
        }

        @media (max-width: 1024px) {
            .teams-grid {
                grid-template-columns: repeat(4, 1fr);
            }
            .game-board {
                grid-template-columns: repeat(2, 1fr);
            }
            .timer-container {
                position: relative;
                margin-bottom: 15px;
            }
        }

        @media (max-width: 768px) {
            .teams-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            .game-board {
                grid-template-columns: 1fr;
            }
            .title {
                font-size: 2rem;
            }
            .team-input-section {
                flex-direction: column;
            }
            .team-input {
                width: 100%;
            }
            .modal-content {
                width: 95%;
                margin: 10% auto;
                padding: 20px;
            }
            .question-text {
                font-size: 1.2rem;
            }
            .result-text {
                font-size: 2rem;
            }
            .result-content {
                min-width: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="neon-bg"></div>
    
    <div class="container">
        <div class="header">
            <h1 class="title">💎 NEON TREASURE HUNT 💎</h1>
            <p class="subtitle">Reading Comprehension Adventure Game</p>
        </div>

        <div class="team-selection">
            <h3 style="text-align: center; margin-bottom: 15px; color: #00f5ff; text-shadow: 0 0 10px #00f5ff;">Create & Select Teams</h3>
            <div class="team-input-section">
                <input type="text" class="team-input" id="teamNameInput" placeholder="Team name..." maxlength="20">
                <input type="number" class="team-input diamonds-input" id="startingDiamondsInput" placeholder="Start 💎" min="0" max="1000" value="50">
                <button class="add-team-btn" onclick="addTeam()">Add Team</button>
            </div>
            <div class="teams-grid" id="teamsGrid">
                <!-- Teams will be generated here -->
            </div>
        </div>

        <div class="game-info">
            <div class="current-team" id="currentTeam">Add teams and select one to start!</div>
            <div style="color: #00f5ff; font-weight: bold;">Correct: +10💎 | Wrong: -5💎 | Overtime: -1💎/sec after 15s</div>
        </div>

        <div class="message-area">
            <div class="message" id="messageArea">Add your teams and start the treasure hunt!</div>
        </div>

        <div class="game-board" id="gameBoard">
            <!-- Treasure cards will be generated here -->
        </div>

        <div class="leaderboard">
            <h3>🏆 LEADERBOARD</h3>
            <div id="leaderboardContent">
                Add teams to see the leaderboard!
            </div>
        </div>
    </div>

    <!-- Question Modal -->
    <div id="questionModal" class="modal">
        <div class="modal-content">
            <div class="timer-container">
                <div class="timer-display" id="timerDisplay">15</div>
                <div class="timer-text">seconds</div>
            </div>
            <span class="close" onclick="closeModal()">&times;</span>
            <div class="question-text" id="questionText"></div>
            <div class="answer-options" id="answerOptions"></div>
        </div>
    </div>

    <!-- Result Modal -->
    <div id="resultModal" class="result-modal">
        <div class="result-content" id="resultContent">
            <div class="result-text" id="resultText"></div>
            <div class="result-details" id="resultDetails"></div>
        </div>
    </div>

    <script>
        // Game data
        const questions = [
            {
                topic: "Mina's Hobby",
                icon: "🏹",
                question: "What is Mina's hobby?",
                options: ["Swimming", "Archery", "Dancing", "Cooking"],
                correct: 1
            },
            {
                topic: "Competition Experience",
                icon: "🏆",
                question: "How long has she competed?",
                options: ["One year", "A few months", "Many years", "Never competed"],
                correct: 2
            },
            {
                topic: "Tournament Success",
                icon: "🥇",
                question: "What has she won?",
                options: ["Nothing yet", "A few tournaments", "One medal", "World championship"],
                correct: 1
            },
            {
                topic: "Mina's Origin",
                icon: "🇰🇷",
                question: "Which country does Mina come from?",
                options: ["Japan", "China", "Korea", "Thailand"],
                correct: 2
            },
            {
                topic: "Olympic Achievement",
                icon: "🥇",
                question: "What did Korean archers win at Rio Olympics?",
                options: ["Silver medals", "Bronze medals", "Gold medals", "No medals"],
                correct: 2
            },
            {
                topic: "Olympic Categories",
                icon: "🎯",
                question: "How many categories did they win?",
                options: ["Two categories", "Three categories", "All four categories", "One category"],
                correct: 2
            },
            {
                topic: "Archery Target",
                icon: "🎯",
                question: "What do archers shoot at?",
                options: ["A moving target", "A stationary target", "Multiple targets", "No specific target"],
                correct: 1
            },
            {
                topic: "Target Rings",
                icon: "⭕",
                question: "How many rings are on the target?",
                options: ["Five rings", "Eight rings", "Ten rings", "Twelve rings"],
                correct: 2
            },
            {
                topic: "Winning Condition",
                icon: "🏆",
                question: "Who wins the competition?",
                options: ["The fastest archer", "The archer with the most points", "The strongest archer", "The youngest archer"],
                correct: 1
            },
            {
                topic: "Andrew's Hobby",
                icon: "📍",
                question: "What is Andrew's hobby?",
                options: ["Photography", "Hiking", "Geocaching", "Bird watching"],
                correct: 2
            },
            {
                topic: "Geocaching Experience",
                icon: "⏰",
                question: "How long has he been geocaching?",
                options: ["Five years", "More than ten years", "Two years", "One year"],
                correct: 1
            },
            {
                topic: "Required Equipment",
                icon: "📱",
                question: "What equipment do you need?",
                options: ["Camera only", "GPS receivers/smartphone", "Compass only", "Map only"],
                correct: 1
            },
            {
                topic: "Geocache Contents",
                icon: "📦",
                question: "What can you find in a geocache?",
                options: ["Only papers", "CDs, books, USBs, money", "Food items", "Clothes"],
                correct: 1
            },
            {
                topic: "Logbook Purpose",
                icon: "📝",
                question: "What is a logbook used for?",
                options: ["Writing stories", "Record of participants who found the cache", "Drawing pictures", "Taking notes"],
                correct: 1
            }
        ];

        // Game state
        let teams = [];
        let teamScores = {};
        let selectedTeam = null;
        let answeredCards = [];
        let currentQuestion = null;
        let timer = null;
        let timeLeft = 15;
        let questionStartTime = null;

        // Audio context for sound effects
        let audioContext;
        
        function initAudio() {
            if (!audioContext) {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        function playSound(frequency, duration, type = 'sine') {
            try {
                initAudio();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.setValueAtTime(frequency, audioContext.currentTime);
                oscillator.type = type;
                
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + duration);
                
                oscillator.start(audioContext.currentTime);
                oscillator.stop(audioContext.currentTime + duration);
            } catch (e) {
                console.log('Audio not supported');
            }
        }

        function playCorrectSound() {
            playSound(523, 0.2); // C
            setTimeout(() => playSound(659, 0.2), 100); // E
            setTimeout(() => playSound(784, 0.3), 200); // G
        }

        function playWrongSound() {
            playSound(400, 0.3, 'sawtooth');
            setTimeout(() => playSound(300, 0.4, 'sawtooth'), 150);
        }

        function playClickSound() {
            playSound(800, 0.1);
        }

        function playWarningSound() {
            playSound(1000, 0.1, 'square');
        }

        function playTimeoutSound() {
            playSound(200, 0.5, 'sawtooth');
        }

        // Timer functions
        function startTimer() {
            timeLeft = 15;
            questionStartTime = Date.now();
            updateTimerDisplay();
            
            timer = setInterval(() => {
                timeLeft--;
                updateTimerDisplay();
                
                if (timeLeft <= 5 && timeLeft > 0) {
                    playWarningSound();
                }
                
                if (timeLeft <= 0) {
                    handleTimeout();
                }
            }, 1000);
        }

        function updateTimerDisplay() {
            const timerDisplay = document.getElementById('timerDisplay');
            timerDisplay.textContent = Math.max(0, timeLeft);
            
            if (timeLeft <= 5) {
                timerDisplay.classList.add('warning');
            } else {
                timerDisplay.classList.remove('warning');
            }
        }

        function stopTimer() {
            if (timer) {
                clearInterval(timer);
                timer = null;
            }
        }

        function handleTimeout() {
            stopTimer();
            const elapsedTime = Math.floor((Date.now() - questionStartTime) / 1000);
            const extraTime = Math.max(0, elapsedTime - 15);
            
            // Deduct points for overtime
            const timeoutPenalty = 5 + extraTime; // Base penalty + extra time penalty
            teamScores[selectedTeam] = Math.max(0, teamScores[selectedTeam] - timeoutPenalty);
            
            closeModal();
            showResultModal('timeout', `⏰ TIME'S UP!\n-${timeoutPenalty}💎 for ${selectedTeam}!\n(Base: -5💎, Overtime: -${extraTime}💎)`);
            playTimeoutSound();
            
            updateTeamsDisplay();
            updateLeaderboard();
        }

        // Initialize game
        function initGame() {
            createGameBoard();
        }

        function addTeam() {
            const nameInput = document.getElementById('teamNameInput');
            const diamondsInput = document.getElementById('startingDiamondsInput');
            const teamName = nameInput.value.trim();
            const startingDiamonds = parseInt(diamondsInput.value) || 50;
            
            if (!teamName) {
                showMessage('Please enter a team name!', 'error');
                return;
            }
            
            if (teams.includes(teamName)) {
                showMessage('Team name already exists!', 'error');
                return;
            }
            
            if (teams.length >= 10) {
                showMessage('Maximum 10 teams allowed!', 'error');
                return;
            }
            
            if (startingDiamonds < 0 || startingDiamonds > 1000) {
                showMessage('Starting diamonds must be between 0-1000!', 'error');
                return;
            }
            
            teams.push(teamName);
            teamScores[teamName] = startingDiamonds;
            nameInput.value = '';
            diamondsInput.value = 50;
            
            updateTeamsDisplay();
            updateLeaderboard();
            showMessage(`Team "${teamName}" added with ${startingDiamonds}💎!`, 'success');
            playClickSound();
        }

        function updateTeamsDisplay() {
            const teamsGrid = document.getElementById('teamsGrid');
            teamsGrid.innerHTML = '';
            
            teams.forEach((team, index) => {
                const teamBtn = document.createElement('div');
                teamBtn.className = 'team-btn';
                if (selectedTeam === team) {
                    teamBtn.classList.add('selected');
                }
                teamBtn.onclick = () => selectTeam(team, teamBtn);
                teamBtn.innerHTML = `
                    <div class="team-name">${team}</div>
                    <div class="team-diamonds">💎 ${teamScores[team]}</div>
                `;
                teamsGrid.appendChild(teamBtn);
            });
        }

        function selectTeam(team, btn) {
            document.querySelectorAll('.team-btn').forEach(b => b.classList.remove('selected'));
            btn.classList.add('selected');
            selectedTeam = team;
            document.getElementById('currentTeam').textContent = `Current Team: ${team} (💎${teamScores[team]})`;
            showMessage(`Team ${team} selected! Click a treasure card to start!`, 'success');
            playClickSound();
        }

        function createGameBoard() {
            const gameBoard = document.getElementById('gameBoard');
            questions.forEach((question, index) => {
                const card = document.createElement('div');
                card.className = 'treasure-card';
                card.onclick = () => openQuestion(index);
                card.innerHTML = `
                    <div class="card-number">${index + 1}</div>
                    <div class="card-icon">${question.icon}</div>
                    <div class="card-topic">${question.topic}</div>
                `;
                gameBoard.appendChild(card);
            });
        }

        function openQuestion(cardIndex) {
            if (!selectedTeam) {
                showMessage('Please select a team first!', 'error');
                return;
            }

            if (answeredCards.includes(cardIndex)) {
                showMessage('This question has already been answered!', 'error');
                return;
            }

            currentQuestion = cardIndex;
            const question = questions[cardIndex];
            
            document.getElementById('questionText').textContent = question.question;
            
            const optionsContainer = document.getElementById('answerOptions');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'answer-option';
                button.textContent = option;
                button.onclick = () => selectAnswer(index);
                optionsContainer.appendChild(button);
            });
            document.getElementById('questionModal').style.display = 'block';
            startTimer();
            playClickSound();
        }

        function selectAnswer(answerIndex) {
            if (!currentQuestion !== null) return;
            
            stopTimer();
            const question = questions[currentQuestion];
            const isCorrect = answerIndex === question.correct;
            const elapsedTime = Math.floor((Date.now() - questionStartTime) / 1000);
            const extraTime = Math.max(0, elapsedTime - 15);
            
            let pointsChange = 0;
            let resultText = '';
            let resultMessage = '';
            
            if (isCorrect) {
                pointsChange = 10 - extraTime;
                teamScores[selectedTeam] += pointsChange;
                resultText = '✅ CORRECT!';
                resultMessage = `+${pointsChange}💎 for ${selectedTeam}!`;
                if (extraTime > 0) {
                    resultMessage += `\n(Base: +10💎, Overtime: -${extraTime}💎)`;
                }
                playCorrectSound();
            } else {
                pointsChange = -5 - extraTime;
                teamScores[selectedTeam] = Math.max(0, teamScores[selectedTeam] + pointsChange);
                resultText = '❌ WRONG!';
                resultMessage = `${pointsChange}💎 for ${selectedTeam}!`;
                if (extraTime > 0) {
                    resultMessage += `\n(Base: -5💎, Overtime: -${extraTime}💎)`;
                }
                playWrongSound();
            }
            
            answeredCards.push(currentQuestion);
            document.querySelectorAll('.treasure-card')[currentQuestion].classList.add('answered');
            
            closeModal();
            showResultModal(isCorrect ? 'correct' : 'wrong', `${resultText}\n${resultMessage}`);
            
            updateTeamsDisplay();
            updateLeaderboard();
            
            currentQuestion = null;
        }

        function closeModal() {
            stopTimer();
            document.getElementById('questionModal').style.display = 'none';
        }

        function showResultModal(type, message) {
            const modal = document.getElementById('resultModal');
            const content = document.getElementById('resultContent');
            const text = document.getElementById('resultText');
            const details = document.getElementById('resultDetails');
            
            content.className = `result-content result-${type}`;
            text.textContent = message.split('\n')[0];
            details.textContent = message.split('\n').slice(1).join('\n');
            
            modal.style.display = 'block';
            
            setTimeout(() => {
                modal.style.display = 'none';
            }, 3000);
        }

        function showMessage(message, type) {
            const messageArea = document.getElementById('messageArea');
            messageArea.innerHTML = `<div class="message ${type}">${message}</div>`;
            
            setTimeout(() => {
                messageArea.innerHTML = '<div class="message">Click a treasure card to answer questions!</div>';
            }, 3000);
        }

        function updateLeaderboard() {
            const leaderboardContent = document.getElementById('leaderboardContent');
            
            if (teams.length === 0) {
                leaderboardContent.innerHTML = 'Add teams to see the leaderboard!';
                return;
            }
            
            const sortedTeams = teams.sort((a, b) => teamScores[b] - teamScores[a]);
            
            let leaderboardHTML = '';
            sortedTeams.forEach((team, index) => {
                const medal = index === 0 ? '🥇' : index === 1 ? '🥈' : index === 2 ? '🥉' : '🏅';
                leaderboardHTML += `
                    <div style="display: flex; justify-content: space-between; align-items: center; padding: 8px; margin: 5px 0; background: rgba(255,255,255,0.1); border-radius: 8px;">
                        <span>${medal} ${team}</span>
                        <span style="color: #00f5ff; font-weight: bold;">💎 ${teamScores[team]}</span>
                    </div>
                `;
            });
            
            leaderboardContent.innerHTML = leaderboardHTML;
        }

        // Event listeners
        document.getElementById('teamNameInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addTeam();
            }
        });

        document.getElementById('startingDiamondsInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addTeam();
            }
        });

        // Close modal when clicking outside
        window.onclick = function(event) {
            const modal = document.getElementById('questionModal');
            if (event.target === modal) {
                closeModal();
            }
        }

        // Initialize the game
        initGame();
    </script>
</body>
</html>
