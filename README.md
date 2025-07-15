# Game

<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>隠された数字を当てる心理戦ゲーム</title>
  <style>
    /* 全体の背景 */
    body {
      font-family: 'Arial', sans-serif;
      background: #f5f5f5;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      color: #333;
    }

    /* メインコンテナ */
    .container {
      background: #ffffff;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
      width: 90%;
      max-width: 650px;
      text-align: center;
    }

    /* ヘッダー */
    h1 {
      font-size: 30px;
      color: #4CAF50;
      margin-bottom: 25px;
      font-weight: bold;
    }

    /* 入力フィールド */
    input[type="number"], select {
      padding: 12px;
      font-size: 18px;
      width: 70%;
      border: 2px solid #ccc;
      border-radius: 8px;
      margin-bottom: 20px;
      transition: all 0.3s ease;
    }

    input[type="number"]:focus, select:focus {
      border-color: #4CAF50;
      box-shadow: 0 0 10px rgba(76, 175, 80, 0.3);
    }

    /* ボタン */
    button {
      padding: 15px 30px;
      font-size: 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease, transform 0.2s ease;
      margin-top: 20px;
    }
    button:hover {
      background-color: #45a049;
      transform: scale(1.05);
    }
    button:active {
      transform: scale(0.98);
    }

    /* カードのスタイル */
    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
      gap: 12px;
      margin-top: 20px;
    }

    .card {
      width: 90px;
      height: 90px;
      background-color: #f0f0f0;
      border-radius: 12px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      cursor: pointer;
      transition: transform 0.3s ease, background-color 0.3s ease;
    }

    .card:hover {
      background-color: #e0e0e0;
      transform: scale(1.1);
    }

    .card.flipped {
      background-color: #4CAF50;
      color: white;
    }

    .card.hidden {
      background-color: #bbb;
    }

    /* ヒント表示 */
    .result {
      font-size: 18px;
      font-weight: bold;
      margin-top: 25px;
      color: #333;
    }

    /* 結果表示 */
    .game-result {
      font-size: 20px;
      font-weight: bold;
      color: #d9534f;
      margin-top: 20px;
      transition: opacity 0.5s ease;
    }

    /* 残りターンの表示 */
    #turnsLeft {
      font-size: 18px;
      color: #333;
    }

  </style>
