
<html>
<head>
    <title>惠如寶貝數獨世界</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            max-width: 500px;
            width: 100%;
        }
        
        h1 {
            text-align: center;
            color: #ff6b6b;
            margin-bottom: 20px;
            font-size: 24px;
        }
        
        .difficulty-select {
            text-align: center;
        }
        
        .difficulty-btn {
            display: block;
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        .difficulty-btn:active {
            transform: scale(0.95);
        }
        
        .on9 {
            background: linear-gradient(145deg, #ffeb3b, #ffd600);
            color: black;
        }
        
        .hard {
            background: linear-gradient(145deg, #ff9800, #f57c00);
            color: white;
        }
        
        .extreme {
            background: linear-gradient(145deg, #f44336, #d32f2f);
            color: white;
        }
        
        .game-container {
            display: none;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        
        .info-item {
            background: #f0f0f0;
            padding: 8px 12px;
            border-radius: 8px;
            margin: 5px;
            font-weight: bold;
        }
        
        .sudoku-board {
            display: grid;
            grid-template-columns: repeat(9, 1fr);
            gap: 1px;
            background: #000;
            border: 2px solid #000;
            margin-bottom: 15px;
        }
        
        .cell {
            width: 100%;
            aspect-ratio: 1;
            background: white;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            user-select: none;
        }
        
        .cell.prefilled {
            background: #e0e0e0;
        }
        
        .cell.selected {
            background: #bbdefb;
        }
        
        .cell.error {
            background: #ffcdd2;
            color: #d32f2f;
        }
        
        .cell.correct {
            background: #c8e6c9;
            color: #388e3c;
        }
        
        .border-right {
            border-right: 2px solid #000 !important;
        }
        
        .border-bottom {
            border-bottom: 2px solid #000 !important;
        }
        
        .number-pad {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .number-btn {
            padding: 12px;
            border: none;
            border-radius: 8px;
            background: #2196f3;
            color: white;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
        
        .number-btn:active {
            background: #1976d2;
        }
        
        .clear-btn {
            background: #ffeb3b;
            color: black;
        }
        
        .hint-btn {
            background: #ff9800;
        }
        
        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
        
        .control-btn {
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 14px;
            fontWeight: bold;
            cursor: pointer;
        }
        
        .back-btn {
            background: #666;
            color: white;
        }
        
        .new-game-btn {
            background: #4caf50;
            color: white;
        }
        
        .game-over, .game-completed {
            text-align: center;
            padding: 20px;
            display: none;
        }
        
        .game-over h2, .game-completed h2 {
            color: #f44336;
            margin-bottom: 15px;
        }
        
        .restart-btn {
            padding: 12px 24px;
            background: #2196f3;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            margin-top: 15px;
        }
        
        @media (max-width: 400px) {
            .cell {
                font-size: 14px;
            }
            
            .number-btn {
                padding: 10px;
                font-size: 14px;
            }
            
            .control-btn {
                padding: 10px;
                font-size: 12px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>惠如寶貝數獨世界</h1>
        
        <div class="difficulty-select" id="difficultySelect">
            <h2>選擇難度</h2>
            <button class="difficulty-btn on9" onclick="startGame('on9')">on9</button>
            <button class="difficulty-btn hard" onclick="startGame('hard')">難</button>
            <button class="difficulty-btn extreme" onclick="startGame('extreme')">好撚難</button>
        </div>
        
        <div class="game-container" id="gameContainer">
            <div class="game-info">
                <div class="info-item" id="timer">時間: 00:00</div>
                <div class="info-item" id="errorCount">錯誤: 0/3</div>
                <div class="info-item" id="hintCount">提示: 3</div>
            </div>
            
            <div class="sudoku-board" id="sudokuBoard"></div>
            
            <div class="number-pad">
                <button class="number-btn" onclick="fillNumber(1)">1</button>
                <button class="number-btn" onclick="fillNumber(2)">2</button>
                <button class="number-btn" onclick="fillNumber(3)">3</button>
                <button class="number-btn" onclick="fillNumber(4)">4</button>
                <button class="number-btn" onclick="fillNumber(5)">5</button>
                <button class="number-btn" onclick="fillNumber(6)">6</button>
                <button class="number-btn" onclick="fillNumber(7)">7</button>
                <button class="number-btn" onclick="fillNumber(8)">8</button>
                <button class="number-btn" onclick="fillNumber(9)">9</button>
                <button class="number-btn clear-btn" onclick="fillNumber(0)">清除</button>
                <button class="number-btn hint-btn" onclick="useHint()">提示</button>
            </div>
            
            <div class="controls">
                <button class="control-btn back-btn" onclick="backToMenu()">← 返回</button>
                <button class="control-btn new-game-btn" onclick="newGame()">🔄 新遊戲</button>
            </div>
        </div>
        
        <div class="game-over" id="gameOver">
            <h2>遊戲結束！錯誤次數已達3次</h2>
            <button class="restart-btn" onclick="backToMenu()">返回主選單</button>
        </div>
        
        <div class="game-completed" id="gameCompleted">
            <h2>恭喜完成！</h2>
            <p id="completionTime">完成時間: 00:00</p>
            <button class="restart-btn" onclick="backToMenu()">返回主選單</button>
        </div>
    </div>

    <script>
        let gameState = {
            board: [],
            solution: [],
            userBoard: [],
            prefilled: [],
            selectedCell: null,
            errorCount: 0,
            hintsUsed: 0,
            maxHints: 3,
            startTime: null,
            timerInterval: null,
            difficulty: 'on9',
            gameOver: false,
            gameCompleted: false
        };

        const difficulties = {
            'on9': { holes: 20, hints: 3 },
            'hard': { holes: 45, hints: 3 },
            'extreme': { holes: 81 - (17 + 3), hints: 5 }
        };

        function startGame(difficulty) {
            gameState.difficulty = difficulty;
            gameState.maxHints = difficulties[difficulty].hints;
            resetGame();
            
            document.getElementById('difficultySelect').style.display = 'none';
            document.getElementById('gameContainer').style.display = 'block';
            document.getElementById('gameOver').style.display = 'none';
            document.getElementById('gameCompleted').style.display = 'none';
        }

        function generateSolution() {
            const grid = Array(9).fill().map(() => Array(9).fill(0));
            
            function isValid(row, col, num) {
                for (let i = 0; i < 9; i++) {
                    if (grid[row][i] === num) return false;
                    if (grid[i][col] === num) return false;
                }
                
                const boxRow = Math.floor(row / 3) * 3;
                const boxCol = Math.floor(col / 3) * 3;
                for (let i = 0; i < 3; i++) {
                    for (let j = 0; j < 3; j++) {
                        if (grid[boxRow + i][boxCol + j] === num) return false;
                    }
                }
                
                return true;
            }
            
            function solve(row = 0, col = 0) {
                if (row === 9) return true;
                if (col === 9) return solve(row + 1, 0);
                if (grid[row][col] !== 0) return solve(row, col + 1);
                
                const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
                for (let i = numbers.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [numbers[i], numbers[j]] = [numbers[j], numbers[i]];
                }
                
                for (const num of numbers) {
                    if (isValid(row, col, num)) {
                        grid[row][col] = num;
                        if (solve(row, col + 1)) return true;
                        grid[row][col] = 0;
                    }
                }
                
                return false;
            }
            
            solve();
            return grid;
        }

        function createPuzzle(solution, difficulty) {
            const puzzle = solution.map(row => [...row]);
            const holes = difficulties[difficulty].holes;
            
            let holesCreated = 0;
            while (holesCreated < holes) {
                const row = Math.floor(Math.random() * 9);
                const col = Math.floor(Math.random() * 9);
                if (puzzle[row][col] !== 0) {
                    puzzle[row][col] = 0;
                    holesCreated++;
                }
            }
            
            return puzzle;
        }

        function resetGame() {
            clearInterval(gameState.timerInterval);
            
            const solution = generateSolution();
            const puzzle = createPuzzle(solution, gameState.difficulty);
            
            gameState = {
                board: puzzle,
                solution: solution,
                userBoard: puzzle.map(row => [...row]),
                prefilled: puzzle.map(row => row.map(val => val !== 0)),
                selectedCell: null,
                errorCount: 0,
                hintsUsed: 0,
                maxHints: difficulties[gameState.difficulty].hints,
                startTime: new Date(),
                timerInterval: null,
                difficulty: gameState.difficulty,
                gameOver: false,
                gameCompleted: false
            };
            
            updateUI();
            startTimer();
        }

        function startTimer() {
            gameState.timerInterval = setInterval(updateTimer, 1000);
        }

        function updateTimer() {
            const elapsed = Math.floor((new Date() - gameState.startTime) / 1000);
            const minutes = Math.floor(elapsed / 60);
            const seconds = elapsed % 60;
            document.getElementById('timer').textContent = `時間: ${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function selectCell(row, col) {
            if (gameState.gameOver || gameState.gameCompleted) return;
            if (!gameState.prefilled[row][col]) {
                gameState.selectedCell = { row, col };
                updateUI();
            }
        }

        function fillNumber(num) {
            if (!gameState.selectedCell || gameState.gameOver || gameState.gameCompleted) return;
            
            const { row, col } = gameState.selectedCell;
            if (gameState.prefilled[row][col]) return;
            
            gameState.userBoard[row][col] = num;
            
            if (num === gameState.solution[row][col]) {
                checkCompletion();
            } else if (num !== 0) {
                gameState.errorCount++;
                if (gameState.errorCount >= 3) {
                    gameState.gameOver = true;
                    clearInterval(gameState.timerInterval);
                    document.getElementById('gameOver').style.display = 'block';
                    document.getElementById('gameContainer').style.display = 'none';
                }
            }
            
            updateUI();
        }

        function useHint() {
            if (!gameState.selectedCell || gameState.hintsUsed >= gameState.maxHints || gameState.gameOver || gameState.gameCompleted) return;
            
            const { row, col } = gameState.selectedCell;
            if (gameState.prefilled[row][col]) return;
            
            gameState.userBoard[row][col] = gameState.solution[row][col];
            gameState.hintsUsed++;
            checkCompletion();
            updateUI();
        }

        function checkCompletion() {
            for (let i = 0; i < 9; i++) {
                for (let j = 0; j < 9; j++) {
                    if (gameState.userBoard[i][j] !== gameState.solution[i][j]) {
                        return;
                    }
                }
            }
            
            gameState.gameCompleted = true;
            clearInterval(gameState.timerInterval);
            
            const elapsed = Math.floor((new Date() - gameState.startTime) / 1000);
            const minutes = Math.floor(elapsed / 60);
            const seconds = elapsed % 60;
            
            document.getElementById('completionTime').textContent = `完成時間: ${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            document.getElementById('gameCompleted').style.display = 'block';
            document.getElementById('gameContainer').style.display = 'none';
        }

        function updateUI() {
            const boardElement = document.getElementById('sudokuBoard');
            boardElement.innerHTML = '';
            
            for (let i = 0; i < 9; i++) {
                for (let j = 0; j < 9; j++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    
                    if (gameState.prefilled[i][j]) {
                        cell.classList.add('prefilled');
                    }
                    
                    if (gameState.selectedCell && gameState.selectedCell.row === i && gameState.selectedCell.col === j) {
                        cell.classList.add('selected');
                    }
                    
                    if (gameState.userBoard[i][j] !== 0 && !gameState.prefilled[i][j]) {
                        if (gameState.userBoard[i][j] !== gameState.solution[i][j]) {
                            cell.classList.add('error');
                        } else {
                            cell.classList.add('correct');
                        }
                    }
                    
                    if ((j + 1) % 3 === 0 && j < 8) {
                        cell.classList.add('border-right');
                    }
                    
                    if ((i + 1) % 3 === 0 && i < 8) {
                        cell.classList.add('border-bottom');
                    }
                    
                    if (gameState.userBoard[i][j] !== 0) {
                        cell.textContent = gameState.userBoard[i][j];
                    }
                    
                    cell.addEventListener('click', () => selectCell(i, j));
                    boardElement.appendChild(cell);
                }
            }
            
            document.getElementById('errorCount').textContent = `錯誤: ${gameState.errorCount}/3`;
            document.getElementById('hintCount').textContent = `提示: ${gameState.maxHints - gameState.hintsUsed}`;
        }

        function backToMenu() {
            clearInterval(gameState.timerInterval);
            document.getElementById('difficultySelect').style.display = 'block';
            document.getElementById('gameContainer').style.display = 'none';
            document.getElementById('gameOver').style.display = 'none';
            document.getElementById('gameCompleted').style.display = 'none';
        }

        function newGame() {
            resetGame();
        }

        // 初始化頁面
        document.getElementById('difficultySelect').style.display = 'block';
        document.getElementById('gameContainer').style.display = 'none';
    </script>
</body>
</html>
