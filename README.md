<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic-Tac-Toe Game</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      background: linear-gradient(135deg, #667eea, #764ba2);
      color: white;
    }
    h1 {
      margin-bottom: 10px;
    }
    #mode-select {
      margin-bottom: 20px;
    }
    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
    }
    .cell {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      background-color: rgba(255, 255, 255, 0.1);
      border-radius: 10px;
      cursor: pointer;
      user-select: none;
      transition: background-color 0.3s ease;
    }
    .cell:hover {
      background-color: rgba(255, 255, 255, 0.3);
    }
    #status {
      margin-top: 20px;
      font-size: 1.2rem;
    }
    #reset {
      margin-top: 10px;
      padding: 10px 20px;
      font-size: 1rem;
      background: white;
      color: #333;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Tic-Tac-Toe</h1>
  <div id="mode-select">
    <label for="mode">Select Mode:</label>
    <select id="mode">
      <option value="2p">2 Player</option>
      <option value="ai">Play vs Computer</option>
    </select>
  </div>
  <div id="board"></div>
  <div id="status"></div>
  <button id="reset">Reset Game</button>

  <script>
    const boardElement = document.getElementById("board");
    const statusElement = document.getElementById("status");
    const resetButton = document.getElementById("reset");
    const modeSelect = document.getElementById("mode");

    let board = ["", "", "", "", "", "", "", "", ""];
    let currentPlayer = "X";
    let gameActive = true;
    let mode = "2p";

    function renderBoard() {
      boardElement.innerHTML = "";
      board.forEach((cell, index) => {
        const cellDiv = document.createElement("div");
        cellDiv.className = "cell";
        cellDiv.textContent = cell;
        cellDiv.addEventListener("click", () => handleCellClick(index));
        boardElement.appendChild(cellDiv);
      });
    }

    function handleCellClick(index) {
      if (!gameActive || board[index] !== "") return;

      board[index] = currentPlayer;
      renderBoard();

      if (checkWinner(currentPlayer)) {
        statusElement.textContent = `Player ${currentPlayer} wins!`;
        gameActive = false;
        return;
      } else if (board.every(cell => cell !== "")) {
        statusElement.textContent = "It's a draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === "X" ? "O" : "X";
      statusElement.textContent = `Player ${currentPlayer}'s turn`;

      if (mode === "ai" && currentPlayer === "O") {
        setTimeout(makeComputerMove, 500);
      }
    }

    function makeComputerMove() {
      const emptyIndices = board.map((val, idx) => val === "" ? idx : null).filter(val => val !== null);
      const randomIndex = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];
      handleCellClick(randomIndex);
    }

    function checkWinner(player) {
      const winPatterns = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ];
      return winPatterns.some(pattern => 
        pattern.every(index => board[index] === player)
      );
    }

    function resetGame() {
      board = ["", "", "", "", "", "", "", "", ""];
      currentPlayer = "X";
      gameActive = true;
      statusElement.textContent = `Player ${currentPlayer}'s turn`;
      renderBoard();
    }

    modeSelect.addEventListener("change", () => {
      mode = modeSelect.value;
      resetGame();
    });

    resetButton.addEventListener("click", resetGame);

    resetGame();
  </script>
</body>
</html>
