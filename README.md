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
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 15px;
      margin-top: 1rem;
      max-height: 400px;
      overflow-y: auto;
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
    .image-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 10;
    }
    .image-modal img {
      max-width: 90%;
      max-height: 90%;
      border-radius: 10px;
    }
    .image-modal .modal-content {
      position: relative;
    }
    .image-modal .close-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 24px;
      color: white;
      background: rgba(0, 0, 0, 0.6);
      padding: 10px;
      cursor: pointer;
      border-radius: 50%;
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

  <div class="image-modal" id="imageModal">
    <div class="modal-content">
      <img id="fullImage" src="" alt="Full Image" />
      <button class="close-btn" onclick="closeImage()">Back</button>
    </div>
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
            frame.onclick = () => showOptions(url);

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

    function showOptions(url) {
      const options = document.createElement("div");
      options.style.position = "fixed";
      options.style.top = "50%";
      options.style.left = "50%";
      options.style.transform = "translate(-50%, -50%)";
      options.style.background = "#222";
      options.style.padding = "20px";
      options.style.borderRadius = "15px";
      options.style.zIndex = 1000;
      options.innerHTML = `
        <p style="margin-bottom: 10px;">Choose an option:</p>
        <button onclick="openFullImage('${url}')">Open Full Image</button>
        <button onclick="requestDelete('${url}')">Request Delete</button>
        <br><br>
        <button onclick="this.parentNode.remove()">Cancel</button>
      `;
      document.body.appendChild(options);
    }

    function openFullImage(url) {
      const modal = document.getElementById("imageModal");
      const fullImage = document.getElementById("fullImage");
      fullImage.src = url;
      modal.style.display = "flex";
    }

    function closeImage() {
      document.getElementById("imageModal").style.display = "none";
    }

    function requestDelete(url) {
      const subject = "Delete Image Request";
      const body = `Please delete the following image: ${url}`;
      const mailto = `mailto:chatgptshorsbytinesh@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.location.href = mailto;
    }
  </script>
</body>
</html>
