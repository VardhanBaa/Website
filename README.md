<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Secur</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
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
      overflow: auto;
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
      max-width: 600px;
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
    #imageList {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 15px;
      margin-top: 1rem;
    }
    .image-frame {
      background: white;
      padding: 8px;
      border-radius: 16px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
      transition: transform 0.3s, box-shadow 0.3s;
    }
    .image-frame:hover {
      transform: scale(1.08);
      box-shadow: 0 6px 16px rgba(0,0,0,0.5);
    }
    .image-frame img {
      width: 120px;
      height: 120px;
      object-fit: cover;
      border-radius: 10px;
    }
    input[type="text"] {
      width: 100%;
      padding: 8px;
      border-radius: 8px;
      border: none;
      margin-bottom: 10px;
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
    const correctPassword = "7/12";
    const imgbbApiKey = "26580a7906145ed4f4f8acbb9387fa0c"; // ✅ Your API Key
    const jsonURL = "https://raw.githubusercontent.com/VardhanBaa/Website/main/images.json"; // ✅ Replace with your raw JSON URL if different

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
      const file = document.getElementById("uploadInput").files[0];
      if (!file) return alert("Select an image Baa");

      const reader = new FileReader();
      reader.onload = async function (e) {
        const base64Image = e.target.result.split(",")[1];

        const formData = new FormData();
        formData.append("key", imgbbApiKey);
        formData.append("image", base64Image);

        const res = await fetch("https://api.imgbb.com/1/upload", {
          method: "POST",
          body: formData
        });

        const result = await res.json();
        const imageUrl = result.data.url;

        // Show URL + copy button
        const imageList = document.getElementById("imageList");
        imageList.innerHTML = `
          <p>✅ Image uploaded successfully!</p>
          <input type="text" value="${imageUrl}" id="uploadedUrl" readonly />
          <button onclick="copyToClipboard()">Copy Image URL</button>
          <p>🛠️ Now go to <code>images.json</code> on GitHub and paste this link inside the array.</p>
        `;
      };

      reader.readAsDataURL(file);
    }

    function copyToClipboard() {
      const urlInput = document.getElementById("uploadedUrl");
      urlInput.select();
      document.execCommand("copy");
      alert("📋 Copied to clipboard!");
    }

    function viewImages() {
      const imageList = document.getElementById("imageList");
      imageList.innerHTML = "Loading images...";

      fetch(jsonURL)
        .then(res => res.json())
        .then(images => {
          imageList.innerHTML = "";
          images.forEach(url => {
            const frame = document.createElement("div");
            frame.className = "image-frame";

            const img = document.createElement("img");
            img.src = url;

            frame.appendChild(img);
            imageList.appendChild(frame);
          });
        })
        .catch(() => {
          imageList.innerHTML = "Images Load avatledhu 😢";
        });
    }
  </script>
</body>
</html>
