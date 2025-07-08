<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>The Block Game</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap" rel="stylesheet" />
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #0d0d0d, #1a1a1a);
      color: #eee;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px 20px;
      min-height: 100vh;
    }
    h1 {
      color: #D4AF37;
      font-weight: 700;
      font-size: 3rem;
      letter-spacing: 2px;
      margin-bottom: 5px;
      text-shadow: 0 0 8px #D4AF37;
    }
    .subtitle {
      color: #ff4d4d;
      font-weight: 700;
      font-size: 1.1rem;
      margin-bottom: 30px;
      letter-spacing: 1.2px;
      text-shadow: 0 0 5px #ff4d4d;
    }
    .controls-wrapper {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 40px;
      margin-bottom: 40px;
      width: 100%;
      max-width: 700px;
    }
    .row {
      display: flex;
      gap: 20px;
      width: 100%;
      max-width: 700px;
      justify-content: center;
    }
    .input-group {
      background: #222;
      border: 2px solid #444;
      border-radius: 8px;
      padding: 15px 20px;
      text-align: center;
      flex: 1 1 150px;
      min-width: 150px;
      transition: border-color 0.3s ease, box-shadow 0.3s ease;
    }
    .input-group:hover:not(:disabled) {
      border-color: #D4AF37;
      box-shadow: 0 0 15px #D4AF37;
    }
    .input-group label {
      display: block;
      font-weight: 600;
      color: #D4AF37;
      margin-bottom: 10px;
      font-size: 1.1rem;
      letter-spacing: 1px;
      user-select: none;
    }
    .balance-label {
      font-size: 1.1rem;
      color: #D4AF37;
      font-weight: 700;
      user-select: none;
      display: inline-block;
      margin-top: 5px;
      letter-spacing: 1.5px;
      text-shadow: 0 0 5px #D4AF37;
    }
    input[type="text"],
    input[type="number"] {
      width: 100%;
      padding: 12px 15px;
      font-size: 1rem;
      border-radius: 6px;
      border: none;
      outline: none;
      background: #111;
      color: #eee;
      box-shadow: inset 0 0 5px #000;
      font-weight: 600;
      text-align: center;
    }
    input[type="text"]:disabled,
    input[type="number"]:disabled {
      background: #333;
      color: #888;
      cursor: not-allowed;
      box-shadow: none;
    }
    input[type="text"]::placeholder,
    input[type="number"]::placeholder {
      color: #777;
    }
    input:focus:not(:disabled) {
      box-shadow: inset 0 0 8px #D4AF37;
    }
    #startButton {
      background: linear-gradient(90deg, #D4AF37, #b8892a);
      border: none;
      border-radius: 30px;
      padding: 15px 50px;
      font-size: 1.25rem;
      font-weight: 700;
      color: #1a1a1a;
      cursor: pointer;
      box-shadow: 0 5px 15px rgba(212, 175, 55, 0.6);
      transition: all 0.3s ease;
      user-select: none;
      letter-spacing: 1.2px;
      margin-bottom: 10px;
    }
    #startButton:hover:enabled {
      background: linear-gradient(90deg, #b8892a, #D4AF37);
      box-shadow: 0 0 20px 5px #D4AF37;
      transform: scale(1.05);
    }
    #startButton:disabled {
      opacity: 0.5;
      cursor: not-allowed;
      transform: none;
      box-shadow: none;
    }

    #gameWrapper {
      display: flex;
      justify-content: center;
      width: 100%;
      margin-top: 30px;
    }
    #gameContainer {
      display: grid;
      grid-template-columns: repeat(10, minmax(0, 1fr));
      gap: 10px;
      width: max-content;
      max-width: 100%;
    }
    .cell {
      background: linear-gradient(145deg, #b8892a, #D4AF37);
      border-radius: 12px;
      box-shadow: inset 0 0 10px #fff9cc, 0 4px 8px rgba(0, 0, 0, 0.4);
      color: #1a1a1a;
      font-weight: 800;
      font-size: 2rem;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      aspect-ratio: 1 / 1;
      width: 80px;
      max-width: 100%;
      user-select: none;
      transition: box-shadow 0.3s ease, transform 0.2s ease;
      position: relative;
    }
    .cell.clicked {
      cursor: default;
      box-shadow: inset 0 0 25px #fff9cc, 0 0 20px 4px #D4AF37;
      transform: translateY(0);
      pointer-events: none;
    }
    .cell:hover:not(.clicked) {
      box-shadow: inset 0 0 20px #fff9cc, 0 6px 12px rgba(212, 175, 55, 0.9);
      transform: translateY(-5px);
    }
    .message {
      margin-top: 20px;
      font-size: 1.5rem;
      font-weight: 700;
      color: #D4AF37;
      text-shadow: 0 0 10px #D4AF37;
      user-select: none;
      min-height: 30px;
      text-align: center;
    }
    #multiplierPreview {
      font-size: 1.25rem;
      color: #ccc;
      margin-top: 5px;
      min-height: 24px;
      user-select: none;
      text-shadow: 0 0 5px #888;
      transition: all 0.3s ease;
    }
  </style>
