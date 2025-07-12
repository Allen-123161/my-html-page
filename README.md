<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>化学魔法抽奖</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial Rounded MT Bold', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0a081f, #1a1a3e, #0c0c2d);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow: hidden;
            position: relative;
        }
        
        /* 化学背景元素 */
        .chemistry-bg {
            position: absolute;
            color: rgba(180, 220, 255, 0.15);
            font-size: 2.5rem;
            z-index: 1;
            pointer-events: none;
            user-select: none;
            animation: float 20s infinite linear;
        }
        
        .chemistry-equation {
            position: absolute;
            color: rgba(200, 230, 255, 0.12);
            font-size: 1.8rem;
            font-style: italic;
            z-index: 1;
            pointer-events: none;
            user-select: none;
            animation: float 25s infinite linear reverse;
        }
        
        .chemistry-icon {
            position: absolute;
            color: rgba(170, 210, 255, 0.18);
            font-size: 3.2rem;
            z-index: 1;
            pointer-events: none;
            user-select: none;
            animation: float 18s infinite linear;
        }
        
        @keyframes float {
            0% { transform: translateY(0) translateX(0) rotate(0deg); }
            25% { transform: translateY(-30px) translateX(20px) rotate(5deg); }
            50% { transform: translateY(10px) translateX(-20px) rotate(-5deg); }
            75% { transform: translateY(-20px) translateX(15px) rotate(3deg); }
            100% { transform: translateY(0) translateX(0) rotate(0deg); }
        }
        
        .container {
            background: rgba(15, 10, 35, 0.92);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.8), 
                        0 0 60px rgba(80, 100, 180, 0.5) inset;
            width: 100%;
            max-width: 900px;
            padding: 40px 30px;
            text-align: center;
            overflow: hidden;
            position: relative;
            z-index: 10;
            border: 3px solid #5a7ac5;
        }
        
        /* 化学光环效果 */
        .container::before {
            content: '';
            position: absolute;
            top: -15px;
            left: -15px;
            right: -15px;
            bottom: -15px;
            background: linear-gradient(45deg, #4a65a5, #2a2a5a, #5a7ac5, #3a8fdf);
            z-index: -1;
            border-radius: 30px;
            animation: rotateHalo 30s linear infinite;
            filter: blur(30px);
            opacity: 0.4;
        }
        
        @keyframes rotateHalo {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .header {
            margin-bottom: 30px;
            position: relative;
        }
        
        .title {
            font-size: 4.2rem;
            background: linear-gradient(45deg, #6bcfff, #5a7ac5, #3a8fdf);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 40px rgba(107, 207, 255, 0.6);
            margin-bottom: 15px;
            letter-spacing: 4px;
            animation: glow 3s ease-in-out infinite alternate;
            font-weight: 900;
        }
        
        @keyframes glow {
            from { text-shadow: 0 0 20px rgba(107, 207, 255, 0.6); }
            to { text-shadow: 0 0 40px rgba(74, 101, 165, 0.8), 0 0 60px rgba(58, 143, 223, 0.7); }
        }
        
        .subtitle {
            font-size: 1.6rem;
            color: #c0d0ff;
            max-width: 700px;
            margin: 0 auto;
            line-height: 1.6;
            text-shadow: 0 0 15px rgba(192, 208, 255, 0.5);
            font-weight: 500;
            margin-bottom: 10px;
        }
        
        .subtitle-line {
            display: block;
        }
        
        /* 长方形显示区 - 更明亮的颜色 */
        .display-rectangle {
            width: 90%;
            max-width: 700px;
            height: 300px;
            margin: 0 auto 40px;
            position: relative;
            background: linear-gradient(135deg, rgba(40, 60, 120, 0.8), rgba(30, 50, 100, 0.95));
            border-radius: 15px;
            box-shadow: 
                inset 0 0 50px rgba(120, 180, 255, 0.6),
                0 0 50px rgba(80, 140, 220, 0.8);
            border: 4px solid rgba(120, 170, 255, 0.6);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            padding: 20px;
            transition: all 0.5s ease;
            z-index: 20;
        }
        
        .display-rectangle::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                linear-gradient(90deg, transparent 50%, rgba(150, 200, 255, 0.1) 50%),
                linear-gradient(transparent 50%, rgba(150, 200, 255, 0.1) 50%);
            background-size: 40px 40px;
            opacity: 0.5;
            z-index: 1;
        }
        
        /* 化学气泡效果 */
        .bubble {
            position: absolute;
            background: rgba(180, 220, 255, 0.3);
            border-radius: 50%;
            animation: bubbleUp 10s infinite ease-in-out;
            z-index: 2;
        }
        
        @keyframes bubbleUp {
            0% { transform: translateY(0) scale(0); opacity: 0; }
            10% { opacity: 0.4; }
            90% { opacity: 0.2; }
            100% { transform: translateY(-300px) scale(1.5); opacity: 0; }
        }
        
        .result-container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            z-index: 15;
        }
        
        .result-line {
            font-size: 2.8rem;
            font-weight: 800;
            color: #ffdd66;
            text-shadow: 0 0 25px rgba(255, 221, 102, 0.8);
            margin: 10px 0;
            text-align: center;
            line-height: 1.4;
            padding: 10px 30px;
            border-radius: 12px;
            background: rgba(10, 25, 50, 0.6);
            transition: all 0.5s ease;
            max-width: 90%;
        }
        
        .result-line-1 {
            font-size: 3.2rem;
            color: #ffcc00;
            font-weight: 900;
            letter-spacing: 1px;
            margin-bottom: 15px;
        }
        
        .result-line-2 {
            font-size: 2.4rem;
            color: #aae0ff;
            background: rgba(15, 35, 70, 0.5);
            padding: 12px 40px;
            border-radius: 50px;
            margin-top: 10px;
            border: 2px solid rgba(100, 180, 255, 0.4);
        }
        
        .magic-button {
            background: linear-gradient(135deg, #5a7ac5 0%, #3a3a7a 100%);
            color: white;
            border: none;
            padding: 25px 70px;
            font-size: 2.4rem;
            font-weight: bold;
            border-radius: 60px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.7), 
                        0 0 50px rgba(100, 150, 220, 0.7);
            position: relative;
            overflow: hidden;
            letter-spacing: 3px;
            margin-top: 20px;
            z-index: 20;
        }
        
        .magic-button::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -60%;
            width: 30px;
            height: 300%;
            background: rgba(200, 230, 255, 0.5);
            transform: rotate(25deg);
            transition: all 0.7s;
        }
        
        .magic-button:hover {
            transform: translateY(-8px);
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.8), 
                        0 0 60px rgba(120, 170, 255, 0.9);
        }
        
        .magic-button:hover::before {
            left: 120%;
        }
        
        .magic-button:active {
            transform: translateY(4px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
        }
        
        .footer {
            margin-top: 30px;
            color: #a0c0ff;
            font-size: 1.3rem;
            opacity: 0.8;
            text-shadow: 0 0 12px rgba(160, 192, 255, 0.4);
            line-height: 1.6;
        }
        
        /* 化学粒子效果 */
        .chemistry-particle {
            position: absolute;
            border-radius: 50%;
            pointer-events: none;
            z-index: 5;
            background: rgba(100, 180, 255, 0.6);
        }
        
        /* 响应式设计 */
        @media (max-width: 1000px) {
            .title {
                font-size: 3.5rem;
            }
            
            .display-rectangle {
                height: 280px;
            }
            
            .result-line-1 {
                font-size: 2.8rem;
            }
            
            .result-line-2 {
                font-size: 2.1rem;
            }
            
            .magic-button {
                padding: 22px 60px;
                font-size: 2.1rem;
            }
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 35px 25px;
            }
            
            .title {
                font-size: 3rem;
            }
            
            .subtitle {
                font-size: 1.4rem;
            }
            
            .display-rectangle {
                height: 250px;
                margin-bottom: 35px;
            }
            
            .result-line-1 {
                font-size: 2.4rem;
            }
            
            .result-line-2 {
                font-size: 1.9rem;
            }
            
            .magic-button {
                padding: 20px 50px;
                font-size: 1.9rem;
            }
            
            .chemistry-bg, .chemistry-equation, .chemistry-icon {
                font-size: 1.5rem;
            }
        }
        
        @media (max-width: 480px) {
            .container {
                padding: 25px 15px;
            }
            
            .title {
                font-size: 2.4rem;
                letter-spacing: 2px;
            }
            
            .subtitle {
                font-size: 1.2rem;
            }
            
            .display-rectangle {
                height: 220px;
                margin-bottom: 30px;
            }
            
            .result-line-1 {
                font-size: 1.9rem;
            }
            
            .result-line-2 {
                font-size: 1.6rem;
                padding: 8px 25px;
            }
            
            .magic-button {
                padding: 16px 40px;
                font-size: 1.6rem;
            }
            
            .footer {
                font-size: 1.1rem;
            }
        }
        
        /* 结果动画 */
        @keyframes resultAppear {
            0% { transform: scale(0.9); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        
        .result-appear {
            animation: resultAppear 0.8s ease-out forwards;
        }
        
        /* 副标题强调动画 */
        @keyframes pulse {
            0% { text-shadow: 0 0 10px rgba(192, 208, 255, 0.5); }
            50% { text-shadow: 0 0 20px rgba(192, 208, 255, 0.8); }
            100% { text-shadow: 0 0 10px rgba(192, 208, 255, 0.5); }
        }
        
        .pulse {
            animation: pulse 3s infinite;
        }
        
        /* 装饰光效 */
        .corner-light {
            position: absolute;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            filter: blur(40px);
            z-index: 2;
        }
        
        .corner-light.tl {
            top: -50px;
            left: -50px;
            background: rgba(100, 180, 255, 0.3);
        }
        
        .corner-light.tr {
            top: -50px;
            right: -50px;
            background: rgba(100, 150, 220, 0.3);
        }
        
        .corner-light.bl {
            bottom: -50px;
            left: -50px;
            background: rgba(80, 120, 200, 0.3);
        }
        
        .corner-light.br {
            bottom: -50px;
            right: -50px;
            background: rgba(70, 130, 210, 0.3);
        }
        
        /* 烟花粒子 */
        .firework {
            position: absolute;
            border-radius: 50%;
            pointer-events: none;
            z-index: 25;
        }
        
        /* 爆炸动画 */
        @keyframes explode {
            0% { 
                transform: translate(-50%, -50%) scale(0.1); 
                opacity: 1;
            }
            100% { 
                transform: translate(-50%, -50%) scale(3); 
                opacity: 0;
            }
        }
        
        .explosion {
            position: absolute;
            border-radius: 50%;
            pointer-events: none;
            z-index: 30;
            background: radial-gradient(circle, rgba(255,255,255,0.9) 0%, rgba(255,200,0,0) 70%);
            animation: explode 1.2s ease-out forwards;
        }
    </style>
</head>
<body>
    <!-- 化学背景元素 -->
    <div class="chemistry-bg" style="top: 5%; left: 5%;">H₂O</div>
    <div class="chemistry-bg" style="top: 15%; left: 85%;">CO₂</div>
    <div class="chemistry-bg" style="top: 85%; left: 10%;">NaCl</div>
    <div class="chemistry-bg" style="top: 75%; left: 90%;">C₆H₁₂O₆</div>
    <div class="chemistry-bg" style="top: 25%; left: 15%;">Fe</div>
    <div class="chemistry-bg" style="top: 45%; left: 92%;">O₂</div>
    <div class="chemistry-bg" style="top: 65%; left: 5%;">N₂</div>
    <div class="chemistry-bg" style="top: 35%; left: 80%;">HCl</div>
    
    <div class="chemistry-equation" style="top: 20%; left: 25%;">2H₂ + O₂ → 2H₂O</div>
    <div class="chemistry-equation" style="top: 60%; left: 70%;">C + O₂ → CO₂</div>
    <div class="chemistry-equation" style="top: 40%; left: 40%;">2Na + Cl₂ → 2NaCl</div>
    <div class="chemistry-equation" style="top: 80%; left: 30%;">CH₄ + 2O₂ → CO₂ + 2H₂O</div>
    
    <div class="chemistry-icon" style="top: 10%; left: 50%;">⚗️</div>
    <div class="chemistry-icon" style="top: 30%; left: 10%;">🧪</div>
    <div class="chemistry-icon" style="top: 50%; left: 90%;">🔬</div>
    <div class="chemistry-icon" style="top: 70%; left: 60%;">🧫</div>
    <div class="chemistry-icon" style="top: 90%; left: 40%;">🌡️</div>
    
    <div class="container">
        <!-- 装饰光效 -->
        <div class="corner-light tl"></div>
        <div class="corner-light tr"></div>
        <div class="corner-light bl"></div>
        <div class="corner-light br"></div>
        
        <div class="header">
            <h1 class="title">化学魔法抽奖</h1>
            <p class="subtitle">
                <span class="subtitle-line pulse">点击启动按钮，揭示化学反应的奇迹！</span>
                <span class="subtitle-line">每次实验仅限一次魔法施展</span>
            </p>
        </div>
        
        <div class="display-rectangle" id="displayRect">
            <div class="result-container">
                <div id="resultLine1" class="result-line result-line-1">化学能量蓄势待发</div>
                <div id="resultLine2" class="result-line result-line-2">等待实验启动中...</div>
            </div>
        </div>
        
        <button id="magicButton" class="magic-button">⚗️ 启动实验 ⚗️</button>
        
        <div class="footer">
            <p>化学能量已激活 | 固定实验次数: 1 | 反应结果随机 | 科学魔法仪式</p>
        </div>
    </div>
    
    <script>
        // 创建化学气泡
        function createChemistryBubbles() {
            const displayRect = document.getElementById('displayRect');
            
            for (let i = 0; i < 15; i++) {
                const bubble = document.createElement('div');
                bubble.classList.add('bubble');
                
                // 随机位置
                const left = Math.random() * 100;
                const size = Math.random() * 25 + 10;
                
                bubble.style.left = left + '%';
                bubble.style.bottom = '-20px';
                bubble.style.width = size + 'px';
                bubble.style.height = size + 'px';
                
                // 随机动画延迟和时长
                bubble.style.animationDelay = Math.random() * 5 + 's';
                bubble.style.animationDuration = (Math.random() * 10 + 10) + 's';
                
                displayRect.appendChild(bubble);
            }
        }
        
        // 创建烟花效果
        function createFireworks(x, y) {
            const colors = ['#ff3366', '#ffcc00', '#4deeea', '#74ee15', '#f000ff', '#4a65a5'];
            
            for (let i = 0; i < 120; i++) {
                const firework = document.createElement('div');
                firework.classList.add('firework');
                
                // 随机大小
                const size = Math.random() * 15 + 5;
                firework.style.width = size + 'px';
                firework.style.height = size + 'px';
                
                // 随机颜色
                firework.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                
                // 起始位置
                firework.style.left = x + 'px';
                firework.style.top = y + 'px';
                
                // 随机移动方向
                const angle = Math.random() * Math.PI * 2;
                const distance = Math.random() * 300 + 150;
                
                // 动画
                firework.style.transition = `all ${Math.random() * 1.5 + 0.8}s ease-out`;
                firework.style.opacity = '1';
                
                document.body.appendChild(firework);
                
                // 动画结束
                setTimeout(() => {
                    firework.style.transform = `translate(${Math.cos(angle) * distance}px, ${Math.sin(angle) * distance}px)`;
                    firework.style.opacity = '0';
                }, 10);
                
                // 移除元素
                setTimeout(() => {
                    firework.remove();
                }, 1800);
            }
        }
        
        // 创建爆炸效果
        function createExplosion(x, y) {
            const explosion = document.createElement('div');
            explosion.classList.add('explosion');
            
            explosion.style.left = x + 'px';
            explosion.style.top = y + 'px';
            explosion.style.width = '0';
            explosion.style.height = '0';
            
            document.body.appendChild(explosion);
            
            // 移除元素
            setTimeout(() => {
                explosion.remove();
            }, 1200);
        }
        
        // 固定文本条目（已添加新条目）
        const magicEntries = [
            "单刀直入 单数组+200",
            "单刀直入 单数组+200",
            "好事成双 双数组+200",
            "好事成双 双数组+200",
            "好风凭借力 和最高分同分",
            "好风凭借力 和最高分同分",
            "欧皇附体 小组分数×2",
            "欧皇附体 小组分数×2",
            "化合反应 抽取其他组各100分",
            "化合反应 抽取其他组各100分",
            "置换反应 从最高分组抽取200分",
            "置换反应 从最高分组抽取200分",
            "分解反应 其他组-100分",
            "分解反应 其他组-100分",
            "复分解反应 指定一组分数与本组交换",
            "复分解反应 指定一组分数与本组交换",
            "取长补短 最高分-200，最高分+200",
            "取长补短 最高分-200，最高分+200",
            "千锤百炼 本组人手一份辅导资料",
            "千锤百炼 本组人手一份辅导资料",
            "真情吐露 小组成员说三遍“我爱化学，化学使我快乐”",
            "真情吐露 小组成员说三遍“我爱化学，化学使我快乐”"
        ];
        
        // 初始化
        document.addEventListener('DOMContentLoaded', function() {
            createChemistryBubbles();
            
            const magicButton = document.getElementById('magicButton');
            const resultLine1 = document.getElementById('resultLine1');
            const resultLine2 = document.getElementById('resultLine2');
            const displayRect = document.getElementById('displayRect');
            
            // 魔法按钮点击事件
            magicButton.addEventListener('click', function(e) {
                // 禁用按钮防止多次点击
                magicButton.disabled = true;
                magicButton.style.opacity = '0.7';
                magicButton.textContent = "⚗️ 实验进行中 ⚗️";
                
                // 显示区发光效果
                displayRect.style.boxShadow = 
                    'inset 0 0 80px rgba(150, 200, 255, 0.8),' +
                    '0 0 80px rgba(120, 180, 255, 0.9)';
                displayRect.style.border = '4px solid rgba(150, 200, 255, 0.8)';
                displayRect.style.transform = 'scale(1.03)';
                displayRect.style.transition = 'all 0.8s ease-in-out';
                
                // 结果元素准备
                resultLine1.textContent = "化学反应进行中";
                resultLine2.textContent = "请稍候...";
                resultLine1.style.opacity = '0.7';
                resultLine2.style.opacity = '0.7';
                resultLine1.style.transform = 'scale(0.9)';
                resultLine2.style.transform = 'scale(0.9)';
                
                // 随机结果延迟
                setTimeout(() => {
                    // 执行抽奖
                    const winner = drawWinner(magicEntries);
                    
                    // 分割结果文本
                    const spaceIndex = winner.indexOf(' ');
                    let firstLine = winner;
                    let secondLine = '';
                    
                    if (spaceIndex !== -1) {
                        firstLine = winner.substring(0, spaceIndex);
                        secondLine = winner.substring(spaceIndex + 1);
                    }
                    
                    // 显示结果
                    displayResult(firstLine, secondLine);
                    
                    // 创建烟花效果
                    const rect = displayRect.getBoundingClientRect();
                    const centerX = rect.left + rect.width / 2;
                    const centerY = rect.top + rect.height / 2;
                    
                    createFireworks(centerX, centerY);
                    createExplosion(centerX, centerY);
                    
                    // 恢复显示区效果
                    setTimeout(() => {
                        displayRect.style.boxShadow = 
                            'inset 0 0 50px rgba(120, 180, 255, 0.6),' +
                            '0 0 50px rgba(80, 140, 220, 0.8)';
                        displayRect.style.border = '4px solid rgba(120, 170, 255, 0.6)';
                        displayRect.style.transform = 'scale(1)';
                        
                        // 重新启用按钮
                        setTimeout(() => {
                            magicButton.disabled = false;
                            magicButton.style.opacity = '1';
                            magicButton.textContent = "⚗️ 启动实验 ⚗️";
                        }, 1000);
                    }, 3000);
                }, 2000);
            });
            
            // 抽奖函数
            function drawWinner(entries) {
                const randomIndex = Math.floor(Math.random() * entries.length);
                return entries[randomIndex];
            }
            
            // 显示结果函数（两行显示）
            function displayResult(line1, line2) {
                resultLine1.textContent = line1;
                resultLine2.textContent = line2;
                
                // 添加动画效果
                resultLine1.classList.remove('result-appear');
                resultLine2.classList.remove('result-appear');
                
                // 触发重绘
                void resultLine1.offsetWidth;
                void resultLine2.offsetWidth;
                
                resultLine1.classList.add('result-appear');
                resultLine2.classList.add('result-appear');
                
                resultLine1.style.opacity = '1';
                resultLine2.style.opacity = '1';
                resultLine1.style.transform = 'scale(1)';
                resultLine2.style.transform = 'scale(1)';
            }
        });
    </script>
</body>
</html>
