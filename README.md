# AIChef-Recipe-Explorer
An interactive web application for AI-powered recipe discovery, cooking guidance, and meal planning with real-time ingredient management.
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GenAI Odyssey - 生成式AI 學習航線</title>
  <style>
    :root {
      --bg-1: #07111f;
      --bg-2: #0d1b2a;
      --panel: rgba(13, 24, 38, 0.82);
      --panel-strong: rgba(17, 31, 48, 0.95);
      --line: rgba(158, 177, 255, 0.22);
      --text: #edf5ff;
      --muted: #9eb6d3;
      --cyan: #73e7ff;
      --blue: #7aa2ff;
      --purple: #a88cff;
      --pink: #ff8dc7;
      --green: #73f7c7;
      --yellow: #ffd86b;
      --shadow: rgba(0,0,0,0.4);
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      font-family: "Segoe UI", "Microsoft JhengHei", sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at 20% 20%, rgba(122, 162, 255, 0.18), transparent 24%),
        radial-gradient(circle at 80% 10%, rgba(168, 140, 255, 0.18), transparent 22%),
        radial-gradient(circle at 50% 80%, rgba(115, 231, 255, 0.12), transparent 35%),
        linear-gradient(180deg, var(--bg-1), var(--bg-2));
      min-height: 100vh;
    }

    a { color: inherit; text-decoration: none; }
    .container {
      width: min(1200px, calc(100% - 32px));
      margin: 0 auto;
    }

    header {
      position: sticky;
      top: 0;
      backdrop-filter: blur(12px);
      background: rgba(7, 17, 31, 0.68);
      border-bottom: 1px solid var(--line);
      z-index: 50;
    }

    .nav {
      display: flex;
      align-items: center;
      justify-content: space-between;
      min-height: 72px;
      gap: 18px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      font-weight: 800;
      letter-spacing: 0.08em;
      text-transform: uppercase;
    }

    .brand-mark {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--cyan), var(--purple));
      box-shadow: 0 0 22px rgba(115, 231, 255, 0.8);
    }

    .nav-links {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;
    }

    .nav-links a {
      color: var(--muted);
      font-size: 0.95rem;
      transition: 0.2s ease;
    }
    .nav-links a:hover { color: var(--text); }

    .hero {
      padding: 68px 0 20px;
    }

    .hero-grid {
      display: grid;
      grid-template-columns: 1.4fr 1fr;
      gap: 28px;
      align-items: center;
    }

    .eyebrow {
      display: inline-block;
      padding: 8px 12px;
      border: 1px solid var(--line);
      border-radius: 999px;
      background: rgba(122, 162, 255, 0.08);
      color: var(--cyan);
      letter-spacing: 0.08em;
      text-transform: uppercase;
      font-size: 0.72rem;
      font-weight: 700;
    }

    h1 {
      font-size: clamp(2.5rem, 5vw, 5rem);
      line-height: 1.05;
      letter-spacing: -0.06em;
      margin: 18px 0 16px;
    }

    .gradient {
      background: linear-gradient(90deg, var(--cyan), var(--blue), var(--purple), var(--pink));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .hero p {
      font-size: 1.08rem;
      color: var(--muted);
      max-width: 62ch;
      line-height: 1.7;
      margin: 0;
    }

    .hero-actions {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      margin-top: 26px;
    }

    .button {
      border: 1px solid rgba(255,255,255,0.1);
      color: var(--text);
      background: rgba(255,255,255,0.04);
      padding: 12px 18px;
      border-radius: 12px;
      cursor: pointer;
      transition: 0.2s ease;
      font-weight: 700;
      letter-spacing: 0.02em;
    }

    .button:hover {
      transform: translateY(-1px);
      border-color: rgba(255,255,255,0.2);
    }

    .button.primary {
      background: linear-gradient(135deg, rgba(115,231,255,0.23), rgba(168,140,255,0.22));
      border-color: var(--line);
      box-shadow: 0 12px 28px rgba(122,162,255,0.18);
    }

    .stats {
      display: grid;
      grid-template-columns: repeat(3, minmax(0,1fr));
      gap: 14px;
      margin-top: 28px;
    }

    .stat {
      background: rgba(255,255,255,0.03);
      border: 1px solid var(--line);
      border-radius: 16px;
      padding: 18px 16px;
      box-shadow: var(--shadow);
    }

    .stat strong {
      display: block;
      font-size: 1.7rem;
      margin-top: 6px;
      letter-spacing: -0.05em;
    }

    .stat span {
      color: var(--muted);
      font-size: 0.8rem;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .orbit {
      position: relative;
      height: 360px;
      background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
      border: 1px solid var(--line);
      border-radius: 24px;
      overflow: hidden;
      box-shadow: 0 20px 40px rgba(0,0,0,0.3);
    }

    .orbit::before {
      content: "";
      position: absolute;
      inset: 18px;
      border-radius: 50%;
      border: 1px solid rgba(255,255,255,0.08);
      background:
        radial-gradient(circle at center, rgba(115,231,255,0.12), rgba(168,140,255,0.08), transparent 60%);
    }

    .core {
      position: absolute;
      inset: 50% auto auto 50%;
      transform: translate(-50%, -50%);
      width: 90px;
      height: 90px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--cyan), var(--purple));
      box-shadow: 0 0 40px rgba(115,231,255,0.8);
      display: grid;
      place-items: center;
      font-size: 1.5rem;
      font-weight: 800;
    }

    .planet {
      position: absolute;
      left: 50%;
      top: 50%;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      border: 1px solid rgba(255,255,255,0.25);
      background: var(--planet-color, #73f7c7);
      box-shadow: 0 0 18px var(--planet-color, #73f7c7);
      animation: orbit 12s linear infinite;
      transform-origin: center;
    }

    .planet:nth-child(2) { animation-delay: 0.2s; }
    .planet:nth-child(3) { animation-delay: 1.2s; }
    .planet:nth-child(4) { animation-delay: 2.1s; }
    .planet:nth-child(5) { animation-delay: 3.1s; }

    @keyframes orbit {
      from { transform: rotate(0deg) translateX(110px) rotate(0deg); }
      to { transform: rotate(360deg) translateX(110px) rotate(360deg); }
    }

    .section {
      padding: 28px 0;
    }

    .section-head {
      display: flex;
      justify-content: space-between;
      align-items: end;
      gap: 12px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .section-head h2 {
      margin: 0;
      font-size: clamp(1.5rem, 3vw, 2.4rem);
      letter-spacing: -0.05em;
    }

    .section-head p {
      margin: 0;
      color: var(--muted);
    }

    .paths {
      display: grid;
      grid-template-columns: repeat(3, minmax(0,1fr));
      gap: 16px;
    }

    .path-card {
      background: rgba(255,255,255,0.03);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 18px 16px;
      cursor: pointer;
      transition: 0.2s ease;
      min-height: 200px;
    }

    .path-card.active {
      border-color: rgba(115,231,255,0.52);
      box-shadow: 0 0 0 1px rgba(115,231,255,0.25), 0 12px 26px rgba(10, 20, 42, 0.7);
      background: linear-gradient(180deg, rgba(115,231,255,0.09), rgba(255,255,255,0.02));
    }

    .path-card h3 {
      margin: 0 0 8px;
      font-size: 1.25rem;
    }

    .path-card p {
      margin: 0;
      color: var(--muted);
      line-height: 1.7;
    }

    .path-meta {
      margin-top: 18px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      color: var(--muted);
      font-size: 0.8rem;
    }

    .progress {
      width: 100%;
      height: 10px;
      background: rgba(255,255,255,0.06);
      border-radius: 999px;
      overflow: hidden;
      margin-top: 18px;
    }

    .progress-bar {
      width: 0%;
      height: 100%;
      background: linear-gradient(90deg, var(--cyan), var(--purple));
      border-radius: inherit;
      transition: width 0.35s ease;
    }

    .mission-panel {
      display: grid;
      grid-template-columns: 1.3fr 1fr;
      gap: 20px;
      margin-top: 24px;
    }

    .mission-box, .ai-box {
      background: rgba(255,255,255,0.03);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 22px;
      box-shadow: var(--shadow);
    }

    .mission-box h3, .ai-box h3 {
      margin: 0 0 12px;
      font-size: 1.1rem;
    }

    .mission-list {
      display: grid;
      gap: 12px;
      margin-top: 18px;
    }

    .mission-item {
      display: flex;
      gap: 12px;
      padding: 12px 14px;
      background: rgba(255,255,255,0.02);
      border: 1px solid rgba(255,255,255,0.04);
      border-radius: 12px;
      color: var(--muted);
    }

    .mission-item .dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--cyan), var(--purple));
      margin-top: 7px;
      flex-shrink: 0;
    }

    .ai-box p {
      margin: 0;
      color: var(--muted);
      line-height: 1.7;
    }

    .mentor-bubble {
      margin-top: 20px;
      padding: 16px 18px;
      border-radius: 14px;
      background: rgba(115,231,255,0.08);
      border: 1px solid rgba(115,231,255,0.18);
      color: var(--text);
      min-height: 108px;
      line-height: 1.7;
    }

    .timeline {
      display: grid;
      grid-template-columns: repeat(3, minmax(0,1fr));
      gap: 18px;
      margin-top: 10px;
    }

    .timeline-card {
      background: rgba(255,255,255,0.03);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 20px;
      position: relative;
      overflow: hidden;
    }

    .timeline-card::before {
      content: "";
      position: absolute;
      inset: 0 auto auto 0;
      width: 100%;
      height: 4px;
      background: linear-gradient(90deg, var(--cyan), var(--purple));
    }

    .timeline-card .phase {
      color: var(--cyan);
      font-size: 0.75rem;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      margin-bottom: 14px;
      font-weight: 700;
    }

    .timeline-card h3 {
      margin: 0 0 10px;
      font-size: 1.15rem;
    }

    .timeline-card p {
      margin: 0;
      color: var(--muted);
      line-height: 1.7;
    }

    .cta {
      display: flex;
      justify-content: center;
      margin-top: 24px;
    }

    .footer {
      padding: 28px 0 42px;
      color: var(--muted);
      text-align: center;
      font-size: 0.9rem;
    }

    @media (max-width: 780px) {
      .hero-grid, .mission-panel, .timeline, .paths {
        grid-template-columns: 1fr;
      }
      .nav {
        flex-direction: column;
        justify-content: center;
        padding: 14px 0;
      }
      .nav-links {
        justify-content: center;
      }
    }
  </style>
