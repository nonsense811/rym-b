<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Rym's Surprise</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #ffecec;
      color: #333;
      text-align: center;
      padding: 40px;
    }
    #passwordSection {
      margin-top: 30px;
    }
    input {
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 8px;
      outline: none;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 10px;
      background-color: #ff80aa;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    #countdown {
      margin-top: 20px;
      font-weight: bold;
      color: #ff4da6;
    }
  </style>
</head>
<body>

  <p style="font-size: 20px;"><strong>Enter the password or wait for the surprise 🎁</strong></p>

  <div id="passwordSection">
    <input type="password" id="passwordInput" placeholder="Enter password...">
    <br>
    <button onclick="checkPassword()">Unlock</button>
  </div>

  <p id="countdown"></p>

  <script>
    const correctPassword = "rym2025"; // change this to your actual password
    const unlockDate = new Date("2025-04-27T00:00:00"); // set the future unlock date

    function checkPassword() {
      const input = document.getElementById("passwordInput").value;
      if (input === correctPassword) {
        window.location.href = "surprise.html"; // redirect to surprise page
      } else {
        alert("Wrong password! Try again.");
      }
    }

    function updateCountdown() {
      const now = new Date();
      const diff = unlockDate - now;

      if (diff <= 0) {
        // Redirect automatically when time is up
        window.location.href = "surprise.html";
        return;
      }

      const days = Math.floor(diff / (1000 * 60 * 60 * 24));
      const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const minutes = Math.floor((diff / 1000 / 60) % 60);
      const seconds = Math.floor((diff / 1000) % 60);

      document.getElementById("countdown").textContent =
        `or wait: ${days}d ${hours}h ${minutes}m ${seconds}s`;
    }

    setInterval(updateCountdown, 1000);
    updateCountdown();
  </script>

</body>
</html>
