<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>🎂 Happy Birthday!</title>

    <!-- Bootstrap -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
    />
    <!-- SweetAlert2 -->
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <!-- Google Fonts -->
    <link
      href="https://fonts.googleapis.com/css2?family=Pacifico&family=Poppins:wght@400;600&display=swap"
      rel="stylesheet"
    />

    <style>
      body {
        background: linear-gradient(135deg, #ffe5ec, #b8fff9);
        font-family: "Poppins", sans-serif;
        overflow: hidden;
        min-height: 100vh;
        text-align: center;
      }

      #button {
        background: linear-gradient(45deg, #ff8fb1, #ffb3c6);
        border: none;
        box-shadow: 0 4px 20px rgba(255, 0, 100, 0.3);
        transition: 0.4s;
        font-size: 1.3rem;
      }

      #button:hover {
        transform: scale(1.1);
        box-shadow: 0 6px 25px rgba(255, 0, 150, 0.4);
      }

      h2 {
        font-family: "Pacifico", cursive;
        color: #ff4fa7;
        animation: fadeIn 1.2s ease-in-out;
      }

      #photoPreview {
        display: none;
        width: 250px;
        height: 250px;
        border-radius: 50%;
        object-fit: cover;
        margin: 20px auto;
        box-shadow: 0 0 25px rgba(255, 0, 150, 0.4);
        animation: fadeInUp 1.2s ease;
      }

      #iloveyou {
        display: none;
        animation: fadeIn 1.5s ease-in-out;
      }

      @keyframes fadeIn {
        from {
          opacity: 0;
        }
        to {
          opacity: 1;
        }
      }

      @keyframes fadeInUp {
        from {
          opacity: 0;
          transform: translateY(30px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }

      #hati {
        color: #ff4fa7;
        font-weight: 600;
        letter-spacing: 1px;
        cursor: pointer;
        transition: 0.3s;
      }

      #hati:hover {
        color: #ff85b3;
      }

      canvas.confetti {
        position: fixed;
        top: 0;
        left: 0;
        pointer-events: none;
      }
    </style>
  </head>

  <body>
    <div class="position-absolute top-50 start-50 translate-middle text-center">
      <button
        id="button"
        class="btn btn-lg text-white px-4 py-3"
        onclick="mulai();music()"
      >
        🎉 Click Here! 🎉
      </button>

      <div id="iloveyou">
        <img id="photoPreview" alt="Foto Anda" />
        <h2 id="birthdayMessage">Happy Birthday <span id="birthdayName"></span> 🎂</h2>
        <h2>🎈💖🎉</h2>
      </div>
    </div>

    <span
      id="hati"
      class="fixed-bottom text-center mb-3"
      onclick="hati()"
    ></span>

    <audio id="bgMusic" src="music/selamatulangtahun.mp3" loop></audio>

    <script>
      const author = "orang Special 💗";
      document.getElementById("hati").innerHTML = `Made with 💕 by ${author}`;

      const swals = Swal.mixin({
        cancelButtonColor: "#ff7aa2",
        confirmButtonColor: "#ff4fa7",
        background: "#fff6fb",
      });

      let audioPlaying = false;
      let userPhoto = null;

      function stopMusic() {
        const audio = document.getElementById("bgMusic");
        audio.pause();
        audioPlaying = false;
      }

      async function mulai() {
        await swals.fire("Haiii!", "Hari ini hari spesial kamu 🎉", "info");
        await swals.fire("Aku mau ngucapin sesuatu buat kamu 💕");

        const { value: nama } = await swals.fire({
          title: "Tulis nama kamu dulu ya 💖",
          input: "text",
          inputPlaceholder: "Namamu...",
        });

        if (!nama) {
          stopMusic();
          return;
        }

        document.getElementById("birthdayName").textContent = nama;

        await swals.fire("Cieee tambah umur~ makin keren aja nih 😍");

        const { value: umur } = await swals.fire({
          title: "Umur kamu sekarang berapa nih? 🎂",
          icon: "question",
          input: "range",
          inputAttributes: { min: 1, max: 100 },
          inputValue: 18,
        });

        await swals.fire(
          `Selamat ulang tahun ${nama}! Di umur ${umur}, semoga kamu selalu bahagia, sehat, dan sukses selalu 💫`
        );

        await swals.fire(
          "Dan semoga semua keinginanmu tahun ini bisa tercapai yaa amin 🌟"
        );

        // Pilihan upload foto
        const pilihFoto = await swals.fire({
          title: "Mau tambah foto kamu di tengah layar biar estetik? 📷",
          showCancelButton: true,
          confirmButtonText: "Pilih Foto Kamu Ya Format Kotak",
          cancelButtonText: "Lanjut Tanpa Foto ;(",
        });

        if (pilihFoto.isConfirmed) {
          // Buat input file manual
          const fileInput = document.createElement("input");
          fileInput.type = "file";
          fileInput.accept = "image/*";
          fileInput.click();

          fileInput.onchange = (e) => {
            const file = e.target.files[0];
            if (file) {
              const reader = new FileReader();
              reader.onload = (event) => {
                userPhoto = event.target.result;
              };
              reader.readAsDataURL(file);
            }
          };
        }

        await swals.fire("Sekarang klik ikon hati di bawah ya 💖");
      }

      function music() {
        const audio = document.getElementById("bgMusic");
        if (!audioPlaying) audio.play();
        else audio.pause();
        audioPlaying = !audioPlaying;
      }

      function hati() {
        document.getElementById("button").style.display = "none";
        document.getElementById("iloveyou").style.display = "block";
        if (userPhoto) {
          const img = document.getElementById("photoPreview");
          img.src = userPhoto;
          img.style.display = "block";
        }
        startConfetti();
      }

      // Confetti animasi
      function startConfetti() {
        const canvas = document.createElement("canvas");
        canvas.classList.add("confetti");
        document.body.appendChild(canvas);
        const ctx = canvas.getContext("2d");
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const confetti = [];
        for (let i = 0; i < 200; i++) {
          confetti.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height - canvas.height,
            r: Math.random() * 6 + 2,
            d: Math.random() * 0.5 + 0.5,
            color: `hsl(${Math.random() * 360}, 100%, 70%)`,
          });
        }

        function draw() {
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          confetti.forEach((p) => {
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
            ctx.fillStyle = p.color;
            ctx.fill();
          });
          update();
        }

        function update() {
          confetti.forEach((p) => {
            p.y += p.d * 3;
            p.x += Math.sin(p.y * 0.02);
            if (p.y > canvas.height) {
              p.y = 0;
              p.x = Math.random() * canvas.width;
            }
          });
        }

        function loop() {
          draw();
          requestAnimationFrame(loop);
        }

        loop();
      }
    </script>
  </body>
</html>
