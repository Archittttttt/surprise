<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Flower + Birthday Surprise</title>
<style>
  body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: pink;
    overflow: hidden;
  }

  /* Flower Page */
  #flowerPage {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }
  #flowerPage h1 {
    position: absolute;
    top: 30px;
    font-size: 2em;
    color: white;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
  }
  .flower {
    position: absolute;
    font-size: 2rem;
    animation: pop 6s linear forwards;
  }
  @keyframes pop {
    0% { transform: scale(0) translateY(100vh); opacity: 0; }
    20% { transform: scale(1) translateY(0); opacity: 1; }
    80% { opacity: 1; }
    100% { transform: scale(0) translateY(-100vh); opacity: 0; }
  }

  /* Birthday Page */
  #birthdayPage { display: none; text-align: center; padding: 40px; }
  button {
    padding: 12px 24px; font-size: 18px; margin: 10px; border: none;
    border-radius: 8px; background-color: #ff69b4; color: white; cursor: pointer;
  }
  input[type="text"] {
    padding: 10px; font-size: 18px; border-radius: 6px;
    border: 1px solid #ccc; margin-top: 10px;
  }
  h1 { color: #ff4081; font-size: 3em; }
  .message { font-size: 1.5em; margin-top: 20px; color: #d6006b; }

  .cake-container {
    position: relative; width: 200px; height: 200px; margin: 40px auto 0;
    animation: bounce 1s infinite;
  }
  .cake-base {
    width: 200px; height: 120px; background: #ffb347;
    border-radius: 0 0 20px 20px; position: absolute; bottom: 0;
  }
  .cake-cream {
    width: 200px; height: 30px; background: #c8a2c8;
    border-radius: 15px; position: absolute; top: 0;
  }
  .candle {
    width: 10px; height: 40px; background: #ff69b4;
    position: absolute; top: -40px; left: 95px;
  }
  .flame {
    width: 15px; height: 15px; background: orange; border-radius: 50%;
    position: absolute; top: -15px; left: -3px; animation: flicker 0.3s infinite;
  }
  @keyframes bounce {
    0% { transform: translateY(0); }
    50% { transform: translateY(-15px); }
    100% { transform: translateY(0); }
  }
  @keyframes flicker {
    0%, 100% { transform: scaleY(1) scaleX(1); }
    50% { transform: scaleY(0.9) scaleX(1.1); }
  }

  /* Ending Page */
  #endingPage {
    display:none; position:relative; height:100vh; overflow:hidden;
    background:pink; text-align:center; padding-top:80px;
  }
  .balloon {
    position: absolute; bottom: -150px; width: 50px; height: 70px;
    border-radius: 50%; animation: rise 6s linear forwards;
  }
  .balloon::after {
    content: ""; position: absolute; top: 70px; left: 50%;
    width: 2px; height: 50px; background: #555; transform: translateX(-50%);
  }
  @keyframes rise {
    0% { transform: translateY(0) scale(1); opacity: 1; }
    100% { transform: translateY(-110vh) scale(1.1); opacity: 0; }
  }

  /* Thank You Page */
  #thankYouPage {
    display:none; height:100vh;
    background:linear-gradient(to bottom, #ffb6c1, #ff69b4);
    text-align:center; color:white; padding-top:40vh;
    font-size:2em; animation: fadeIn 2s ease-in; position: relative; overflow: hidden;
  }
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  .confetti {
    position: absolute; width: 10px; height: 10px; background: yellow;
    animation: fall 3s linear infinite;
  }
  @keyframes fall {
    0% { transform: translateY(0) rotate(0); opacity: 1; }
    100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
  }

  /* Not Yet Page */
  #waitPage {
    height:100vh; display:flex; justify-content:center; align-items:center;
    font-size:2em; color:#ff4081; text-align:center;
  }
</style>
</head>
<body>

<!-- Wait Page -->
<div id="waitPage">🎁 The surprise will be available on 12 August 2025 🎁</div>

<!-- Flower Page -->
<div id="flowerPage" style="display:none;">
  <h1>💐 Surprise! 🌸</h1>
</div>