</head>
<body>
  <h1>The Block Game</h1>
  <div class="subtitle">
    Maximum Win: 1,000,000 bits &bull; Maximum Bet: 1,000,000 bits &bull; Maximum Total Blocks: 1,000
  </div>

  <div class="controls-wrapper">
    <div class="row">
      <div class="input-group">
        <label for="balance">Balance</label>
        <div><span id="balance" class="balance-label">1,000.00</span> bits</div>
      </div>
      <div class="input-group">
        <label for="betAmount">Bet Amount</label>
        <input type="text" id="betAmount" value="100" inputmode="decimal" maxlength="8" />
      </div>
    </div>
    <div class="row">
      <div class="input-group">
        <label for="totalBlocks">Total Blocks</label>
        <input type="text" id="totalBlocks" value="10" maxlength="5" />
      </div>
      <div class="input-group">
        <label for="payBlocks">Pay Blocks</label>
        <input type="number" id="payBlocks" value="1" min="1" max="999" maxlength="3" />
      </div>
      <div class="input-group">
        <label for="multiplierPreview">Payout Multiplier</label>
        <span id="multiplierPreview" class="balance-label">—</span>
      </div>
    </div>
  </div>

  <button id="startButton" type="button">Start Game</button>
  <div id="gameWrapper">
    <div id="gameContainer"></div>
  </div>
  <div class="message" id="message"></div>

  <script>
    let balance = 1000.00;
    const MAX_WIN = 1000000;
    const MAX_BLOCKS = 1000;
    const MAX_BET = 1000000;

    function truncateToTwoDecimals(num) {
      return Math.floor(num * 100) / 100;
    }

    function formatWithCommas(value) {
      let numStr = value.replace(/,/g, '');
      if (numStr === '') return '';
      return numStr.replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }

    function updateMultiplierPreview() {
      const totalInput = document.getElementById("totalBlocks");
      const payInput = document.getElementById("payBlocks");
      const betInput = document.getElementById("betAmount");
      const multiplierEl = document.getElementById("multiplierPreview");
      const startBtn = document.getElementById("startButton");

      let rawTotal = totalInput.value.replace(/,/g, '').slice(0, 5);
      let rawBet = betInput.value.replace(/,/g, '').slice(0, 8);
      payInput.value = payInput.value.replace(/[^\d]/g, '').slice(0, 3);

      totalInput.value = formatWithCommas(rawTotal);
      betInput.value = formatWithCommas(rawBet);

      let total = parseInt(rawTotal);
      let pay = parseInt(payInput.value);
      let bet = parseFloat(rawBet.replace(/,/g, ''));

      if (
        isNaN(total) || isNaN(pay) || isNaN(bet) ||
        total < 2 || pay < 1 || pay >= total ||
        total > MAX_BLOCKS || bet <= 0 || bet > balance || bet > MAX_BET
      ) {
        multiplierEl.textContent = '—';
        startBtn.disabled = true;
        return;
      }

      const houseEdge = 0.005;
      const rawMultiplier = (total / pay) * (1 - houseEdge);
      const payoutMultiplier = truncateToTwoDecimals(rawMultiplier);

      multiplierEl.textContent = `${payoutMultiplier.toFixed(2)}x`;
      startBtn.disabled = false;
    }

    function updateBalanceDisplay() {
      document.getElementById("balance").textContent = balance.toLocaleString(undefined, { minimumFractionDigits: 2 });
      updateMultiplierPreview();
    }

    function startGame() {
      const container = document.getElementById("gameContainer");
      const message = document.getElementById("message");
      const totalInput = document.getElementById("totalBlocks");
      const payInput = document.getElementById("payBlocks");
      const betInput = document.getElementById("betAmount");
      const startBtn = document.getElementById("startButton");

      const totalStr = totalInput.value.replace(/,/g, '').trim();
      const payStr = payInput.value.trim();
      const betStr = betInput.value.replace(/,/g, '').trim();

      const total = parseInt(totalStr, 10);
      const pay = parseInt(payStr, 10);
      const bet = parseFloat(betStr);

      if (
        isNaN(total) || total < 2 || total > MAX_BLOCKS ||
        isNaN(pay) || pay < 1 || pay >= total ||
        isNaN(bet) || bet <= 0 || bet > balance || bet > MAX_BET
      ) {
        alert('Invalid input or insufficient balance to place bet.');
        return;
      }

      container.innerHTML = '';
      message.textContent = '';

      totalInput.disabled = true;
      payInput.disabled = true;
      betInput.disabled = true;
      startBtn.disabled = true;

      const columns = total < 10 ? total : 10;
      container.style.gridTemplateColumns = `repeat(${columns}, minmax(0, 1fr))`;

      const winningIndexes = new Set();
      while (winningIndexes.size < pay) {
        winningIndexes.add(Math.floor(Math.random() * total));
      }

      let balanceChanged = false;

      for (let i = 0; i < total; i++) {
        const cell = document.createElement("div");
        cell.className = "cell";
        cell.textContent = i + 1;

        cell.addEventListener("click", function () {
          if (cell.classList.contains("clicked")) return;

          cell.classList.add("clicked");

          if (winningIndexes.has(i)) {
            cell.style.background = '#00ff88';
            cell.textContent = "💎";
          } else {
            cell.style.background = '#444';
            cell.textContent = "💀";
          }

          for (let j = 0; j < container.children.length; j++) {
            const c = container.children[j];
            if (!c.classList.contains("clicked")) {
              c.classList.add("clicked");
              if (winningIndexes.has(j)) {
                c.style.background = '#00ff88';
                c.textContent = "💎";
              } else {
                c.style.background = '#444';
                c.textContent = "💀";
              }
            }
          }

          if (!balanceChanged) {
            const multiplierText = document.getElementById("multiplierPreview").textContent;
            const multiplier = parseFloat(multiplierText);

            let winnings = 0;
            if (winningIndexes.has(i)) {
              winnings = bet * multiplier;
            }

            balance += winnings - bet;

            if (isNaN(balance) || balance < 0) balance = 0;

            updateBalanceDisplay();

            const netProfit = winnings - bet;
            message.textContent = (netProfit > 0)
              ? `You won ${netProfit.toFixed(2)} bits!`
              : `You lost ${bet.toFixed(2)} bits.`;

            balanceChanged = true;

            totalInput.disabled = false;
            payInput.disabled = false;
            betInput.disabled = false;
            startBtn.disabled = false;
          }
        });
        container.appendChild(cell);
      }
    }

    function sanitizeNumericInput(el, maxLength) {
      el.addEventListener('input', () => {
        let cleaned = el.value.replace(/[^\d]/g, '');
        cleaned = cleaned.slice(0, maxLength);
        let withCommas = cleaned.replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        if (el.value !== withCommas) {
          el.value = withCommas;
        }
      });
    }

    document.getElementById("totalBlocks").addEventListener("input", updateMultiplierPreview);
    document.getElementById("payBlocks").addEventListener("input", updateMultiplierPreview);
    document.getElementById("betAmount").addEventListener("input", updateMultiplierPreview);
    document.getElementById("startButton").addEventListener("click", startGame);

    sanitizeNumericInput(document.getElementById("betAmount"), 8);
    sanitizeNumericInput(document.getElementById("totalBlocks"), 5);

    updateBalanceDisplay();
  </script>
</body>
</html>



