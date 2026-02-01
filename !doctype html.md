#   
  
<!doctype html>  
<html lang="en">  
<head>  
  <meta charset="utf-8" />  
  <meta name="viewport" content="width=device-width,initial-scale=1" />  
  <title>Valentine</title>  
  
  <style>  
    body {  
      margin: 0;  
      min-height: 100vh;  
      display: grid;  
      place-items: center;  
      background: linear-gradient(135deg,#ffd6e7,#fff1f6);  
      font-family: system-ui, Arial;  
      overflow: hidden;  
    }  
  
    .card {  
      background: white;  
      padding: 32px;  
      border-radius: 20px;  
      text-align: center;  
      width: min(520px, 90vw);  
      box-shadow: 0 20px 40px rgba(0,0,0,.12);  
      position: relative;  
      z-index: 2;  
    }  
  
    h1 {  
      font-size: 30px;  
      margin-bottom: 20px;  
      color: #d6336c;  
    }  
  
    .stage {  
      height: 170px;  
      position: relative;  
      margin-top: 20px;  
    }  
  
    button {  
      font-size: 20px;  
      padding: 12px 26px;  
      border-radius: 999px;  
      border: none;  
      cursor: pointer;  
      position: absolute;  
      user-select: none;  
      touch-action: manipulation;  
    }  
  
    #yes {  
      left: 80px;  
      top: 60px;  
      background: #ff4d8d;  
      color: white;  
      transition: transform .2s ease;  
    }  
  
    #no {  
      left: 240px;  
      top: 60px;  
      background: #eee;  
      color: #111;  
    }  
  
    #msg {  
      margin-top: 20px;  
      font-size: 22px;  
      color: #ff4d8d;  
      font-weight: 700;  
      min-height: 28px;  
    }  
  
    .heart {  
      position: fixed;  
      bottom: -40px;  
      font-size: 28px;  
      animation: floatUp 6s linear infinite;  
      opacity: .85;  
      z-index: 1;  
      pointer-events: none;  
    }  
  
    @keyframes floatUp {  
      from { transform: translateY(0); opacity: .9; }  
      to { transform: translateY(-110vh); opacity: 0; }  
    }  
  </style>  
</head>  
  
<body>  
  <div class="card">  
    <h1>Saima, will you be my Valentine? ❤️</h1>  
  
    <div class="stage" id="stage">  
      <button id="yes" type="button">Yes</button>  
      <button id="no" type="button">No</button>  
    </div>  
  
    <div id="msg"></div>  
  </div>  
  
  <script>  
    const stage = document.getElementById("stage");  
    const noBtn = document.getElementById("no");  
    const yesBtn = document.getElementById("yes");  
    const msg = document.getElementById("msg");  
  
    let yesScale = 1;  
    let answered = false;  
  
    function clamp(n, min, max) {  
      return Math.max(min, Math.min(max, n));  
    }  
  
    function moveNo() {  
      if (answered) return;  
  
      const push = 90;  
      const angle = Math.random() * Math.PI * 2;  
  
      let newLeft = noBtn.offsetLeft + Math.cos(angle) * push;  
      let newTop  = noBtn.offsetTop  + Math.sin(angle) * push;  
  
      newLeft = clamp(newLeft, 10, stage.clientWidth - noBtn.offsetWidth - 10);  
      newTop  = clamp(newTop, 10, stage.clientHeight - noBtn.offsetHeight - 10);  
  
      noBtn.style.left = newLeft + "px";  
      noBtn.style.top  = newTop + "px";  
  
      yesScale += 0.05;  
      yesBtn.style.transform = `scale(${yesScale})`;  
    }  
  
    // Run away when the pointer gets close to No (desktop)  
    stage.addEventListener("mousemove", (e) => {  
      if (answered) return;  
  
      const r = noBtn.getBoundingClientRect();  
      const near =  
        e.clientX > r.left - 50 &&  
        e.clientX < r.right + 50 &&  
        e.clientY > r.top - 50 &&  
        e.clientY < r.bottom + 50;  
  
      if (near) moveNo();  
    });  
  
    // Run away on touch (mobile)  
    stage.addEventListener("touchstart", () => moveNo(), { passive: true });  
  
    // Extra safety if they somehow reach it  
    noBtn.addEventListener("mouseenter", () => moveNo());  
    noBtn.addEventListener("click", (e) => {  
      e.preventDefault();  
      moveNo();  
    });  
  
    yesBtn.addEventListener("click", () => {  
      answered = true;  
  
      yesScale = 1.6;  
      yesBtn.style.transform = `scale(${yesScale})`;  
  
      msg.textContent = "SHAHRIAR LOVES YOU SO SO MUCHH ❤️ AND HE WANTS YOU FOREVER";  
  
      spawnHearts();  
    });  
  
    function spawnHearts() {  
      for (let i = 0; i < 35; i++) {  
        const h = document.createElement("div");  
        h.className = "heart";  
        h.textContent = "❤️";  
        h.style.left = (Math.random() * 100) + "vw";  
        h.style.animationDuration = (4 + Math.random() * 3) + "s";  
        h.style.fontSize = (20 + Math.random() * 22) + "px";  
        document.body.appendChild(h);  
        setTimeout(() => h.remove(), 7500);  
      }  
    }  
  </script>  
</body>  
</html>  
