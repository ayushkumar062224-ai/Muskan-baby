<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8" />
  <title>Maafi ❤️</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Great+Vibes&family=Pacifico&family=Playfair+Display:wght@400;700&family=Qwitcher+Grypen:wght@700&display=swap" rel="stylesheet">

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      height: 100vh;
      width: 100vw;
      overflow: hidden;
      background: radial-gradient(circle at top, #ffdde1, #ee9ca7, #3a1c71);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
      font-family: "Poppins", sans-serif;
    }

    .overlay {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.35);
      pointer-events: none;
    }

    .container {
      position: relative;
      z-index: 2;
      width: 90%;
      max-width: 600px;
      min-height: 60vh;
      padding: 24px 20px 18px;
      border-radius: 24px;
      backdrop-filter: blur(18px);
      background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.02));
      box-shadow: 0 18px 40px rgba(0, 0, 0, 0.38);
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .hearts {
      position: fixed;
      inset: 0;
      overflow: hidden;
      z-index: 1;
      pointer-events: none;
    }

    .heart {
      position: absolute;
      font-size: 18px;
      animation: floatUp linear infinite;
      opacity: 0.8;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(100vh) scale(0.7);
        opacity: 0;
      }
      20% {
        opacity: 0.9;
      }
      100% {
        transform: translateY(-10vh) scale(1.2);
        opacity: 0;
      }
    }

    .slide-count {
      font-size: 13px;
      letter-spacing: 1px;
      text-transform: uppercase;
      opacity: 0.8;
      margin-bottom: 4px;
    }

    .title-small {
      font-size: 16px;
      opacity: 0.85;
      margin-bottom: 8px;
    }

    .text-box {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 12px;
      line-height: 1.5;
      animation: fadeIn 0.7s ease;
    }

    .main-text {
      font-size: 22px;
      font-weight: 500;
    }

    .buttons {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
      gap: 10px;
    }

    button {
      flex: 1;
      padding: 10px 14px;
      border-radius: 999px;
      border: none;
      outline: none;
      cursor: pointer;
      font-size: 15px;
      font-weight: 500;
      font-family: inherit;
      background: rgba(255, 255, 255, 0.93);
      color: #5b1d58;
      box-shadow: 0 8px 18px rgba(0, 0, 0, 0.25);
      transition: transform 0.15s ease, box-shadow 0.15s ease, background 0.2s;
    }

    button:active {
      transform: translateY(1px) scale(0.98);
      box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
    }

    button.secondary {
      background: transparent;
      border: 1px solid rgba(255, 255, 255, 0.6);
      color: #fff;
      box-shadow: none;
    }

    .song-note {
      font-size: 11px;
      opacity: 0.8;
      margin-top: 4px;
      text-align: center;
    }

    .hidden {
      display: none;
    }

    .final-options {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 12px;
    }

    .final-options button {
      flex: unset;
      width: 100%;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @media (max-width: 480px) {
      .container {
        padding: 18px 15px 14px;
        min-height: 65vh;
      }
      .main-text {
        font-size: 19px;
      }
    }
  </style>
</head>
<body>

  <!-- Dil ke upar halka dark overlay -->
  <div class="overlay"></div>

  <!-- Floating hearts background -->
  <div class="hearts" id="hearts"></div>

  <!-- Background song -->
  <!-- IMPORTANT: yahan src me apna mp3 file ka naam/URL daalna -->
  <audio id="bgMusic" loop>
    <!-- Example: agar file ka naam song.mp3 hai to same folder me rakhna -->
    <source src="song.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <div class="container">
    <div>
      <div class="slide-count" id="slideCount">1 / 5</div>
      <div class="title-small" id="smallTitle">Mujhe kuch kehna hai…</div>
      <div class="text-box">
        <div class="main-text" id="mainText">
          Loading...
        </div>
      </div>
    </div>

    <div>
      <div class="buttons" id="normalButtons">
        <button class="secondary" id="prevBtn">⟵ Pehle wala</button>
        <button id="nextBtn">Aage ⟶</button>
      </div>

      <div class="final-options hidden" id="finalButtons">
        <button id="yesBtn">Haan, wapas aao 💖</button>
        <button class="secondary" id="noBtn">Thoda time chahiye…</button>
      </div>

      <p class="song-note">
        Agar song nahi baj raha ho to upar screen par kahin bhi tap karke “Play” allow karo. 🔊
      </p>
    </div>
  </div>

  <script>
    // ---- TEXT SLIDES YAHAN CHANGE KAR SAKTE HO ----
    const slides = [
      {
        text: "Shayad main perfect nahi hoon, lekin jo bhi hoon, dil se sirf tumhara hoon. 💔",
        fontFamily: "'Playfair Display', serif",
        smallTitle: "Sabse pehle…"
      },
      {
        text: "Jo galtiyan maine ki hain, unke liye main dil se maafi maangta hoon. Main tumhe hurt karna kabhi nahi chahta tha.",
        fontFamily: "'Poppins', sans-serif",
        smallTitle: "Meri galti…"
      },
      {
        text: "Tumhare bina sab kuch adhoora sa lagta hai. Chats, calls, vo chhoti chhoti baatein… sab yaad aati hain.",
        fontFamily: "'Pacifico', cursive",
        smallTitle: "Tumhari yaadein…"
      },
      {
        text: "Main change hone ki puri koshish kar raha hoon, taaki tum hamesha proud feel karo ki tumne mujhe choose kiya. ✨",
        fontFamily: "'Great Vibes', cursive",
        smallTitle: "Main koshish karunga…"
      },
      {
        text: "Ek last sawaal… kya tum wapas mere sath relationship me rehna chahogi? 💌",
        fontFamily: "'Qwitcher Grypen', cursive",
        smallTitle: "Bas ek sawaal…",
        isFinal: true
      }
    ];

    const mainText = document.getElementById("mainText");
    const smallTitle = document.getElementById("smallTitle");
    const slideCount = document.getElementById("slideCount");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");
    const normalButtons = document.getElementById("normalButtons");
    const finalButtons = document.getElementById("finalButtons");
    const yesBtn = document.getElementById("yesBtn");
    const noBtn = document.getElementById("noBtn");
    const bgMusic = document.getElementById("bgMusic");

    let currentSlide = 0;

    function renderSlide() {
      const slide = slides[currentSlide];

      mainText.style.opacity = 0;
      smallTitle.style.opacity = 0;

      setTimeout(() => {
        mainText.textContent = slide.text;
        mainText.style.fontFamily = slide.fontFamily;
        smallTitle.textContent = slide.smallTitle || "";

        slideCount.textContent = (currentSlide + 1) + " / " + slides.length;

        mainText.style.opacity = 1;
        smallTitle.style.opacity = 1;
      }, 150);

      // Buttons handle
      if (slide.isFinal) {
        normalButtons.classList.add("hidden");
        finalButtons.classList.remove("hidden");
      } else {
        normalButtons.classList.remove("hidden");
        finalButtons.classList.add("hidden");
      }

      prevBtn.disabled = currentSlide === 0;
      prevBtn.style.opacity = currentSlide === 0 ? 0.4 : 1;
    }

    prevBtn.addEventListener("click", () => {
      if (currentSlide > 0) {
        currentSlide--;
        renderSlide();
      }
    });

    nextBtn.addEventListener("click", () => {
      if (currentSlide < slides.length - 1) {
        currentSlide++;
        renderSlide();
      }
    });

    yesBtn.addEventListener("click", () => {
      mainText.textContent = "Thank you... tumne 'haan' kaha. Main is baar tumhe kabhi disappoint nahi karunga. 💖";
      smallTitle.textContent = "Tum meri ho.";
      normalButtons.classList.add("hidden");
      finalButtons.classList.add("hidden");
    });

    noBtn.addEventListener("click", () => {
      mainText.textContent = "Theek hai… main wait karunga. Jab tak tum ready nahi hoti, tab tak main apne aap ko aur better banaunga. 🌙";
      smallTitle.textContent = "Main yahi hoon…";
      normalButtons.classList.add("hidden");
      finalButtons.classList.add("hidden");
    });

    // Auto play attempt on first click/tap anywhere
    let musicTried = false;
    function tryPlayMusic() {
      if (musicTried) return;
      musicTried = true;
      bgMusic.play().catch(() => {
        // Agar browser block kare to user ko already note me bata diya gaya hai
      });
    }

    document.body.addEventListener("click", tryPlayMusic);
    document.body.addEventListener("touchstart", tryPlayMusic);

    // Floating hearts create
    const heartsContainer = document.getElementById("hearts");
    function createHeart() {
      const heart = document.createElement("div");
      heart.classList.add("heart");
      heart.textContent = "❤";

      const size = Math.random() * 16 + 12; // 12–28
      const left = Math.random() * 100; // 0–100vw
      const duration = Math.random() * 5 + 6; // 6–11s

      heart.style.left = left + "vw";
      heart.style.fontSize = size + "px";
      heart.style.animationDuration = duration + "s";

      heartsContainer.appendChild(heart);

      setTimeout(() => {
        heartsContainer.removeChild(heart);
      }, duration * 1000);
    }

    setInterval(createHeart, 800);

    // First slide render
    renderSlide();
  </script>
</body>
</html>