</head>
<body>

  <div id="step1" class="container">
    <h1>隠された数字を当てる心理戦ゲーム</h1>

    <p>ゲームモードを選んでください:</p>
    <select id="gameMode">
      <option value="player">対人戦</option>
      <option value="pc">PC対戦</option>
    </select>

    <p>スタートの数字を選んでください（最小値）:</p>
    <input type="number" id="startNumber" value="1">

    <p>終わりの数字を選んでください（最大値）:</p>
    <input type="number" id="endNumber" value="30">

    <p>隠す数字を2つ選んでください（スタートから終わりの範囲）:</p>
    <input type="number" id="hiddenNumberInput1">
    <input type="number" id="hiddenNumberInput2">

    <p>ヒントの種類を選んでください:</p>
    <select id="hintType">
      <option value="sum">和</option>
      <option value="diff">差</option>
      <option value="product">積</option>
      <option value="quotient">商</option>
      <option value="random">ランダム</option>
    </select>

    <button onclick="startGame()">ゲーム開始</button>
  </div>

  <div id="step2" class="container" style="display: none;">
    <p>指定した範囲のカードを裏返しで配置しています。隠された数字を当ててください！</p>

    <div id="cards" class="cards"></div>

    <div>
      <p>予測した数字を2つ入力:</p>
      <input type="number" id="guessInput1" style="display: none;">
      <input type="number" id="guessInput2" style="display: none;">
      <button onclick="checkGuess()" style="display: none;">予測！</button>
    </div>

    <p id="turnsLeft">残りの回数: <span id="turnCount">4</span></p>
    <p id="hintDisplay" class="result"></p>
  </div>

  <div id="result" class="game-result"></div>

  <script>
    let hiddenNumbers = [];
    let remainingNumbers = [];
    let cardsFlipped = 0;
    let maxTurns = 4;
    let turnsLeft = maxTurns;
    let hintType = '';
    let startRange = 1;
    let endRange = 30;
    let gameMode = 'player';

    function startGame() {
      gameMode = document.getElementById('gameMode').value;
      startRange = parseInt(document.getElementById('startNumber').value);
      endRange = parseInt(document.getElementById('endNumber').value);

      const hiddenNumberInput1 = document.getElementById('hiddenNumberInput1').value;
      const hiddenNumberInput2 = document.getElementById('hiddenNumberInput2').value;
      hiddenNumbers = [parseInt(hiddenNumberInput1), parseInt(hiddenNumberInput2)];

      if (gameMode === 'player') {
        if (hiddenNumbers[0] < startRange || hiddenNumbers[0] > endRange || hiddenNumbers[1] < startRange || hiddenNumbers[1] > endRange || hiddenNumbers[0] === hiddenNumbers[1]) {
          alert('隠された数字は' + startRange + '〜' + endRange + 'の範囲で異なる2つを選んでください！');
          return;
        }
      } else if (gameMode === 'pc') {
        hiddenNumbers = generateHiddenNumbers();
      }

      hintType = document.getElementById('hintType').value;
      let hintMessage = '';
      
      if (hintType === 'sum') {
        const sum = hiddenNumbers[0] + hiddenNumbers[1];
        hintMessage = `隠された数字の和は ${sum} です。`;
      } else if (hintType === 'random') {
        // ランダムに和、差、積、商を選択
        const hintOptions = ['sum', 'diff', 'product', 'quotient'];
        const randomHintType = hintOptions[Math.floor(Math.random() * hintOptions.length)];

        if (randomHintType === 'sum') {
          const sum = hiddenNumbers[0] + hiddenNumbers[1];
          hintMessage = `隠された数字のランダムは ${sum} です。`; // 修正
        } else if (randomHintType === 'diff') {
          hintMessage = `隠された数字のランダムは ${Math.abs(hiddenNumbers[0] - hiddenNumbers[1])} です。`; // 修正
        } else if (randomHintType === 'product') {
          hintMessage = `隠された数字のランダムは ${hiddenNumbers[0] * hiddenNumbers[1]} です。`; // 修正
        } else if (randomHintType === 'quotient') {
          if (hiddenNumbers[1] !== 0) {
            hintMessage = `隠された数字のランダムは ${ (hiddenNumbers[0] / hiddenNumbers[1]).toFixed(2) } です。`; // 小数点以下を表示
          } else {
            hintMessage = "隠された数字の商は定義できません（ゼロ除算）。";
          }
        }
      } else if (hintType === 'diff') {
        hintMessage = `隠された数字の差は ${Math.abs(hiddenNumbers[0] - hiddenNumbers[1])} です。`;
      } else if (hintType === 'product') {
        hintMessage = `隠された数字の積は ${hiddenNumbers[0] * hiddenNumbers[1]} です。`;
      } else if (hintType === 'quotient') {
        if (hiddenNumbers[1] !== 0) {
          hintMessage = `隠された数字の商は ${ (hiddenNumbers[0] / hiddenNumbers[1]).toFixed(2) } です。`; // 小数点以下を表示
        } else {
          hintMessage = "隠された数字の商は定義できません（ゼロ除算）。";
        }
      }

      document.getElementById('hintDisplay').textContent = hintMessage;
      document.getElementById('step1').style.display = 'none';
      document.getElementById('step2').style.display = 'block';

      remainingNumbers = [];
      for (let i = startRange; i <= endRange; i++) {
        if (i !== hiddenNumbers[0] && i !== hiddenNumbers[1]) {
          remainingNumbers.push(i);
        }
      }

      const cardsContainer = document.getElementById('cards');
      for (let i = startRange; i <= endRange; i++) {
        if (i !== hiddenNumbers[0] && i !== hiddenNumbers[1]) {
          const card = document.createElement('div');
          card.className = 'card hidden';
          card.textContent = '';
          card.setAttribute('data-number', i);
          card.onclick = () => revealCard(card);
          cardsContainer.appendChild(card);
        }
      }
    }

    function generateHiddenNumbers() {
      const randomNumber1 = Math.floor(Math.random() * (endRange - startRange + 1)) + startRange;
      let randomNumber2 = Math.floor(Math.random() * (endRange - startRange + 1)) + startRange;
      while (randomNumber2 === randomNumber1) {
        randomNumber2 = Math.floor(Math.random() * (endRange - startRange + 1)) + startRange;
      }
      return [randomNumber1, randomNumber2];
    }

    function revealCard(card) {
      if (card.classList.contains('flipped') || turnsLeft <= 0) {
        return;
      }

      const cardNumber = parseInt(card.getAttribute('data-number'));
      card.style.backgroundColor = '#ddd';
      card.textContent = cardNumber;
      card.classList.add('flipped');
      card.classList.remove('hidden');

      cardsFlipped++;
      turnsLeft--;
      document.getElementById('turnCount').textContent = turnsLeft;

      if (turnsLeft === 0) {
        document.getElementById('guessInput1').style.display = 'block';
        document.getElementById('guessInput2').style.display = 'block';
        document.querySelector('button[onclick="checkGuess()"]').style.display = 'block';
      }
    }

    function checkGuess() {
      const guess1 = parseInt(document.getElementById('guessInput1').value);
      const guess2 = parseInt(document.getElementById('guessInput2').value);

      if (hiddenNumbers.includes(guess1) && hiddenNumbers.includes(guess2)) {
        document.getElementById('result').textContent = `正解！隠された数字は ${hiddenNumbers[0]} と ${hiddenNumbers[1]} です！`;
        document.getElementById('result').style.color = '#5cb85c'; // 成功時は緑
      } else {
        document.getElementById('result').textContent = `不正解。隠された数字は ${hiddenNumbers[0]} と ${hiddenNumbers[1]} です。`;
      }

      document.getElementById('cards').style.display = 'none';
    }
  </script>

</body>
</html>
