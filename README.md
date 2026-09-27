<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tcabaran Padanan Cermin (Sains Tingkatan 1)</title>
    <style>
        :root {
            --primary-color: #4f46e5;
            --secondary-color: #312e81;
            --accent-color: #10b981;
            --bg-color: #f3f4f6;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #1f2937;
        }
        h1 {
            color: var(--secondary-color);
            margin-bottom: 5px;
            font-size: 1.5rem;
            text-align: center;
        }
        .subtitle {
            color: #4b5563;
            font-size: 0.95rem;
            margin-bottom: 20px;
            text-align: center;
        }
        .game-container {
            max-width: 600px;
            width: 100%;
            background: white;
            padding: 20px;
            border-radius: 16px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .level-indicator {
            font-weight: bold;
            color: var(--primary-color);
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
        }
        .scenario-box {
            background: #e0e7ff;
            border: 2px dashed #6366f1;
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 20px;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }
        .scenario-desc-zh {
            font-size: 0.9rem;
            color: #4b5563;
            font-weight: normal;
            margin-top: 8px;
        }
        .options-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 12px;
        }
        .mirror-btn {
            background: white;
            border: 2px solid #d1d5db;
            padding: 15px;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: left;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .mirror-btn:hover {
            border-color: var(--primary-color);
            background: #f5f3ff;
            transform: translateY(-2px);
        }
        .mirror-btn span.zh {
            font-size: 0.85rem;
            color: #6b7280;
            font-weight: normal;
        }
        #feedback {
            margin-top: 15px;
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            font-weight: bold;
            display: none;
        }
        .correct {
            background: #d1fae5;
            color: #065f46;
        }
        .wrong {
            background: #fee2e2;
            color: #991b1b;
        }
        .hidden {
            display: none;
        }
        .score-board {
            text-align: center;
            font-size: 1.2rem;
            font-weight: bold;
            color: var(--accent-color);
            margin-top: 20px;
        }
        .restart-btn {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
            display: block;
            width: 100%;
        }
        .restart-btn:hover {
            background: var(--secondary-color);
        }
    </style>
</head>
<body>

    <h1>镜子匹配挑战 | Cabaran Padanan Cermin</h1>
    <div class="subtitle">Sains Tingkatan 1: Bab 7 (Cahaya dan Optik)</div>

    <div class="game-container" id="game-box">
        <div class="level-indicator">
            <span id="level-title">Tahap 1 / 6</span>
            <span id="score-text">Skor: 0</span>
        </div>

        <div class="scenario-box" id="scenario-box">
            <!-- 场景内容由JS动态加载 -->
        </div>

        <div class="options-grid" id="options-grid">
            <!-- 选项由JS动态加载 -->
        </div>

        <div id="feedback"></div>
    </div>

    <div id="end-screen" class="game-container hidden" style="text-align: center;">
        <h2>🎉 Tahniah! 恭喜通关！</h2>
        <p>你已经成功掌握了所有关于平镜、凹镜和凸镜的应用场景！</p>
        <div class="score-board" id="final-score"></div>
        <button class="restart-btn" onclick="restartGame()">再玩一次 | Main Lagi</button>
    </div>

<script>
    const levels = [
        {
            level: 1,
            scenario: "Seseorang sedang bersiap, merapikan rambut atau bersolek di bilik.",
            scenarioZh: "一个人在房间里照镜子梳头或化妆 (Cermin untuk berhias)",
            correct: "Cermin Satah",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        },
        {
            level: 2,
            scenario: "Doktor gigi menggunakan cermin kecil untuk memeriksa gigi pesakit.",
            scenarioZh: "牙医用小镜子检查牙齿（放大牙齿影像）",
            correct: "Cermin Cekung",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        },
        {
            level: 3,
            scenario: "Lampu depan kereta mengeluarkan pancaran cahaya yang kuat dan lurus.",
            scenarioZh: "汽车车头灯发射出笔直强烈的光束（汇聚光线）",
            correct: "Cermin Cekung",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        },
        {
            level: 4,
            scenario: "Cermin sisi pada kenderaan (kereta/motor) untuk melihat kenderaan di belakang.",
            scenarioZh: "汽车的侧后视镜（缩小影像但扩大视野）",
            correct: "Cermin Cembung",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        },
        {
            level: 5,
            scenario: "Cermin keselamatan di selekoh jalan raya yang berbahaya.",
            scenarioZh: "马路转弯处的安全凸面镜（扩大视野防止盲区）",
            correct: "Cermin Cembung",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        },
        {
            level: 6,
            scenario: "Cermin keselamatan di bumbung pasar raya untuk memantau kawasan.",
            scenarioZh: "超市天花板上的防盗全景凸镜",
            correct: "Cermin Cembung",
            options: ["Cermin Satah (平镜)", "Cermin Cekung (凹镜)", "Cermin Cembung (凸镜)"]
        }
    ];

    let currentLevel = 0;
    let score = 0;
    let isAnswered = false;

    function loadLevel() {
        isAnswered = false;
        const data = levels[currentLevel];
        
        document.getElementById("level-title").innerText = `Tahap ${currentLevel + 1} / ${levels.length}`;
        document.getElementById("score-text").innerText = `Skor: ${score}`;
        
        const scenarioBox = document.getElementById("scenario-box");
        scenarioBox.innerHTML = `
            <div>📍 ${data.scenario}</div>
            <div class="scenario-desc-zh">${data.scenarioZh}</div>
        `;

        const optionsGrid = document.getElementById("options-grid");
        optionsGrid.innerHTML = "";

        // 打乱选项顺序
        let shuffledOptions = [...data.options].sort(() => Math.random() - 0.5);

        shuffledOptions.forEach(opt => {
            let btn = document.createElement("button");
            btn.className = "mirror-btn";
            let cleanOptName = opt.split(" (")[0]; // 提取马来文名称用于比对
            btn.innerHTML = `<span>${opt}</span>`;
            btn.onclick = () => checkAnswer(cleanOptName, data.correct, btn);
            optionsGrid.appendChild(btn);
        });

        document.getElementById("feedback").style.display = "none";
    }

    function checkAnswer(selected, correct, btnElement) {
        if (isAnswered) return;
        isAnswered = true;

        const feedback = document.getElementById("feedback");
        feedback.style.display = "block";

        if (selected === correct) {
            feedback.className = "correct";
            feedback.innerHTML = "✅ Betul! 答对了！";
            score += 10;
            document.getElementById("score-text").innerText = `Skor: ${score}`;
            
            setTimeout(() => {
                currentLevel++;
                if (currentLevel < levels.length) {
                    loadLevel();
                } else {
                    showEndScreen();
                }
            }, 1200);
        } else {
            feedback.className = "wrong";
            feedback.innerHTML = `❌ Salah! Jawapan yang betul ialah: <b>${correct}</b>`;
            
            setTimeout(() => {
                currentLevel++;
                if (currentLevel < levels.length) {
                    loadLevel();
                } else {
                    showEndScreen();
                }
            }, 2000);
        }
    }

    function showEndScreen() {
        document.getElementById("game-box").classList.add("hidden");
        document.getElementById("end-screen").classList.remove("hidden");
        document.getElementById("final-score").innerText = `Skor Akhir Anda: ${score} / ${levels.length * 10}`;
    }

    function restartGame() {
        currentLevel = 0;
        score = 0;
        isAnswered = false;
        document.getElementById("game-box").classList.remove("hidden");
        document.getElementById("end-screen").classList.add("hidden");
        loadLevel();
    }

    // 初始化加载第一关
    loadLevel();
</script>

</body>
</html>
