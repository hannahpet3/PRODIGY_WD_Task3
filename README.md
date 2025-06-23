<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Tic Tac Toe</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #3a1c71, #d76d77, #ffaf7b);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }

    h1 {
      margin-bottom: 20px;
    }

    .mode-select {
      margin-bottom: 20px;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-gap: 5px;
    }

    .cell {
      width: 100px;
      height: 100px;
      font-size: 2em;
      background-color: #fff;
      color: #333;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    }

    .cell:hover {
      background-color: #eee;
    }

    .message {
      margin-top: 20px;
      font-size: 1.2em;
    }

    .reset {
      margin-top: 10px;
      padding: 10px 20px;
      border: none;
      background: #fff;
      color: #333;
      font-size: 1em;
      cursor: pointer;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <h1>Tic Tac Toe</h1>
  <div class="mode-select">
    <label><input type="radio" name="mode" value="two" checked> Two Player</label>
    <label><input type="radio" name="mode" value="computer"> Play vs Computer</label>
  </div>
  <div class="board" id="board"></div>
  <div class="message" id="message"></div>
  <button class="reset" onclick="init()">Restart Game</button>

  <script>
    const boardElement = document.getElementById('board');
    const messageElement = document.getElementById('message');
    let cells = [];
    let board = Array(9).fill(null);
    let currentPlayer = 'X';
    let gameActive = true;
    let vsComputer = false;

    function init() {
      board = Array(9).fill(null);
      currentPlayer = 'X';
      gameActive = true;
      vsComputer = document.querySelector('input[name="mode"]:checked').value === 'computer';
      boardElement.innerHTML = '';
      messageElement.textContent = "Player X's turn";
      cells = [];

      for (let i = 0; i < 9; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.dataset.index = i;
        cell.addEventListener('click', handleCellClick);
        boardElement.appendChild(cell);
        cells.push(cell);
      }
    }

    function handleCellClick(e) {
      const index = e.target.dataset.index;
      if (!gameActive || board[index]) return;

      board[index] = currentPlayer;
      e.target.textContent = currentPlayer;

      if (checkWinner(currentPlayer)) {
        messageElement.textContent = `Player ${currentPlayer} wins!`;
        gameActive = false;
        return;
      }

      if (board.every(cell => cell)) {
        messageElement.textContent = "It's a draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      messageElement.textContent = `Player ${currentPlayer}'s turn`;

      if (vsComputer && currentPlayer === 'O') {
        setTimeout(computerMove, 500);
      }
    }

    function computerMove() {
      if (!gameActive) return;
      let index = getBestMove();
      board[index] = 'O';
      cells[index].textContent = 'O';

      if (checkWinner('O')) {
        messageElement.textContent = `Computer wins!`;
        gameActive = false;
        return;
      }

      if (board.every(cell => cell)) {
        messageElement.textContent = "It's a draw!";
        gameActive = false;
        return;
      }

      currentPlayer = 'X';
      messageElement.textContent = `Player ${currentPlayer}'s turn`;
    }

    function getBestMove() {
      let emptyIndexes = board.map((val, idx) => val ? null : idx).filter(i => i !== null);
      return emptyIndexes[Math.floor(Math.random() * emptyIndexes.length)];
    }

    function checkWinner(player) {
      const winPatterns = [
        [0,1,2],[3,4,5],[6,7,8],
        [0,3,6],[1,4,7],[2,5,8],
        [0,4,8],[2,4,6]
      ];

      return winPatterns.some(pattern =>
        pattern.every(index => board[index] === player)
      );
    }

    init();
  </script>
</body>
</html>
