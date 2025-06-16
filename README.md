<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Treasure Hunt Game</title>
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
        }

        .team-input {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #8338ec;
            border-radius: 10px;
            padding: 10px 15px;
            color: #fff;
            font-size: 1rem;
            outline: none;
            width: 200px;
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
            margin-left: 5px;
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

        .team-diamonds.negative {
            color: #ff0040;
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

        .treasure-card.correct {
            background: linear-gradient(45deg, #00ff0022, #00f5ff22);
            border-color: #00ff00;
            opacity: 0.5;
            cursor: not-allowed;
        }

        .treasure-card.wrong {
            background: linear-gradient(45deg, #ff004022, #ff006e22);
            border-color: #ff0040;
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

        .reset-btn {
            background: linear-gradient(45deg, #ff006e, #8338ec);
            border: none;
            border-radius: 10px;
            padding: 10px 20px;
            color: #fff;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-left: 10px;
        }

        .reset-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px rgba(255, 0, 110, 0.5);
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

        .timer-container {
            position: absolute;
            top: 20px;
            right: 30px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #ff006e;
            border-radius: 50%;
            width: 80px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            font-weight: bold;
            color: #00f5ff;
            text-shadow: 0 0 10px #00f5ff;
        }

        .timer-danger {
            border-color: #ff0040 !important;
            color: #ff0040 !important;
            text-shadow: 0 0 15px #ff0040 !important;
            animation: timerBlink 0.5s infinite alternate;
        }

        .timer-overtime {
            border-color: #ff6600 !important;
            color: #ff6600 !important;
            text-shadow: 0 0 15px #ff6600 !important;
            animation: timerBlink 0.3s infinite alternate;
        }

        @keyframes timerBlink {
            from { opacity: 1; }
            to { opacity: 0.5; }
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
            .timer-container {
                width: 60px;
                height: 60px;
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="neon-bg"></div>
    
    <div class="container">
        <div class="header">
            <h1 class="title">🏴‍☠️ TREASURE HUNT GAME 🏴‍☠️</h1>
            <p class="subtitle">Adventure Awaits Those Who Dare!</p>
        </div>

        <div class="team-selection">
            <h3 style="text-align: center; margin-bottom: 15px; color: #00f5ff;">⚔️ Create Your Teams ⚔️</h3>
            <div class="team-input-section">
                <input type="text" id="teamNameInput" class="team-input" placeholder="Enter team name..." maxlength="20">
                <input type="number" id="startingDiamondsInput" class="team-input diamonds-input" placeholder="Diamonds" min="-999" max="999" value="0">
                <button class="add-team-btn" onclick="addTeam()">Add Team</button>
                <button class="reset-btn" onclick="resetQuestions()">Reset Questions</button>
            </div>
            <div class="teams-grid" id="teamsGrid"></div>
        </div>

        <div class="game-info">
            <div>Current Turn: <span class="current-team" id="currentTeam">Select a team first</span></div>
            <div>Round: <span id="roundCounter">1</span></div>
        </div>

        <div class="message-area">
            <div class="message" id="gameMessage">Welcome! Create teams and start your adventure!</div>
        </div>

        <div class="game-board" id="gameBoard"></div>

        <div class="leaderboard">
            <h3>🏆 LEADERBOARD 🏆</h3>
            <div id="leaderboardList"></div>
        </div>
    </div>

    <!-- Question Modal -->
    <div id="questionModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <!-- Timer Container -->
            <div class="timer-container" id="timerDisplay">15</div>
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
        // Game state
        let teams = [];
        let currentTeamIndex = 0;
        let cardStates = {}; // เก็บสถานะของแต่ละการ์ด: 'correct', 'wrong', หรือ undefined
        let gameRound = 1;
        let currentQuestionIndex = -1;
        
        // Timer variables
        let questionTimer = null;
        let timeLeft = 15;
        let isOvertime = false;
        let overtimeSeconds = 0;

        // Sound creation functions
        function createTickSound() {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            oscillator.frequency.value = 800;
            oscillator.type = 'square';
            
            gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.1);
            
            oscillator.start(audioContext.currentTime);
            oscillator.stop(audioContext.currentTime + 0.1);
        }

        function createOvertimeSound() {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            oscillator.frequency.value = 600;
            oscillator.type = 'sine';
            
            gainNode.gain.setValueAtTime(0.2, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.2);
            
            oscillator.start(audioContext.currentTime);
            oscillator.stop(audioContext.currentTime + 0.2);
        }

        // Questions data
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

        // Add team function
        function addTeam() {
            const nameInput = document.getElementById('teamNameInput');
            const diamondsInput = document.getElementById('startingDiamondsInput');
            const teamName = nameInput.value.trim();
            const startingDiamonds = parseInt(diamondsInput.value) || 0;
            
            if (teamName === '') {
                showMessage('Please enter a team name!', 'error');
                return;
            }

            if (teams.some(team => team.name.toLowerCase() === teamName.toLowerCase())) {
                showMessage('Team name already exists!', 'error');
                return;
            }

            if (teams.length >= 10) {
                showMessage('Maximum 10 teams allowed!', 'error');
                return;
            }

            if (startingDiamonds < -999 || startingDiamonds > 999) {
                showMessage('Starting diamonds must be between -999 to 999!', 'error');
                return;
            }

            teams.push({
                name: teamName,
                diamonds: startingDiamonds
            });

            nameInput.value = '';
            diamondsInput.value = '0';
            updateTeamsDisplay();
            updateLeaderboard();
            
            if (teams.length === 1) {
                currentTeamIndex = 0;
                updateCurrentTeamDisplay();
                createGameBoard();
            }
            
            showMessage(`Team "${teamName}" added with ${startingDiamonds} diamonds!`, 'success');
        }

        function updateTeamsDisplay() {
            const teamsGrid = document.getElementById('teamsGrid');
            teamsGrid.innerHTML = '';
            
            teams.forEach((team, index) => {
                const teamBtn = document.createElement('div');
                teamBtn.className = `team-btn ${index === currentTeamIndex ? 'selected' : ''}`;
                teamBtn.onclick = () => selectTeam(index);
                teamBtn.innerHTML = `
                    <div class="team-name">${team.name}</div>
                    <div class="team-diamonds ${team.diamonds < 0 ? 'negative' : ''}">💎 ${team.diamonds}</div>
                `;
                teamsGrid.appendChild(teamBtn);
            });
        }

        function selectTeam(index) {
            if (teams.length === 0) return;
            currentTeamIndex = index;
            updateTeamsDisplay();
            updateCurrentTeamDisplay();
        }

        function updateCurrentTeamDisplay() {
            const currentTeamElement = document.getElementById('currentTeam');
            if (teams.length > 0) {
                currentTeamElement.textContent = teams[currentTeamIndex].name;
            } else {
                currentTeamElement.textContent = 'No teams created';
            }
        }

        function createGameBoard() {
            const gameBoard = document.getElementById('gameBoard');
            gameBoard.innerHTML = '';
            
            questions.forEach((q, index) => {
                const card = document.createElement('div');
                let cardClass = 'treasure-card';
                
                if (cardStates[index] === 'correct') {
                    cardClass += ' correct';
                } else if (cardStates[index] === 'wrong') {
                    cardClass += ' wrong';
                }
                
                card.className = cardClass;
                card.onclick = () => openQuestion(index);
                card.innerHTML = `
                    <div class="card-number">${index + 1}</div>
                    <div class="card-topic">${q.topic}</div>
                    <div class="card-icon">${q.icon}</div>
                `;
                gameBoard.appendChild(card);
            });
        }

        // Timer functions
        function startTimer() {
            timeLeft = 15;
            isOvertime = false;
            overtimeSeconds = 0;
            updateTimerDisplay();
            
            questionTimer = setInterval(() => {
                if (timeLeft > 0) {
                    timeLeft--;
                    if (timeLeft <= 5 && timeLeft > 0) {
                        createTickSound();
                    }
                } else {
                    isOvertime = true;
                    overtimeSeconds++;
                    createOvertimeSound();
                }
                updateTimerDisplay();
            }, 1000);
        }

        function stopTimer() {
            if (questionTimer) {
                clearInterval(questionTimer);
                questionTimer = null;
            }
        }

        function updateTimerDisplay() {
            const timerDisplay = document.getElementById('timerDisplay');
            
            if (!isOvertime) {
                timerDisplay.textContent = timeLeft;
                if (timeLeft <= 5) {
                    timerDisplay.className = 'timer-container timer-danger';
                } else {
                    timerDisplay.className = 'timer-container';
                }
            } else {
                timerDisplay.textContent = `+${overtimeSeconds}`;
                timerDisplay.className = 'timer-container timer-overtime';
            }
        }

        function openQuestion(questionIndex) {
            if (teams.length === 0) {
                showMessage('Please create at least one team first!', 'error');
                return;
        }

            if (cardStates[questionIndex] === 'correct') {
                showMessage('This question has already been answered correctly!', 'error');
                return;
            }

            currentQuestionIndex = questionIndex;
            const question = questions[questionIndex];
            
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
        }

        function selectAnswer(answerIndex) {
            stopTimer();
            const question = questions[currentQuestionIndex];
            const isCorrect = answerIndex === question.correct;
            
            // Calculate penalty/reward
            let diamondChange = 0;
            let penaltyMessage = '';
            
            if (isCorrect) {
                diamondChange = 10;
                cardStates[currentQuestionIndex] = 'correct';
            } else {
                // Base penalty
                diamondChange = -5;
                
                // Time penalty
                if (isOvertime) {
                    const timePenalty = Math.min(overtimeSeconds, 10); // Max 10 extra penalty
                    diamondChange -= timePenalty;
                    penaltyMessage = ` (includes ${timePenalty} overtime penalty)`;
                }
                
                cardStates[currentQuestionIndex] = 'wrong';
            }
            
            // Update team diamonds
            teams[currentTeamIndex].diamonds += diamondChange;
            
            closeModal();
            showResult(isCorrect, diamondChange, penaltyMessage);
            
            setTimeout(() => {
                updateTeamsDisplay();
                updateLeaderboard();
                createGameBoard();
                nextTurn();
                closeResultModal();
            }, 2500);
        }

        function showResult(isCorrect, diamondChange, penaltyMessage = '') {
            const resultModal = document.getElementById('resultModal');
            const resultContent = document.getElementById('resultContent');
            const resultText = document.getElementById('resultText');
            const resultDetails = document.getElementById('resultDetails');
            
            if (isCorrect) {
                resultContent.className = 'result-content result-correct';
                resultText.textContent = '🎉 CORRECT! 🎉';
                resultDetails.innerHTML = `
                    <div>Team: ${teams[currentTeamIndex].name}</div>
                    <div>Earned: +${diamondChange} diamonds</div>
                    <div>Total: ${teams[currentTeamIndex].diamonds + diamondChange} diamonds</div>
                `;
            } else {
                resultContent.className = 'result-content result-wrong';
                resultText.textContent = '❌ WRONG! ❌';
                resultDetails.innerHTML = `
                    <div>Team: ${teams[currentTeamIndex].name}</div>
                    <div>Lost: ${diamondChange} diamonds${penaltyMessage}</div>
                    <div>Total: ${teams[currentTeamIndex].diamonds + diamondChange} diamonds</div>
                `;
            }
            
            resultModal.style.display = 'block';
        }

        function closeResultModal() {
            document.getElementById('resultModal').style.display = 'none';
        }

        function closeModal() {
            stopTimer();
            document.getElementById('questionModal').style.display = 'none';
        }

        function nextTurn() {
            if (teams.length === 0) return;
            
            currentTeamIndex = (currentTeamIndex + 1) % teams.length;
            
            // Check if all teams have played this round
            if (currentTeamIndex === 0) {
                gameRound++;
                document.getElementById('roundCounter').textContent = gameRound;
            }
            
            updateCurrentTeamDisplay();
            updateTeamsDisplay();
        }

        function showMessage(message, type) {
            const messageElement = document.getElementById('gameMessage');
            messageElement.textContent = message;
            messageElement.className = `message ${type}`;
            
            setTimeout(() => {
                messageElement.textContent = 'Select a treasure card to answer a question!';
                messageElement.className = 'message';
            }, 3000);
        }

        function updateLeaderboard() {
            const leaderboardList = document.getElementById('leaderboardList');
            
            if (teams.length === 0) {
                leaderboardList.innerHTML = '<div style="text-align: center; color: #aaa;">No teams yet</div>';
                return;
            }
            
            // Sort teams by diamonds (descending)
            const sortedTeams = [...teams].sort((a, b) => b.diamonds - a.diamonds);
            
            leaderboardList.innerHTML = '';
            sortedTeams.forEach((team, index) => {
                const position = index + 1;
                let medal = '';
                let textColor = '#fff';
                
                switch(position) {
                    case 1:
                        medal = '🥇';
                        textColor = '#FFD700';
                        break;
                    case 2:
                        medal = '🥈';
                        textColor = '#C0C0C0';
                        break;
                    case 3:
                        medal = '🥉';
                        textColor = '#CD7F32';
                        break;
                    default:
                        medal = `${position}.`;
                        break;
                }
                
                const teamDiv = document.createElement('div');
                teamDiv.style.cssText = `
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    padding: 8px 12px;
                    margin: 5px 0;
                    background: rgba(255, 255, 255, 0.05);
                    border-radius: 8px;
                    border-left: 3px solid ${textColor};
                    color: ${textColor};
                `;
                
                teamDiv.innerHTML = `
                    <span>${medal} ${team.name}</span>
                    <span style="color: ${team.diamonds < 0 ? '#ff0040' : '#00f5ff'};">💎 ${team.diamonds}</span>
                `;
                
                leaderboardList.appendChild(teamDiv);
            });
        }

        function resetQuestions() {
            if (confirm('Are you sure you want to reset all questions? This will clear all progress but keep teams and scores.')) {
                cardStates = {};
                createGameBoard();
                showMessage('All questions have been reset!', 'success');
            }
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
            const resultModal = document.getElementById('resultModal');
            
            if (event.target === modal) {
                closeModal();
            }
            if (event.target === resultModal) {
                closeResultModal();
            }
        }

        // Close modal with Escape key
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') {
                closeModal();
                closeResultModal();
            }
        });

        // Initialize game
        document.addEventListener('DOMContentLoaded', function() {
            updateLeaderboard();
            showMessage('Welcome to Treasure Hunt! Create your teams and start playing!', 'success');
        });
    </script>
</body>
</html>
