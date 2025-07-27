<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>타자 연습 게임</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: white;
      text-align: center;
      margin-top: 50px;
    }
    #gameArea, #wordMode, #sentenceMode, #typingGame {
      display: none;
    }
    button {
      font-size: 20px;
      padding: 10px 20px;
      margin: 10px;
    }
    #inputField {
      font-size: 20px;
      padding: 5px;
    }
    #wordDisplay {
      font-size: 30px;
      margin: 20px;
    }
  </style>
</head>
<body>
  <div id="startScreen">
    <button onclick="showModeSelection()">게임 시작하기</button>
  </div>

  <div id="gameArea">
    <button onclick="startWordGame()">단어</button>
    <button onclick="startSentenceMode()">문장</button>
  </div>

  <div id="sentenceMode">
    <h2>준비중...</h2>
  </div>

  <div id="typingGame">
    <div id="timer">남은 시간: 60초</div>
    <div id="wordDisplay"></div>
    <input type="text" id="inputField" autocomplete="off" />
    <div>점수: <span id="score">0</span></div>
    <div>최고 점수: <span id="highScore">0</span></div>
    <br />
    <button id="restartBtn" style="display:none;" onclick="goToStartScreen()">다시 시작하기</button>
  </div>

  <script>
    const words = [
      "실", "핸드폰", "용암", "물", "키보드", "컴퓨터", "엄마", "아빠", "형", "동생", "타자", "연습", "영어",
      "프랑스", "독일", "이탈리아", "종이", "지우개", "연필", "비행기", "방망이", "게", "개", "기도", "블록",
      "호랑이", "확률", "운", "사람", "카펫", "소년", "소녀", "기름", "나무", "막대기", "여우", "석탄", "트럭",
      "자동차", "연못", "진흙", "영화", "사고", "운전", "팝콘", "의자", "볼펜", "친구", "자연", "서울", "강원도",
      "경기도", "안양", "장작", "숲", "오두막", "병원", "일본", "라면", "식사", "지갑", "돈", "휴지", "바나나",
      "양복", "유리", "참외", "키위", "곰팡이"
    ];

    let currentWord = "";
    let score = 0;
    let timeLeft = 60;
    let timerInterval;

    function showModeSelection() {
      document.getElementById("startScreen").style.display = "none";
      document.getElementById("gameArea").style.display = "block";
      document.getElementById("typingGame").style.display = "none";
      document.getElementById("restartBtn").style.display = "none"; // 숨김
    }

    function startWordGame() {
      document.getElementById("gameArea").style.display = "none";
      document.getElementById("typingGame").style.display = "block";
      document.getElementById("inputField").disabled = false;
      document.getElementById("inputField").focus();
      score = 0;
      timeLeft = 60;
      document.getElementById("score").textContent = score;
      document.getElementById("highScore").textContent = localStorage.getItem("highScore") || 0;
      document.getElementById("timer").textContent = `남은 시간: ${timeLeft}초`;
      document.getElementById("restartBtn").style.display = "none"; // 게임 시작할 때 다시하기 숨김
      nextWord();
      timerInterval = setInterval(() => {
        timeLeft--;
        document.getElementById("timer").textContent = `남은 시간: ${timeLeft}초`;
        if (timeLeft <= 0) endGame();
      }, 1000);
    }

    function startSentenceMode() {
      document.getElementById("gameArea").style.display = "none";
      document.getElementById("sentenceMode").style.display = "block";
      setTimeout(() => {
        document.getElementById("sentenceMode").style.display = "none";
        document.getElementById("startScreen").style.display = "block";
      }, 3000);
    }

    function nextWord() {
      currentWord = words[Math.floor(Math.random() * words.length)];
      document.getElementById("wordDisplay").textContent = currentWord;
      const input = document.getElementById("inputField");
      input.value = "";
      input.focus();
    }

    document.getElementById("inputField").addEventListener("keydown", function (e) {
      if (e.key === "Enter") {
        if (this.value.trim() === currentWord) {
          score++;
          document.getElementById("score").textContent = score;
          nextWord();
        }
      }
    });

    function endGame() {
      clearInterval(timerInterval);
      document.getElementById("inputField").disabled = true;
      const savedHighScore = localStorage.getItem("highScore") || 0;
      if (score > savedHighScore) {
        localStorage.setItem("highScore", score);
        document.getElementById("highScore").textContent = score;
      }
      // 다시 시작 버튼 보이기
      document.getElementById("restartBtn").style.display = "inline-block";
    }

    function goToStartScreen() {
      clearInterval(timerInterval);
      document.getElementById("typingGame").style.display = "none";
      document.getElementById("gameArea").style.display = "none";
      document.getElementById("sentenceMode").style.display = "none";
      document.getElementById("startScreen").style.display = "block";
      document.getElementById("restartBtn").style.display = "none";
      document.getElementById("inputField").disabled = false;
    }
  </script>
</body>
</html>

