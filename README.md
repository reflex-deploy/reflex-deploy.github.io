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
        .hidden { display: none; }
        .end-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.8); color: white; display: flex; flex-direction: column; align-items: center; justify-content: center; }
    </style>
</head>
<body>
    <h1>Mines Gambling Game</h1>
    <p>Click tiles to reveal safe spots. Hit a mine and lose!</p>
    <div class="grid" id="grid"></div>
    <p id="status">Balance: $100</p>
    <button onclick="restartGame()">Restart Game</button>
    <div id="endScreen" class="end-screen hidden">
        <h2 id="endMessage"></h2>
        <button onclick="restartGame()">Play Again</button>
    </div>
    <script>
        const gridSize = 5;
        let balance = 100;
        let mineIndex;
        let revealedCells = 0;

        function generateMine() {
            mineIndex = Math.floor(Math.random() * (gridSize * gridSize));
        }

        function createGrid() {
            const grid = document.getElementById("grid");
            grid.innerHTML = "";  // Clear the grid before creating it again
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

            if (index === mineIndex) {
                cell.classList.add("mine");
                cell.innerHTML = "💣";
                balance = Math.floor(balance / 2);
                document.getElementById("status").innerText = `You hit the mine! Balance: $${balance}`;
                checkEndGame();
            } else {
                cell.classList.add("revealed");
                cell.innerHTML = "💎";
                revealedCells++;
                balance += 5;
                document.getElementById("status").innerText = `Balance: $${balance}`;
                checkVictory();
            }
        }

        function checkVictory() {
            if (revealedCells === gridSize * gridSize - 1) {
                document.getElementById("endMessage").innerText = "Lucky Pick!";
                document.getElementById("endScreen").classList.remove("hidden");
            }
        }

        function checkEndGame() {
            if (balance <= 0) {
                document.getElementById("endMessage").innerText = "Game Over! You're out of money!";
                document.getElementById("endScreen").classList.remove("hidden");
            } else if (balance >= 1000) {
                document.getElementById("endMessage").innerText = "99% of gamblers quit before winning!";
                document.getElementById("endScreen").classList.remove("hidden");
            }
        }

        function restartGame() {
            revealedCells = 0;
            balance = 100;
            generateMine();  // Ensure the mine is generated before the grid is created.
            createGrid();    // Now, create the grid and bind the click events.
            document.getElementById("status").innerText = `Balance: $${balance}`;
            document.getElementById("endScreen").classList.add("hidden"); // Hide end screen after restart
        }

        generateMine();   // Ensure the mine index is set before grid creation.
        createGrid();     // Now, create the grid and bind the click events.
    </script>
</body>
</html>
