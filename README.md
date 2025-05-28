# CV-generator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Live CV Generator</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 20px;
      min-height: 100vh;
      overflow: hidden;
      position: relative;
      background: linear-gradient(-45deg, #667eea, #764ba2, #6ee7b7, #f472b6);
      background-size: 400% 400%;
      animation: gradientMove 15s ease infinite;
      color: #333;
    }

    @keyframes gradientMove {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
      background-color: rgba(255, 255, 255, 0.1);
      border-radius: 20px;
      padding: 20px;
      backdrop-filter: blur(10px);
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
      position: relative;
      z-index: 2;
    }

    h1 {
      text-align: center;
      margin-bottom: 30px;
      color: #fff;
    }

    .input-section, .cv-preview {
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      width: 350px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }

    input, textarea, select, button {
      width: 100%;
      padding: 8px;
      margin-top: 10px;
      border-radius: 6px;
      border: 1px solid #ccc;
      font-size: 14px;
    }

    label {
      font-weight: bold;
    }

    .btn-group {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }

    .cv-preview {
      max-width: 600px;
      transition: all 0.3s ease;
    }

    #cvPhoto {
      width: 100px;
      height: 100px;
      object-fit: cover;
      border-radius: 50%;
      margin-bottom: 10px;
    }

    .template-light {
      background: #fff;
      color: #000;
    }

    .template-dark {
      background: #222;
      color: #f0f0f0;
    }

    .btn {
      background: #007bff;
      color: #fff;
      border: none;
      cursor: pointer;
    }

    .btn:hover {
      background: #0056b3;
    }

    @media print {
      body * {
        visibility: hidden;
      }
      .cv-preview, .cv-preview * {
        visibility: visible;
      }
      .cv-preview {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        margin: 0;
      }
    }

    /* Floating shapes */
    .floating-shapes {
      position: fixed;
      top: 0; left: 0;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
      z-index: 0;
    }

    .shape {
      position: absolute;
      width: 60px;
      height: 60px;
      background: rgba(255, 255, 255, 0.15);
      border-radius: 50%;
      animation: float 20s linear infinite;
      top: calc(100% + 60px);
      left: calc(var(--i) * 16%);
    }

    @keyframes float {
      0% {
        transform: translateY(0) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateY(-120vh) scale(0.5);
        opacity: 0;
      }
    }
  </style>
</head>
<body>

<h1>Live CV Generator</h1>

<div class="container">
  <!-- Input Section -->
  <div class="input-section">
    <label>Full Name</label>
    <input type="text" id="nameInput" placeholder="Your Name" oninput="updateCV()">

    <label>Email</label>
    <input type="email" id="emailInput" placeholder="example@mail.com" oninput="updateCV()">

    <label>Phone</label>
    <input type="text" id="phoneInput" placeholder="+91..." oninput="updateCV()">

    <label>Education</label>
    <textarea id="educationInput" placeholder="Your education details" oninput="updateCV()"></textarea>

    <label>Experience</label>
    <textarea id="experienceInput" placeholder="Your work experience" oninput="updateCV()"></textarea>

    <label>Skills</label>
    <textarea id="skillsInput" placeholder="Java, HTML, CSS, etc." oninput="updateCV()"></textarea>

    <label>Upload Photo</label>
    <input type="file" id="photoInput" accept="image/*" onchange="previewPhoto()">

    <label>Choose Template</label>
    <select id="templateSelect" onchange="changeTemplate()">
      <option value="template-light">Light</option>
      <option value="template-dark">Dark</option>
    </select>

    <div class="btn-group">
      <button class="btn" onclick="saveCV()">Save</button>
      <button class="btn" onclick="loadCV()">Load</button>
      <button class="btn" onclick="window.print()">Download</button>
    </div>
  </div>

  <!-- CV Preview Section -->
  <div class="cv-preview template-light" id="cvPreview">
    <div class="cv-section">
      <img id="cvPhoto" src="" alt="Photo Preview">
      <h2 id="cvName">Your Name</h2>
      <p id="cvEmail">Email: example@mail.com</p>
      <p id="cvPhone">Phone: +91...</p>
    </div>
    <div class="cv-section">
      <h3>Education</h3>
      <p id="cvEducation">Your education details</p>
    </div>
    <div class="cv-section">
      <h3>Experience</h3>
      <p id="cvExperience">Your work experience</p>
    </div>
    <div class="cv-section">
      <h3>Skills</h3>
      <p id="cvSkills">Java, HTML, CSS, etc.</p>
    </div>
  </div>
</div>

<!-- Floating Shapes -->
<div class="floating-shapes">
  <div class="shape" style="--i:0;"></div>
  <div class="shape" style="--i:1;"></div>
  <div class="shape" style="--i:2;"></div>
  <div class="shape" style="--i:3;"></div>
  <div class="shape" style="--i:4;"></div>
  <div class="shape" style="--i:5;"></div>
</div>

<script>
  function updateCV() {
    document.getElementById("cvName").textContent = 
      document.getElementById("nameInput").value || "Your Name";
    document.getElementById("cvEmail").textContent = "Email: " + 
      (document.getElementById("emailInput").value || "example@mail.com");
    document.getElementById("cvPhone").textContent = "Phone: " + 
      (document.getElementById("phoneInput").value || "+91...");
    document.getElementById("cvEducation").textContent = 
      document.getElementById("educationInput").value || "Your education details";
    document.getElementById("cvExperience").textContent = 
      document.getElementById("experienceInput").value || "Your work experience";
    document.getElementById("cvSkills").textContent = 
      document.getElementById("skillsInput").value || "Java, HTML, CSS, etc.";
  }

  function previewPhoto() {
    const file = document.getElementById("photoInput").files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function(e) {
        document.getElementById("cvPhoto").src = e.target.result;
      };
      reader.readAsDataURL(file);
    }
  }

  function changeTemplate() {
    const selected = document.getElementById("templateSelect").value;
    const preview = document.getElementById("cvPreview");
    preview.className = "cv-preview " + selected;
  }

  function saveCV() {
    const data = {
      name: document.getElementById("nameInput").value,
      email: document.getElementById("emailInput").value,
      phone: document.getElementById("phoneInput").value,
      education: document.getElementById("educationInput").value,
      experience: document.getElementById("experienceInput").value,
      skills: document.getElementById("skillsInput").value,
      photo: document.getElementById("cvPhoto").src,
      template: document.getElementById("templateSelect").value
    };
    localStorage.setItem("cvData", JSON.stringify(data));
    alert("CV saved successfully!");
  }

  function loadCV() {
    const data = JSON.parse(localStorage.getItem("cvData"));
    if (data) {
      document.getElementById("nameInput").value = data.name;
      document.getElementById("emailInput").value = data.email;
      document.getElementById("phoneInput").value = data.phone;
      document.getElementById("educationInput").value = data.education;
      document.getElementById("experienceInput").value = data.experience;
      document.getElementById("skillsInput").value = data.skills;
      document.getElementById("templateSelect").value = data.template;
      document.getElementById("cvPhoto").src = data.photo;

      updateCV();
      changeTemplate();
    } else {
      alert("No saved CV found.");
    }
  }

  // Initialize on load
  updateCV();
</script>

</body>
</html>
