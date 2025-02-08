<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mines Gambling Game</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .grid { display: grid; grid-template-columns: repeat(5, 60px); gap: 5px; justify-content: center; margin-top: 20px; }
        .cell { width: 60px; height: 60px; background: #ccc; display: flex; align-items: center; justify-content: center; font-size: 24px; cursor: pointer; border: 2px solid #444; }
        .cell.revealed { background: lightgreen; cursor: default; }
        .cell.mine { background: red; }
        button { margin-top: 20px; padding: 10px; font-size: 16px; }
    </style>
</head>
<body>
    <h1>Mines Gambling Game</h1>
    <p>Click tiles to reveal safe spots. Hit a mine and lose!</p>
    <div class="grid" id="grid"></div>
    <p id="status">Balance: $100</p>
    <button onclick="restartGame()">Restart Game</button>
    <script>
        const gridSize = 5;
        let balance = 100;
        let mines = [];
        let revealedCells = 0;

        function generateMines() {
            mines = Array.from({ length: gridSize * gridSize }, () => Math.random() < 0.2);
        }

        function createGrid() {
            const grid = document.getElementById("grid");
            grid.innerHTML = "";
            for (let i = 0; i < gridSize * gridSize; i++) {
                const cell = document.createElement("div");
                cell.classList.add("cell");
                cell.dataset.index = i;
                cell.addEventListener("click", () => revealCell(i));
                grid.appendChild(cell);
            }
        }

        function revealCell(index) {
            const cell = document.querySelector(`.cell[data-index='${index}']`);
            if (!cell || cell.classList.contains("revealed")) return;

            if (mines[index]) {
                cell.classList.add("mine");
                cell.innerHTML = "💣";
                balance -= 10;
                document.getElementById("status").innerText = `You hit a mine! Balance: $${balance}`;
                setTimeout(restartGame, 1000);
            } else {
                cell.classList.add("revealed");
                cell.innerHTML = "✔";
                revealedCells++;
                balance += 5;
                document.getElementById("status").innerText = `Balance: $${balance}`;
            }
        }

        function restartGame() {
            revealedCells = 0;
            generateMines();
            createGrid();
            document.getElementById("status").innerText = `Balance: $${balance}`;
        }

        generateMines();
        createGrid();
    </script>
</body>
</html>
