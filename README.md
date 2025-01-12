<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Luckey Eric's Casino</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #222;
      color: #fff;
      text-align: center;
    }

    h1 {
      margin: 20px 0;
    }

    .nft-container {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin: 20px;
    }

    .nft {
      border: 2px solid #fff;
      border-radius: 10px;
      padding: 20px;
      width: 200px;
      background: #333;
    }

    .nft.unavailable {
      opacity: 0.5;
    }

    canvas {
      margin: 10px auto;
    }

    .controls {
      margin-top: 20px;
    }

    button {
      padding: 10px 20px;
      margin: 5px;
      font-size: 16px;
      background: #f39c12;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .roulette-button {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #e74c3c;
    }
  </style>
</head>
<body>
  <h1>Luckey Eric's Casino</h1>
  <div class="nft-container">
    <div class="nft" id="eric">
      <h2>Eric</h2>
      <p>Value: <span id="eric-value">$100</span></p>
      <canvas id="eric-graph" width="180" height="100"></canvas>
    </div>
    <div class="nft unavailable">
      <h2>Unavailable</h2>
    </div>
    <div class="nft unavailable">
      <h2>Unavailable</h2>
    </div>
  </div>

  <div class="controls">
    <button onclick="deposit()">Deposit</button>
    <button onclick="withdraw()">Withdraw</button>
  </div>

  <button class="roulette-button" onclick="goToRoulette()">Roulette Table</button>

  <script>
    let ericValue = 100;
    const ericValueElement = document.getElementById("eric-value");
    const ericGraph = document.getElementById("eric-graph");
    const ctx = ericGraph.getContext("2d");

    let funds = 1000; // Starting funds
    const graphData = [];

    function updateEricValue() {
      const change = (Math.random() - 0.5) * 10; // Random change in value
      ericValue = Math.max(10, ericValue + change); // Prevent going below $10
      ericValueElement.textContent = `$${ericValue.toFixed(2)}`;

      graphData.push(ericValue);
      if (graphData.length > 20) graphData.shift();

      drawGraph();
    }

    function drawGraph() {
      ctx.clearRect(0, 0, ericGraph.width, ericGraph.height);
      ctx.strokeStyle = "#f39c12";
      ctx.beginPath();
      graphData.forEach((val, index) => {
        const x = (index / (graphData.length - 1)) * ericGraph.width;
        const y = ericGraph.height - ((val - 10) / 90) * ericGraph.height;
        if (index === 0) ctx.moveTo(x, y);
        else ctx.lineTo(x, y);
      });
      ctx.stroke();
    }

    function deposit() {
      const depositAmount = prompt("Enter deposit amount:", "100");
      if (!isNaN(depositAmount) && depositAmount > 0) {
        funds += parseFloat(depositAmount);
        alert(`Deposited $${depositAmount}. Current funds: $${funds}`);
      }
    }

    function withdraw() {
      const withdrawAmount = prompt("Enter withdrawal amount:", "100");
      if (!isNaN(withdrawAmount) && withdrawAmount > 0 && withdrawAmount <= funds) {
        funds -= parseFloat(withdrawAmount);
        alert(`Withdrew $${withdrawAmount}. Current funds: $${funds}`);
      } else {
        alert("Invalid or insufficient funds.");
      }
    }

    function goToRoulette() {
      window.location.href = "roulette.html";
    }

    setInterval(updateEricValue, 2000);
  </script>
</body>
</html>

<!-- Roulette Page: roulette.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Roulette Table</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #222;
      color: #fff;
      text-align: center;
    }

    h1 {
      margin: 20px 0;
    }

    button {
      padding: 10px 20px;
      margin: 5px;
      font-size: 16px;
      background: #f39c12;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Roulette Table</h1>
  <p>Place your bets and spin the wheel!</p>
  <button onclick="spinRoulette()">Spin</button>

  <script>
    function spinRoulette() {
      const outcome = Math.random() > 0.5 ? "Win" : "Lose";
      alert(`You ${outcome}!`);
    }
  </script>
</body>
</html>