</head>
<body>
  <header>
    <div class="container nav">
      <div class="brand">
        <span class="brand-mark"></span>
        <span>GenAI Odyssey</span>
      </div>
      <nav class="nav-links" aria-label="主選單">
        <a href="#learn">學習路線</a>
        <a href="#missions">任務</a>
        <a href="#timeline">航程</a>
      </nav>
    </div>
  </header>

  <main class="container">
    <section class="hero">
      <div class="hero-grid">
        <div>
          <span class="eyebrow">生成式AI 學習航線</span>
          <h1>從 <span class="gradient">Prompt</span> 到 <span class="gradient">Production</span></h1>
          <p>
            以互動式學習旅程的方式，帶你從 AI 基礎概念、實際應用設計到系統落地，
            建立真正可運作的 AI 應用能力，而不是只會發問。
          </p>
          <div class="hero-actions">
            <button class="button primary" id="startMission">開始任務</button>
            <button class="button" id="randomTip">隨機提示</button>
          </div>
          <div class="stats">
            <div class="stat">
              <span>篇幅</span>
              <strong id="statHours">10h</strong>
            </div>
            <div class="stat">
              <span>技能點</span>
              <strong id="statXp">320</strong>
            </div>
            <div class="stat">
              <span>完成度</span>
              <strong id="statProgress">32%</strong>
            </div>
          </div>
        </div>

        <div class="orbit" aria-label="AI 學習軌道示意">
          <div class="core">AI</div>
          <div class="planet" style="--planet-color:#73e7ff;"></div>
          <div class="planet" style="--planet-color:#7aa2ff;"></div>
          <div class="planet" style="--planet-color:#a88cff;"></div>
          <div class="planet" style="--planet-color:#ff8dc7;"></div>
        </div>
      </div>
    </section>

    <section class="section" id="learn">
      <div class="section-head">
        <div>
          <h2>選擇你的學習路線</h2>
        </div>
        <p>依照你的角色與目標選擇航線</p>
      </div>

      <div class="paths">
        <div class="path-card active" data-path="starter">
          <h3>新手航線</h3>
          <p>適合想從零開始理解 AI、Prompt 設計與日常流程自動化的人。</p>
          <div class="path-meta">
            <span>基礎</span>
            <span>3 週</span>
          </div>
          <div class="progress"><div class="progress-bar" style="width: 40%;"></div></div>
        </div>

        <div class="path-card" data-path="builder">
          <h3>開發者航線</h3>
          <p>專注於工具鏈、API 串接、RAG、工作流與產品原型設計。</p>
          <div class="path-meta">
            <span>進階</span>
            <span>4 週</span>
          </div>
          <div class="progress"><div class="progress-bar" style="width: 65%;"></div></div>
        </div>

        <div class="path-card" data-path="researcher">
          <h3>研究者航線</h3>
          <p>探索模型評估、知識工程、提示式研究與 AI 系統設計思維。</p>
          <div class="path-meta">
            <span>專家</span>
            <span>5 週</span>
          </div>
          <div class="progress"><div class="progress-bar" style="width: 82%;"></div></div>
        </div>
      </div>
    </section>

    <section class="section" id="missions">
      <div class="section-head">
        <div>
          <h2>今日任務</h2>
        </div>
        <p>根據航線自動切換任務內容</p>
      </div>

      <div class="mission-panel">
        <div class="mission-box">
          <h3>任務清單</h3>
          <div class="mission-list" id="missionList"></div>
        </div>

        <div class="ai-box">
          <h3>AI 助教</h3>
          <p>你的提示詞是什麼？讓模型真正「理解任務」才能產出好結果。</p>
          <div class="mentor-bubble" id="mentorBubble">
            先選擇一條學習航線，讓 AI 助教為你制定更精準的實戰任務。
          </div>
        </div>
      </div>
    </section>

    <section class="section" id="timeline">
      <div class="section-head">
        <div>
          <h2>學習航程</h2>
        </div>
        <p>六個核心階段，逐步建立 AI 能力</p>
      </div>

      <div class="timeline">
        <div class="timeline-card">
          <div class="phase">Phase 01</div>
          <h3>AI 基礎建模</h3>
          <p>理解模型、資料、訓練與推理；學會從「輸入」到「輸出」的思維轉換。</p>
        </div>
        <div class="timeline-card">
          <div class="phase">Phase 02</div>
          <h3>Prompt Engineering</h3>
          <p>學會設計有效 prompts，控制風格、約束、輸出格式與任務分解能力。</p>
        </div>
        <div class="timeline-card">
          <div class="phase">Phase 03</div>
          <h3>RAG & 知識整合</h3>
          <p>具備檢索增強生成的邏輯，將外部文件與知識庫整合進 AI 流程中。</p>
        </div>
        <div class="timeline-card">
          <div class="phase">Phase 04</div>
          <h3>AI 工作流</h3>
          <p>掌握工具調用、流程串接與任務自動化，從單一 prompt 升級為完整系統。</p>
        </div>
        <div class="timeline-card">
          <div class="phase">Phase 05</div>
          <h3>Evaluation</h3>
          <p>建立評估標準，衡量輸出品質、風險與真實世界適用性，避免「看起來很像」。</p>
        </div>
        <div class="timeline-card">
          <div class="phase">Phase 06</div>
          <h3>Launch & Scale</h3>
          <p>將 AI 能力落地為產品、服務或自動化流程，並持續觀察與迭代。</p>
        </div>
      </div>

      <div class="cta">
        <button class="button primary" id="launchBtn">立即啟航</button>
      </div>
    </section>
  </main>

  <footer class="footer">
    © 2026 GenAI Odyssey • 生成式AI 學習航線
  </footer>

  <script>
    const routeData = {
      starter: {
        title: "新手航線",
        hours: "6h",
        xp: 320,
        progress: 32,
        tasks: [
          "理解 AI、LLM 與生成式內容的基本概念",
          "練習 5 個高品質 Prompt 模板",
          "設計一份個人 AI 助理的日常工作流",
          "完成一個小型自動化任務（如整理筆記）",
          "建立一個簡單的 AI 專案資料夾結構"
        ],
        tips: [
          "好的 Prompt 不是長，而是清楚。",
          "先問『我要做什麼』，再問『我需要怎麼輸出』。",
          "把模型當成合作夥伴，而不是一個黑箱。",
          "先做一個小任務，讓 AI 幫你從 0 到 1。"
        ]
      },
      builder: {
        title: "開發者航線",
        hours: "9h",
        xp: 540,
        progress: 52,
        tasks: [
          "串接 OpenAI / Azure OpenAI API",
          "使用 JSON Schema 定義輸出格式",
          "實作 RAG：從知識庫中搜尋並回應",
          "設計工作流：輸入 → 檢查 → 回覆 → 儲存",
          "部署一個原型網站或小型 API"
        ],
        tips: [
          "先把任務拆成步驟，讓模型更容易遵循。",
          "避免只把資料塞給模型，需要給它工具與上下文。",
          "用實驗筆記記錄每次 prompt 的效果差異。",
          "一個小型產品原型往往比長篇演示更有說服力。"
        ]
      },
      researcher: {
        title: "研究者航線",
        title2: "研究型航線",
        hours: "12h",
        xp: 780,
        progress: 82,
        tasks: [
          "設計對照實驗，評估不同 model 的輸出差異",
          "分析 prompt 改變造成的輸出品質變化",
          "建立 RAG 評估資料集與錯誤分類",
          "撰寫模型安全性與成本效論述",
          "研究多模態與 AI 系統實驗設計"
        ],
        tips: [
          "模型不是『正確』或『錯誤』，而是『適合』或『不適合』。",
          "寫出明確評估標準，才能比較不同實驗結果。",
          "研究需要系統思維，而不只是單點技巧。",
          "將歷史實驗結果轉成可重複的流程。"
        ]
      }
    };

    const missionList = document.getElementById("missionList");
    const mentorBubble = document.getElementById("mentorBubble");
    const statHours = document.getElementById("statHours");
    const statXp = document.getElementById("statXp");
    const statProgress = document.getElementById("statProgress");

    const pathCards = [...document.querySelectorAll(".path-card")];
    const randomTips = [
      "把 prompt 設計視為產品設計，而不是單純文字遊戲。",
      "先讓模型回答『它知道什麼』，再讓它做更有價值的事情。",
      "無論是 FAQ、生成摘要或智慧客服，先定義輸出格式。",
      "每次讓模型交付一件可檢查的成果，而不是一大堆模糊描述。"
    ];

    function renderRoute(pathKey) {
      const route = routeData[pathKey];
      if (!route) return;

      pathCards.forEach(card => {
        card.classList.toggle("active", card.dataset.path === pathKey);
      });

      missionList.innerHTML = route.tasks.map(task => `
        <div class="mission-item">
          <div class="dot"></div>
          <div>${task}</div>
        </div>
      `).join("");

      mentorBubble.textContent = route.tips[Math.floor(Math.random() * route.tips.length)];
      statHours.textContent = route.hours;
      statXp.textContent = route.xp;
      statProgress.textContent = route.progress + "%";

      document.title = "GenAI Odyssey - " + route.title;
    }

    pathCards.forEach(card => {
      card.addEventListener("click", () => renderRoute(card.dataset.path));
    });

    document.getElementById("startMission").addEventListener("click", () => {
      const active = document.querySelector(".path-card.active");
      const route = routeData[active.dataset.path];
      if (!route) return;
      mentorBubble.textContent = "你現在正在「" + route.title + "」航線。下一步：先拆解任務，然後用精準 prompt 讓 AI 幫你生成具體成果。";
    });

    document.getElementById("randomTip").addEventListener("click", () => {
      const random = randomTips[Math.floor(Math.random() * randomTips.length)];
      mentorBubble.textContent = random;
    });

    document.getElementById("launchBtn").addEventListener("click", () => {
      const active = document.querySelector(".path-card.active");
      const route = routeData[active.dataset.path];
      const quote = route ? route.title : "新手航線";
      mentorBubble.textContent = "啟航成功！你已經進入「" + quote + "」航線，接下來請從第一個任務開始，逐步建立可交付的 AI 能力。";
    });

    renderRoute("starter");
  </script>
</body>
</html>
