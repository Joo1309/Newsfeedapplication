<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Neon News Dashboard</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap" rel="stylesheet" />
  <style>
    body {
      margin: 0;
      padding: 0;
      background: radial-gradient(circle at top left, #0d1117 0%, #000 80%);
      font-family: 'Poppins', sans-serif;
      color: #eaf6ff;
      display: flex;
      min-height: 100vh;
      overflow-x: hidden;
    }

    .sidebar {
      width: 200px;
      background: rgba(0, 20, 40, 0.95);
      border-right: 2px solid #00e0ff33;
      backdrop-filter: blur(12px);
      padding: 30px 0;
      box-shadow: 2px 0 10px #00e0ff33;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
    }

    .sidebar h2 {
      color: #00e0ff;
      font-size: 1.5rem;
      font-weight: 800;
      text-align: left;
      margin-left: 20px;
      margin-bottom: 25px;
      text-shadow: 0 0 15px #00e0ff99;
    }

    .sidebar nav a {
      display: block;
      padding: 12px 25px;
      color: #bcdfff;
      text-decoration: none;
      font-weight: 600;
      border-left: 3px solid transparent;
      transition: 0.3s;
    }

    .sidebar nav a:hover, .sidebar nav a.active {
      color: #00e0ff;
      border-left: 3px solid #00e0ff;
      background: rgba(0, 224, 255, 0.08);
      box-shadow: inset 0 0 10px #00e0ff33;
    }

    .main {
      flex: 1;
      padding: 50px 25px;
      max-width: 1100px;
      margin: 0 auto;
    }

    .title {
      font-size: 2.3rem;
      font-weight: 800;
      color: #00e0ff;
      text-align: center;
      letter-spacing: 2px;
      text-shadow: 0 0 20px #00e0ff66;
      margin-bottom: 10px;
    }

    .subtitle {
      font-size: 1.1rem;
      text-align: center;
      color: #8cd9ff;
      margin-bottom: 40px;
    }

    .cards {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
    }

    .card {
      background: rgba(10, 30, 50, 0.8);
      border: 1px solid #00e0ff33;
      border-radius: 15px;
      width: 260px;
      padding: 20px;
      text-align: left;
      transition: 0.3s;
      position: relative;
      overflow: hidden;
    }

    .card::before {
      content: "";
      position: absolute;
      top: -50%;
      left: -50%;
      width: 200%;
      height: 200%;
      background: conic-gradient(from 180deg, transparent, #00e0ff33, transparent 30%);
      animation: rotate 6s linear infinite;
      z-index: 0;
    }

    .card-content {
      position: relative;
      z-index: 1;
    }

    @keyframes rotate {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .card:hover {
      transform: translateY(-7px);
      box-shadow: 0 0 20px #00e0ff44;
    }

    .card-title {
      font-weight: 700;
      font-size: 1.2rem;
      color: #00e0ff;
      margin-bottom: 10px;
    }

    .card-desc {
      font-size: 0.95rem;
      color: #c6eaff;
      margin-bottom: 15px;
    }

    .card-btn {
      background: #00e0ff;
      color: #00101a;
      font-weight: 700;
      padding: 8px 15px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    .card-btn:hover {
      background: #02c5e6;
    }

    #details-section {
      display: none;
      margin-top: 25px;
      padding: 25px;
      background: rgba(0, 25, 45, 0.85);
      border: 1px solid #00e0ff33;
      border-radius: 12px;
      box-shadow: 0 0 25px #00e0ff22;
    }

    #details-section.active {
      display: block;
    }

    #details-section h3 {
      color: #00e0ff;
      margin-bottom: 10px;
    }

    .loading {
      color: #00e0ff;
      font-size: 1.1rem;
    }
  </style>
</head>
<body>
  <div class="sidebar">
    <h2>NewsPulse</h2>
    <nav>
      <a href="#" class="active" onclick="showSection('home', this)">Home</a>
      <a href="#" onclick="showSection('trending', this)">Trending</a>
      <a href="#" onclick="showSection('world', this)">World</a>
      <a href="#" onclick="showSection('tech', this)">Tech</a>
      <a href="#" onclick="showSection('sports', this)">Sports</a>
      <a href="#" onclick="showSection('business', this)">Business</a>
    </nav>
  </div>

  <div class="main">
    <div class="title">Neon News Feed</div>
    <div class="subtitle">Live • Trending • Fast • Smart</div>

    <div class="cards">
      <div class="card">
        <div class="card-content">
          <div class="card-title">Tech Breakthrough</div>
          <div class="card-desc">AI surpasses human-level performance in real-time translation across 50+ languages.</div>
          <button class="card-btn" onclick="loadDetails('tech')">Read More</button>
        </div>
      </div>

      <div class="card">
        <div class="card-content">
          <div class="card-title">Global Economy</div>
          <div class="card-desc">Markets surge as inflation slows, and investors bet big on digital transformation sectors.</div>
          <button class="card-btn" onclick="loadDetails('business')">Read More</button>
        </div>
      </div>

      <div class="card">
        <div class="card-content">
          <div class="card-title">Sports Spotlight</div>
          <div class="card-desc">Historic win for underdogs in the championship match sparks global excitement.</div>
          <button class="card-btn" onclick="loadDetails('sports')">Read More</button>
        </div>
      </div>
    </div>

    <div id="details-section"></div>
  </div>

  <script>
    function setActiveSidebar(el) {
      const links = document.querySelectorAll('.sidebar nav a');
      links.forEach(link => link.classList.remove('active'));
      el.classList.add('active');
    }

    function showSection(section, el) {
      setActiveSidebar(el);
      if (section === 'home') {
        document.getElementById('details-section').style.display = 'none';
      } else {
        loadDetails(section);
      }
    }

    async function loadDetails(section) {
      const detailsDiv = document.getElementById("details-section");
      detailsDiv.className = "active";
      detailsDiv.innerHTML = '<span class="loading">Fetching latest updates...</span>';

      setTimeout(() => {
        detailsDiv.innerHTML = `
          <h3>${section.toUpperCase()} HEADLINES</h3>
          <p>• Exclusive insights, stories, and live updates on ${section} news.<br>
          • Stay tuned with real-time data and interactive features.</p>
        `;
      }, 1000);
    }
  </script>
</body>
</html>

