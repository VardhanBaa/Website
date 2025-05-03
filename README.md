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
    input[type="text"] {
      width: 100%;
      padding: 8px;
      border-radius: 8px;
      border: none;
      margin-bottom: 10px;
    }
    #dropArea {
      border: 2px dashed white;
      padding: 20px;
      border-radius: 12px;
      margin-top: 10px;
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
    <div id="dropArea">Drag & Drop Images Here</div>
    <button onclick="viewImages()">Uploaded Images Chudu Baa</button>
    <div id="imageList"></div>
  </div>

  <script>
    const correctPassword = "7/12";
    const imgbbApiKey = "26580a7906145ed4f4f8acbb9387fa0c";
    const jsonURL = "https://raw.githubusercontent.com/VardhanBaa/Website/main/images.json";

    function checkPassword() {
      const userPass = document.getElementById("password").value;
      if (userPass === correctPassword) {
        document.getElementById("loginBox").style.display = "none";
        document.getElementById("mainBox").style.display = "block";
      } else {
        alert("Wrong password Baa 😅");
      }
    }

    function uploadFile(file) {
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

    function uploadImage() {
      const file = document.getElementById("uploadInput").files[0];
      if (!file) return alert("Select an image Baa");
      uploadFile(file);
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
            const img = document.createElement("img");
            img.src = url;

            img.addEventListener("contextmenu", e => {
              e.preventDefault();
              if (confirm("🗑️ Delete this image from view?")) {
                img.remove();
              }
            });

            imageList.appendChild(img);
          });
        })
        .catch(() => {
          imageList.innerHTML = "Images Load avatledhu 😢";
        });
    }

    const dropArea = document.getElementById("dropArea");
    dropArea.addEventListener("dragover", e => {
      e.preventDefault();
      dropArea.style.background = "#ffffff22";
    });
    dropArea.addEventListener("dragleave", () => {
      dropArea.style.background = "transparent";
    });
    dropArea.addEventListener("drop", e => {
      e.preventDefault();
      dropArea.style.background = "transparent";
      const file = e.dataTransfer.files[0];
      if (file && file.type.startsWith("image/")) {
        uploadFile(file);
      } else {
        alert("Drag an image file only Baa!");
      }
    });
  </script>
</body>
</html>
