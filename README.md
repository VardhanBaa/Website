<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Secure Image Portal</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      height: 100vh;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #ff6ec4, #7873f5, #4ade80, #facc15);
      background-size: 400% 400%;
      animation: gradient 15s ease infinite;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
    }

    @keyframes gradient {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .container {
      background: rgba(0, 0, 0, 0.4);
      padding: 2rem;
      border-radius: 20px;
      text-align: center;
      width: 90%;
      max-width: 400px;
    }

    button, input[type="file"] {
      margin: 10px;
      padding: 10px 20px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      font-weight: bold;
    }

    button:hover {
      background-color: #ffffff22;
    }

    #imageList img {
      width: 100px;
      margin: 5px;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <div class="container" id="loginBox">
    <h2>Password Enter Chey Baa</h2>
    <input type="password" id="password" placeholder="Enter password" />
    <br>
    <button onclick="checkPassword()">Dhenki Waiting Elu Enjoy Chey!</button>
  </div>

  <div class="container" id="mainBox" style="display: none;">
    <h2>Hi Baaa Enjoy!</h2>
    <button onclick="document.getElementById('uploadInput').click()">Image Upload Chey Baa</button>
    <input type="file" id="uploadInput" accept="image/*" style="display: none;" onchange="uploadImage()" />
    <button onclick="viewImages()">Uploaded Images Chudu Baa</button>
    <div id="imageList"></div>
  </div>
  <script>
    const correctPassword = "7/12"; // change your password here
    let uploadedImages = [];

    function checkPassword() {
      const userPass = document.getElementById("password").value;
      if (userPass === correctPassword) {
        document.getElementById("loginBox").style.display = "none";
        document.getElementById("mainBox").style.display = "block";
      } else {
        alert("Wrong password Baa 😅");
      }
    }

    function uploadImage() {
      const fileInput = document.getElementById("uploadInput");
      const file = fileInput.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          uploadedImages.push(e.target.result);
          alert("Image uploaded ayindhi Baa 🚀");
        };
        reader.readAsDataURL(file);
      }
    }

    function viewImages() {
      const imageList = document.getElementById("imageList");
      imageList.innerHTML = "";
      uploadedImages.forEach(img => {
        const imageElement = document.createElement("img");
        imageElement.src = img;
        imageList.appendChild(imageElement);
      });
    }
  </script>
</body>
</html>