<!-- Birthday Page -->
<div id="birthdayPage">
  <div id="welcome">
    <h1>🎁 Do you want to see a surprise?</h1>
    <button onclick="askAgain(false)">No</button>
    <button onclick="askAgain(true)">Yes</button>
  </div>
  <div id="namePage" style="display:none;">
    <h1>🎈 What's your name? 🎈</h1>
    <input type="text" id="nameInput" placeholder="Enter your name"><br>
    <button onclick="submitName()">Submit</button>
  </div>
  <div id="cakePage" style="display:none;">
    <div class="cake-container">
      <div class="cake-base"></div>
      <div class="cake-cream"></div>
      <div class="candle"><div class="flame"></div></div>
    </div>
    <p id="wishMessage" class="message"></p>
  </div>
</div>

<!-- Ending Page -->
<div id="endingPage">
  <h1 style="color:#ff4081; font-size:2.5em;">🎊 Wishing you a day filled with smiles and surprises 🎊</h1>
  <p style="font-size:1.5em; color:#d6006b;">
    Shristy, may your year be filled with laughter, love, and all the sweet moments you deserve 💖
  </p>
</div>

<!-- Thank You Page -->
<div id="thankYouPage">
  <p>🙏 Thank you that's all peace! 🎉</p>
</div>

<script>
  const targetDate = new Date("2025-08-12T00:00:00");

  function checkTime() {
    const now = new Date();
    if (now >= targetDate) {
      document.getElementById("waitPage").style.display = "none";
      startFlowers();
    } else {
      setTimeout(checkTime, 1000);
    }
  }

  function startFlowers() {
    document.getElementById("flowerPage").style.display = "flex";
    const flowerEmojis = ["🌸", "🌹", "🌻", "🌼", "💮", "🌷"];
    function createFlower() {
      const flower = document.createElement("div");
      flower.classList.add("flower");
      flower.textContent = flowerEmojis[Math.floor(Math.random() * flowerEmojis.length)];
      flower.style.left = Math.random() * 100 + "vw";
      flower.style.fontSize = (20 + Math.random() * 30) + "px";
      flower.style.animationDuration = (4 + Math.random() * 3) + "s";
      document.getElementById("flowerPage").appendChild(flower);
      setTimeout(() => flower.remove(), 6000);
    }
    const flowerInterval = setInterval(createFlower, 300);

    setTimeout(() => {
      clearInterval(flowerInterval);
      document.getElementById("flowerPage").style.display = "none";
      document.getElementById("birthdayPage").style.display = "block";
    }, 5000);
  }

  function askAgain(response) {
    if (response) {
      document.getElementById("welcome").style.display = "none";
      document.getElementById("namePage").style.display = "block";
    } else {
      alert("Come on! You're gonna love it 😄");
    }
  }

  function submitName() {
    const name = document.getElementById("nameInput").value.trim();
    if (name === "") {
      alert("Please enter a name!");
      return;
    }
    document.getElementById("namePage").style.display = "none";
    document.getElementById("cakePage").style.display = "block";
    document.getElementById("wishMessage").innerText =
      `🎉 Happy Birthday, ${name}! 🎉`;
    setTimeout(() => {
      document.getElementById("cakePage").style.display = "none";
      document.getElementById("endingPage").style.display = "block";
      startBalloons("endingPage");
      setTimeout(() => {
        document.getElementById("endingPage").style.display = "none";
        document.getElementById("thankYouPage").style.display = "block";
        startBalloons("thankYouPage");
        startConfetti();
      }, 5000);
    }, 4000);
  }

  function startBalloons(containerId) {
    const colors = ["#ff69b4", "#ff4500", "#ffd700", "#87ceeb", "#32cd32"];
    for (let i = 0; i < 20; i++) {
      const balloon = document.createElement("div");
      balloon.className = "balloon";
      balloon.style.left = Math.random() * 100 + "vw";
      balloon.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
      balloon.style.animationDuration = (5 + Math.random() * 3) + "s";
      document.getElementById(containerId).appendChild(balloon);
      setTimeout(() => balloon.remove(), 8000);
    }
  }

  function startConfetti() {
    const colors = ["#ff0", "#0f0", "#0ff", "#f0f", "#f00", "#00f"];
    for (let i = 0; i < 50; i++) {
      const conf = document.createElement("div");
      conf.className = "confetti";
      conf.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
      conf.style.left = Math.random() * 100 + "vw";
      conf.style.top = -10 + "px";
      conf.style.animationDuration = (2 + Math.random() * 3) + "s";
      document.getElementById("thankYouPage").appendChild(conf);
      setTimeout(() => conf.remove(), 5000);
    }
  }

  checkTime();
</script>
</body>
</html>
