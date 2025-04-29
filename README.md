<!DOCTYPE html>
<html>
<head>
  <title>TechPath - Student Guide</title>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" />

  <!-- Firebase Compat SDKs (for Firebase v9+ but old-style syntax) -->
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>

  <!-- Your CSS -->
  <link rel="stylesheet" href="style.css" />

  <!-- Your custom JS (with defer) -->
  <script src="app.js" defer></script>
</head>
<body class="bg-light">
  <div class="container mt-5">
    <h2 class="text-center">TechPath - Student Roadmap App</h2>

    <!-- Login Section -->
    <div id="login-section">
      <input type="email" id="email" class="form-control my-2" placeholder="Email" />
      <input type="password" id="password" class="form-control my-2" placeholder="Password" />
      <button onclick="login()" class="btn btn-primary">Login</button>
      <button onclick="signup()" class="btn btn-secondary">Sign Up</button>
    </div>

    <!-- Dashboard Section -->
    <div id="dashboard-section" style="display: none;">
      <h4 class="mt-4">Welcome to TechPath Dashboard</h4>
      <p>Select your career interest:</p>
      <select id="domain-select" class="form-select" onchange="selectDomain(this.value)">
        <option selected disabled>Select Domain</option>
        <option>Web Development</option>
        <option>Cybersecurity</option>
        <option>Game Development</option>
        <option>AI/ML</option>
      </select>
    </div>

    <!-- Roadmap Display -->
    <div id="roadmap-section" style="display: none;">
      <h5 class="mt-4" id="domain-title"></h5>
      <ul class="list-group" id="roadmap-content"></ul>
      <button onclick="goBack()" class="btn btn-warning mt-3">Back</button>
    </div>
  </div>

  <script>
    // Your Firebase Config Here (Replace with your project credentials)
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      appId: "YOUR_APP_ID"
    };

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    function login() {
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;
      auth.signInWithEmailAndPassword(email, password)
        .then(() => {
          document.getElementById("login-section").style.display = "none";
          document.getElementById("dashboard-section").style.display = "block";
        })
        .catch(err => alert(err.message));
    }

    function signup() {
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;
      auth.createUserWithEmailAndPassword(email, password)
        .then(() => alert("Account created! Please login."))
        .catch(err => alert(err.message));
    }

    function selectDomain(domain) {
      const roadmaps = {
        "Web Development": [
          "Learn HTML, CSS, and JavaScript",
          "Understand how the web works (DNS, Hosting, etc.)",
          "Learn a front-end framework (React, Vue, etc.)",
          "Explore backend (Node.js, Express) and databases (MongoDB, Firebase)",
          "Build real projects and host them on GitHub",
          "Watch YouTube tutorials (Traversy Media, CodeWithHarry)",
          "Ask questions in Q&A forums like Stack Overflow or TechPath Discord",
          "Consider free or paid courses (Coursera, Udemy)",
          "Connect with mentors via LinkedIn or mentorship platforms (ADPList, MentorCruise)",
          "Apply to companies like Infosys, TCS, Accenture, Zoho (Entry salary: ₹3.5L – ₹6L/year)",
          "Internship portals: Internshala, LetsIntern, LinkedIn Internships",
          "Join TechPath mentor chat to talk with seniors or alumni"
        ],
        "Cybersecurity": [
          "Understand computer networks, Linux, and basic scripting (Python/Bash)",
          "Learn ethical hacking fundamentals (OWASP, Nmap, Wireshark)",
          "Explore platforms like TryHackMe, Hack The Box, and CTFs",
          "Study tools like Metasploit, Burp Suite, and Wireshark",
          "Watch cybersecurity content (John Hammond, NetworkChuck)",
          "Join security communities (Reddit, Discord, etc.)",
          "Consider certifications (CEH, CompTIA Security+)",
          "Explore bug bounty platforms (HackerOne, Bugcrowd)",
          "Seek mentorship from ethical hackers or professionals in forums",
          "Companies hiring: Deloitte, PwC, Cisco, Palo Alto (₹5L – ₹10L/year)",
          "Internship portals: HackerOne Learning Hub, Internshala, LinkedIn",
          "Join TechPath mentor chat to get advice from cyber professionals"
        ],
        "Game Development": [
          "Learn a programming language (C#, C++, or Python)",
          "Start with engines like Unity or Godot",
          "Build simple 2D games, then advance to 3D",
          "Understand physics, animation, and game design",
          "Follow YouTube tutorials (Brackeys, GameDevTV)",
          "Contribute to game jams or open-source games",
          "Connect with indie dev mentors via Discord/LinkedIn",
          "Companies hiring: Ubisoft, Zynga, EA, small indie studios (₹4L – ₹8L/year)",
          "Internship portals: Gamedev.net, Unity Connect, LinkedIn",
          "Join TechPath mentor chat to collaborate with devs"
        ],
        "AI/ML": [
          "Learn Python and libraries (NumPy, Pandas, Matplotlib)",
          "Understand basic math: linear algebra, probability, statistics",
          "Learn ML libraries: Scikit-Learn, TensorFlow, PyTorch",
          "Practice with datasets (Kaggle, UCI)",
          "Take MOOCs (Coursera, fast.ai, deeplearning.ai)",
          "Explore deep learning, NLP, and computer vision",
          "Join AI communities (Kaggle, HuggingFace, Discord)",
          "Find mentorship via AI communities and conferences",
          "Companies hiring: Google, Microsoft, Amazon, TCS AI Labs (₹6L – ₹15L/year)",
          "Internship portals: Analytics Vidhya, Internshala, Kaggle Jobs",
          "Join TechPath mentor chat to ask project or career questions"
        ]
      };

      document.getElementById("dashboard-section").style.display = "none";
      document.getElementById("roadmap-section").style.display = "block";
      document.getElementById("domain-title").innerText = domain + " Roadmap";

      const roadmapList = document.getElementById("roadmap-content");
      roadmapList.innerHTML = "";
      roadmaps[domain].forEach(item => {
        const li = document.createElement("li");
        li.className = "list-group-item";
        li.textContent = item;
        roadmapList.appendChild(li);
      });
    }

    function goBack() {
      document.getElementById("roadmap-section").style.display = "none";
      document.getElementById("dashboard-section").style.display = "block";
    }
  </script>
</body>
</html>
