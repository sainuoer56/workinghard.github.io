<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>收入激励计算器</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0c192c, #152642, #1d3557);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            padding: 15px;
            overflow-x: hidden;
        }
        
        .container {
            width: 100%;
            max-width: 100%;
            background: rgba(0, 0, 0, 0.75);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
            position: relative;
            height: calc(100vh - 30px);
            max-height: 720px;
            aspect-ratio: 9/16;
            display: flex;
            flex-direction: column;
        }
        
        .screen {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
            padding-bottom: 10px;
        }
        
        /* 输入界面样式 */
        .input-screen {
            width: 100%;
            height: 100%;
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        
        h1 {
            font-size: 1.8rem;
            margin-bottom: 15px;
            background: linear-gradient(to right, #ffd700, #ff8c00);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
            line-height: 1.2;
            padding: 0 10px;
        }
        
        .subtitle {
            font-size: 0.9rem;
            color: #aaa;
            margin-bottom: 20px;
            max-width: 90%;
            line-height: 1.5;
        }
        
        .input-group {
            width: 100%;
            max-width: 100%;
            margin: 15px 0;
        }
        
        .input-card {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 15px;
            margin: 10px 0;
            text-align: left;
            border: 1px solid rgba(255, 215, 0, 0.2);
        }
        
        .input-card label {
            display: block;
            margin-bottom: 8px;
            font-size: 0.95rem;
            color: #ffd700;
        }
        
        .input-card input {
            width: 100%;
            padding: 12px;
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(255, 215, 0, 0.3);
            border-radius: 10px;
            color: white;
            font-size: 1.1rem;
            text-align: center;
            outline: none;
        }
        
        button {
            background: linear-gradient(to right, #ff8c00, #ffd700);
            color: #1a1a2e;
            border: none;
            padding: 14px 25px;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            margin: 20px 0 15px;
            box-shadow: 0 4px 12px rgba(255, 140, 0, 0.4);
            width: 90%;
            max-width: 280px;
        }
        
        button:hover {
            transform: scale(1.03);
            box-shadow: 0 6px 20px rgba(255, 215, 0, 0.7);
        }
        
        /* 结果界面样式 */
        .result-screen {
            width: 100%;
            height: 100%;
            padding: 20px 15px 10px;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            text-align: center;
            position: relative;
            overflow-y: auto;
        }
        
        .result-header {
            margin-bottom: 15px;
            width: 100%;
        }
        
        .result-header h2 {
            font-size: 1.4rem;
            color: #ffd700;
            margin-bottom: 8px;
            text-shadow: 0 0 8px rgba(255, 215, 0, 0.3);
        }
        
        .result-header p {
            font-size: 0.85rem;
            color: #aaa;
        }
        
        .result-content {
            display: flex;
            flex-direction: column;
            gap: 12px;
            width: 100%;
            margin: 12px 0;
        }
        
        .income-card {
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            padding: 15px;
            width: 100%;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 215, 0, 0.3);
            position: relative;
        }
        
        .income-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 4px;
            background: linear-gradient(to right, #ff8c00, #ffd700);
        }
        
        .card-title {
            font-size: 1rem;
            margin-bottom: 10px;
            color: #aaa;
        }
        
        .income-value {
            font-size: 1.6rem;
            font-weight: bold;
            color: #4ade80;
            text-shadow: 0 0 10px rgba(74, 222, 128, 0.7);
            min-height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .per-second {
            animation: pulse 1.5s infinite alternate;
        }
        
        @keyframes pulse {
            from { text-shadow: 0 0 5px rgba(74, 222, 128, 0.5); }
            to { text-shadow: 0 0 15px rgba(74, 222, 128, 1), 0 0 8px rgba(74, 222, 128, 0.8); }
        }
        
        /* 每秒收入记录区域 */
        .income-records {
            width: 100%;
            margin: 10px 0;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            padding: 15px;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .record-title {
            color: #ffd700;
            font-size: 0.95rem;
            margin-bottom: 8px;
            text-align: center;
        }
        
        .records-container {
            height: 90px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 6px;
            padding: 8px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
        }
        
        .record-item {
            padding: 7px 10px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 6px;
            color: #4ade80;
            font-family: 'Courier New', monospace;
            text-align: left;
            animation: slideIn 0.5s ease-out;
            display: flex;
            align-items: center;
            border-left: 2px solid #ffd700;
            font-size: 0.85rem;
        }
        
        .record-item i {
            margin-right: 6px;
            color: #ffd700;
            font-size: 0.85rem;
        }
        
        @keyframes slideIn {
            from { 
                opacity: 0; 
                transform: translateX(-15px);
            }
            to { 
                opacity: 1; 
                transform: translateX(0);
            }
        }
        
        /* 进度条区域 */
        .progress-section {
            width: 100%;
            margin: 12px 0;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            padding: 15px;
            border: 1px solid rgba(255, 215, 0, 0.3);
        }
        
        .progress-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            align-items: center;
        }
        
        .progress-title {
            color: #ffd700;
            font-size: 1rem;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        
        .progress-percent {
            font-size: 1rem;
            font-weight: bold;
            color: #4ade80;
            text-shadow: 0 0 6px rgba(74, 222, 128, 0.5);
        }
        
        .progress-container {
            width: 100%;
            height: 18px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 9px;
            overflow: hidden;
            position: relative;
        }
        
        .progress-bar {
            height: 100%;
            border-radius: 9px;
            width: 0%;
            transition: width 0.5s ease;
            position: relative;
            box-shadow: 0 0 6px rgba(255, 215, 0, 0.5);
        }
        
        .daily-progress {
            background: linear-gradient(90deg, #ff8c00, #ffd700);
        }
        
        .monthly-progress {
            background: linear-gradient(90deg, #4a86e8, #5dade2);
        }
        
        .progress-label {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #1a1a2e;
            font-weight: bold;
            font-size: 0.75rem;
        }
        
        .progress-info {
            display: flex;
            justify-content: space-between;
            margin-top: 8px;
            font-size: 0.8rem;
            color: #aaa;
        }
        
        .motivation-section {
            margin-top: 12px;
            padding: 15px;
            background: rgba(255, 215, 0, 0.1);
            border-radius: 15px;
            border: 1px solid rgba(255, 215, 0, 0.2);
            position: relative;
        }
        
        .edit-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            background: transparent;
            border: none;
            color: rgba(255, 215, 0, 0.7);
            cursor: pointer;
            font-size: 0.85rem;
            padding: 4px;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }
        
        .edit-btn:hover {
            background: rgba(255, 215, 0, 0.2);
            color: #ffd700;
            transform: rotate(15deg);
        }
        
        .motivation-text {
            font-size: 1rem;
            font-style: italic;
            color: #ffd700;
            text-shadow: 0 0 6px rgba(255, 215, 0, 0.4);
            margin-bottom: 10px;
            padding: 0 20px;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            line-height: 1.4;
        }
        
        .motivation-input {
            width: 100%;
            padding: 10px;
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(255, 215, 0, 0.5);
            border-radius: 8px;
            color: white;
            font-size: 0.95rem;
            text-align: center;
            outline: none;
            margin-bottom: 8px;
            display: none;
        }
        
        .save-btn {
            background: rgba(255, 215, 0, 0.2);
            color: #ffd700;
            border: 1px solid rgba(255, 215, 0, 0.3);
            padding: 7px 14px;
            border-radius: 50px;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.3s;
            display: none;
        }
        
        .author {
            color: #aaa;
            font-size: 0.8rem;
            margin-top: 3px;
        }
        
        .reset-btn {
            background: rgba(255, 255, 255, 0.1);
            color: #ffd700;
            margin-top: 15px;
            border: 1px solid rgba(255, 215, 0, 0.3);
            padding: 10px 20px;
            font-size: 0.95rem;
            width: auto;
        }
        
        .coins-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
        }
        
        .coin {
            position: absolute;
            width: 20px;
            height: 20px;
            background: radial-gradient(circle at 30% 30%, #ffd700, #daa520);
            border-radius: 50%;
            top: -50px;
            box-shadow: 0 0 6px #ffd700;
            animation: fall linear forwards;
        }
        
        @keyframes fall {
            to {
                transform: translateY(800px) rotate(360deg);
                opacity: 0;
            }
        }
        
        .hidden {
            display: none;
        }
        
        /* 滚动条样式 */
        .records-container::-webkit-scrollbar {
            width: 5px;
        }
        
        .records-container::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 2.5px;
        }
        
        .records-container::-webkit-scrollbar-thumb {
            background: linear-gradient(to bottom, #ffd700, #ff8c00);
            border-radius: 2.5px;
        }
        
        /* 底部控制区域 */
        .control-section {
            padding: 0 15px 15px;
            display: flex;
            justify-content: center;
        }
        
        /* 进度条容器 */
        .progress-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 12px;
            margin-bottom: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="screen">
            <!-- 输入界面 -->
            <div class="input-screen" id="inputScreen">
                <h1><i class="fas fa-calculator"></i> 收入激励计算器</h1>
                <p class="subtitle">输入您的收入信息，实时查看收入增长，让每一秒的努力都变得可见！</p>
                
                <div class="input-group">
                    <div class="input-card">
                        <label><i class="fas fa-money-bill-wave"></i> 月工资（元）</label>
                        <input type="number" id="monthlyIncome" placeholder="例如：15000" min="0" value="15000">
                    </div>
                    
                    <div class="input-card">
                        <label><i class="fas fa-calendar-days"></i> 每月工作天数</label>
                        <input type="number" id="workDays" placeholder="例如：22" min="1" max="31" value="22">
                    </div>
                    
                    <div class="input-card">
                        <label><i class="fas fa-clock"></i> 每天工作时长（小时）</label>
                        <input type="number" id="hoursPerDay" placeholder="例如：8" min="1" max="24" value="8">
                    </div>
                </div>
                
                <button id="calculateBtn">
                    <i class="fas fa-rocket"></i> 开始计算
                </button>
                
                <p class="subtitle">每一次计算都是对自己价值的肯定，每一秒的增长都是努力的见证！</p>
            </div>
            
            <!-- 结果界面 -->
            <div class="result-screen hidden" id="resultScreen">
                <div class="coins-container" id="coinsContainer"></div>
                
                <div class="result-header">
                    <h2><i class="fas fa-coins"></i> 收入实时增长</h2>
                    <p>每一秒都在创造价值，每一刻都在增加财富</p>
                </div>
                
                <div class="result-content">
                    <div class="income-card">
                        <div class="card-title">每秒收入（元）</div>
                        <div id="perSecond" class="income-value per-second">0.0000</div>
                    </div>
                    
                    <div class="income-card">
                        <div class="card-title">今日收入（元）</div>
                        <div id="todayIncome" class="income-value">0.00</div>
                    </div>
                </div>
                
                <!-- 每秒收入记录区域 -->
                <div class="income-records">
                    <div class="record-title"><i class="fas fa-history"></i> 每秒收入记录</div>
                    <div class="records-container" id="recordsContainer">
                        <div class="record-item"><i class="fas fa-plus-circle"></i> 每秒收入 + 0.0000元</div>
                        <div class="record-item"><i class="fas fa-plus-circle"></i> 每秒收入 + 0.0000元</div>
                    </div>
                </div>
                
                <!-- 进度条网格 -->
                <div class="progress-grid">
                    <!-- 今日目标进度 -->
                    <div class="progress-section">
                        <div class="progress-header">
                            <div class="progress-title">
                                <i class="fas fa-sun"></i> 今日目标
                            </div>
                            <div class="progress-percent" id="progressPercent">0%</div>
                        </div>
                        
                        <div class="progress-container">
                            <div class="progress-bar daily-progress" id="dailyProgressBar" style="width: 0%">
                                <div class="progress-label" id="dailyProgressLabel">0%</div>
                            </div>
                        </div>
                        
                        <div class="progress-info">
                            <div>已完成: <span id="completedAmount">0.00</span> 元</div>
                            <div>今日目标: <span id="dailyTarget">0.00</span> 元</div>
                        </div>
                    </div>
                    
                    <!-- 月度目标进度 -->
                    <div class="progress-section">
                        <div class="progress-header">
                            <div class="progress-title">
                                <i class="fas fa-calendar-check"></i> 月度目标
                            </div>
                            <div class="progress-percent" id="monthlyProgressPercent">0%</div>
                        </div>
                        
                        <div class="progress-container">
                            <div class="progress-bar monthly-progress" id="monthlyProgressBar" style="width: 0%">
                                <div class="progress-label" id="monthlyProgressLabel">0%</div>
                            </div>
                        </div>
                        
                        <div class="progress-info">
                            <div>本月收入: <span id="monthlyIncomeEarned">0.00</span> 元</div>
                            <div>月总工资: <span id="totalMonthlyIncome">0.00</span> 元</div>
                        </div>
                    </div>
                </div>
                
                <!-- 财富格言区域 -->
                <div class="motivation-section">
                    <button class="edit-btn" id="editBtn" title="编辑格言">
                        <i class="fas fa-edit"></i>
                    </button>
                    
                    <div class="motivation-text" id="motivation">
                        "时间就是金钱，效率就是生命！你的每一秒都在创造价值"
                    </div>
                    
                    <input type="text" class="motivation-input" id="motivationInput" 
                           placeholder="输入您的激励格言..." maxlength="60">
                    <button class="save-btn" id="saveBtn">保存格言</button>
                    
                    <div class="author">- 财富格言 -</div>
                </div>
            </div>
        </div>
        
        <!-- 底部控制区域 -->
        <div class="control-section">
            <button class="reset-btn" id="resetBtn">
                <i class="fas fa-rotate-left"></i> 重新计算
            </button>
        </div>
    </div>
    
    <!-- 金币音效 -->
    <audio id="coinSound" preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-coin-win-notification-1992.mp3" type="audio/mpeg">
    </audio>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // 获取DOM元素
            const inputScreen = document.getElementById('inputScreen');
            const resultScreen = document.getElementById('resultScreen');
            const calculateBtn = document.getElementById('calculateBtn');
            const resetBtn = document.getElementById('resetBtn');
            const perSecondDisplay = document.getElementById('perSecond');
            const todayIncomeDisplay = document.getElementById('todayIncome');
            const motivationText = document.getElementById('motivation');
            const motivationInput = document.getElementById('motivationInput');
            const editBtn = document.getElementById('editBtn');
            const saveBtn = document.getElementById('saveBtn');
            const coinSound = document.getElementById('coinSound');
            const coinsContainer = document.getElementById('coinsContainer');
            const recordsContainer = document.getElementById('recordsContainer');
            const dailyProgressBar = document.getElementById('dailyProgressBar');
            const dailyProgressPercent = document.getElementById('progressPercent');
            const dailyProgressLabel = document.getElementById('dailyProgressLabel');
            const completedAmount = document.getElementById('completedAmount');
            const dailyTarget = document.getElementById('dailyTarget');
            const monthlyProgressBar = document.getElementById('monthlyProgressBar');
            const monthlyProgressPercent = document.getElementById('monthlyProgressPercent');
            const monthlyProgressLabel = document.getElementById('monthlyProgressLabel');
            const monthlyIncomeEarned = document.getElementById('monthlyIncomeEarned');
            const totalMonthlyIncome = document.getElementById('totalMonthlyIncome');
            
            let incomePerSecond = 0;
            let todayIncome = 0;
            let dailyIncome = 0;
            let monthlyIncome = 0;
            let timer;
            let lastCoinTime = 0;
            let recordCounter = 0;
            let lastUpdateTime = Date.now();
            let backgroundWorker;
            
            // 初始化自定义格言
            loadCustomMotivation();
            
            // 加载自定义格言
            function loadCustomMotivation() {
                const customMotivation = localStorage.getItem('customMotivation');
                if (customMotivation) {
                    motivationText.textContent = `"${customMotivation}"`;
                }
            }
            
            // 显示/隐藏编辑格言区域
            editBtn.addEventListener('click', () => {
                motivationText.style.display = 'none';
                motivationInput.style.display = 'block';
                saveBtn.style.display = 'block';
                
                // 设置输入框内容
                motivationInput.value = motivationText.textContent.replace(/^"|"$/g, '');
                motivationInput.focus();
            });
            
            // 保存自定义格言
            saveBtn.addEventListener('click', () => {
                const newMotivation = motivationInput.value.trim();
                if (newMotivation) {
                    motivationText.textContent = `"${newMotivation}"`;
                    localStorage.setItem('customMotivation', newMotivation);
                }
                
                // 隐藏编辑区域
                motivationText.style.display = 'flex';
                motivationInput.style.display = 'none';
                saveBtn.style.display = 'none';
            });
            
            // 计算按钮事件
            calculateBtn.addEventListener('click', () => {
                // 获取输入值
                monthlyIncome = parseFloat(document.getElementById('monthlyIncome').value);
                const workDays = parseInt(document.getElementById('workDays').value);
                const hoursPerDay = parseInt(document.getElementById('hoursPerDay').value);
                
                // 验证输入
                if (isNaN(monthlyIncome) || isNaN(workDays) || isNaN(hoursPerDay) ||
                    monthlyIncome <= 0 || workDays <= 0 || hoursPerDay <= 0) {
                    alert("请输入有效的正数");
                    return;
                }
                
                // 计算每秒收入
                const totalSeconds = workDays * hoursPerDay * 3600;
                incomePerSecond = monthlyIncome / totalSeconds;
                
                // 计算每日收入目标
                dailyIncome = monthlyIncome / workDays;
                
                // 显示结果界面
                inputScreen.classList.add('hidden');
                resultScreen.classList.remove('hidden');
                
                // 显示每秒收入（保留4位小数）
                perSecondDisplay.textContent = incomePerSecond.toFixed(4);
                
                // 重置今日收入
                todayIncome = 0;
                todayIncomeDisplay.textContent = todayIncome.toFixed(2);
                
                // 设置每日目标
                dailyTarget.textContent = dailyIncome.toFixed(2);
                
                // 设置月度目标
                totalMonthlyIncome.textContent = monthlyIncome.toFixed(2);
                monthlyIncomeEarned.textContent = "0.00";
                
                // 更新进度条
                updateProgressBars();
                
                // 清空记录容器
                recordsContainer.innerHTML = '';
                
                // 记录开始时间
                lastUpdateTime = Date.now();
                
                // 启动后台运行检测
                startBackgroundDetection();
                
                // 启动计时器（每100ms更新一次更流畅）
                if (timer) clearInterval(timer);
                timer = setInterval(updateIncome, 100);
            });
            
            // 重置按钮事件
            resetBtn.addEventListener('click', () => {
                clearInterval(timer);
                if (backgroundWorker) {
                    backgroundWorker.terminate();
                    backgroundWorker = null;
                }
                resultScreen.classList.add('hidden');
                inputScreen.classList.remove('hidden');
            });
            
            // 更新收入函数
            function updateIncome() {
                // 计算经过的时间（秒）
                const now = Date.now();
                const elapsedTime = (now - lastUpdateTime) / 1000; // 转换为秒
                lastUpdateTime = now;
                
                // 更新今日收入
                todayIncome += incomePerSecond * elapsedTime;
                todayIncomeDisplay.textContent = todayIncome.toFixed(2);
                
                // 更新本月收入（当前仅展示当天收入）
                monthlyIncomeEarned.textContent = todayIncome.toFixed(2);
                
                // 每增加0.5元播放一次金币音效并创建金币动画
                if (todayIncome - lastCoinTime >= 0.5) {
                    playCoinSound();
                    createCoinAnimation();
                    lastCoinTime = todayIncome;
                }
                
                // 更新激励语句
                updateMotivation(todayIncome);
                
                // 每秒添加一条记录（每10次添加一条记录）
                recordCounter++;
                if (recordCounter >= 10) {
                    addIncomeRecord();
                    recordCounter = 0;
                }
                
                // 更新进度条
                updateProgressBars();
            }
            
            // 启动后台运行检测
            function startBackgroundDetection() {
                // 使用Web Worker确保后台运行
                if (window.Worker) {
                    // 如果已经存在worker，先终止
                    if (backgroundWorker) {
                        backgroundWorker.terminate();
                    }
                    
                    // 创建新的Web Worker
                    const workerScript = `
                        setInterval(() => {
                            postMessage('tick');
                        }, 1000);
                    `;
                    
                    const blob = new Blob([workerScript], { type: 'application/javascript' });
                    backgroundWorker = new Worker(URL.createObjectURL(blob));
                    
                    backgroundWorker.onmessage = function(e) {
                        // 即使页面在后台，我们也会收到这个tick
                        // 这里不需要做任何事情，因为计时器在后台也会运行
                    };
                }
            }
            
            // 更新进度条
            function updateProgressBars() {
                // 计算今日目标完成百分比
                let dailyPercentage = (todayIncome / dailyIncome) * 100;
                if (dailyPercentage > 100) dailyPercentage = 100;
                
                // 更新今日进度条
                dailyProgressBar.style.width = `${dailyPercentage}%`;
                dailyProgressPercent.textContent = `${dailyPercentage.toFixed(1)}%`;
                dailyProgressLabel.textContent = `${dailyPercentage.toFixed(1)}%`;
                completedAmount.textContent = todayIncome.toFixed(2);
                
                // 计算月度目标完成百分比
                let monthlyPercentage = (todayIncome / monthlyIncome) * 100;
                if (monthlyPercentage > 100) monthlyPercentage = 100;
                
                // 更新月度进度条
                monthlyProgressBar.style.width = `${monthlyPercentage}%`;
                monthlyProgressPercent.textContent = `${monthlyPercentage.toFixed(1)}%`;
                monthlyProgressLabel.textContent = `${monthlyPercentage.toFixed(1)}%`;
                
                // 根据进度改变今日进度条颜色
                if (dailyPercentage >= 90) {
                    dailyProgressBar.style.background = "linear-gradient(90deg, #4ade80, #22c55e)";
                } else if (dailyPercentage >= 75) {
                    dailyProgressBar.style.background = "linear-gradient(90deg, #ffd700, #4ade80)";
                } else if (dailyPercentage >= 50) {
                    dailyProgressBar.style.background = "linear-gradient(90deg, #ff8c00, #ffd700)";
                } else {
                    dailyProgressBar.style.background = "linear-gradient(90deg, #ff8c00, #ffd700)";
                }
            }
            
            // 播放金币音效
            function playCoinSound() {
                coinSound.currentTime = 0;
                coinSound.play().catch(e => console.log("音效播放失败，请交互后重试:", e));
            }
            
            // 创建金币动画
            function createCoinAnimation() {
                const coin = document.createElement('div');
                coin.classList.add('coin');
                
                // 随机位置
                const leftPos = Math.random() * 90 + 5;
                coin.style.left = `${leftPos}%`;
                
                // 随机大小
                const size = Math.random() * 8 + 16;
                coin.style.width = `${size}px`;
                coin.style.height = `${size}px`;
                
                // 随机下落速度
                const duration = Math.random() * 2 + 1.5;
                coin.style.animation = `fall ${duration}s linear forwards`;
                
                coinsContainer.appendChild(coin);
                
                // 动画结束后移除元素
                setTimeout(() => {
                    coin.remove();
                }, duration * 1000);
            }
            
            // 添加收入记录
            function addIncomeRecord() {
                const recordItem = document.createElement('div');
                recordItem.classList.add('record-item');
                recordItem.innerHTML = `<i class="fas fa-plus-circle"></i> 每秒收入 + ${incomePerSecond.toFixed(4)}元`;
                
                recordsContainer.appendChild(recordItem);
                
                // 保持滚动条在底部
                recordsContainer.scrollTop = recordsContainer.scrollHeight;
                
                // 限制最多显示12条记录
                if (recordsContainer.children.length > 12) {
                    recordsContainer.removeChild(recordsContainer.firstChild);
                }
            }
            
            // 根据收入更新激励语句
            function updateMotivation(income) {
                const motivations = [
                    "时间就是金钱，效率就是生命！你的每一秒都在创造价值",
                    "财富的积累源自每一秒的坚持，继续保持！",
                    "每一秒的增长都是你能力的证明，加油！",
                    "专注当下，未来可期！你的努力正在转化为财富",
                    "持续进步，收入节节攀升！目标就在前方",
                    "你的时间价值正在不断提升，继续努力！",
                    "每一秒的坚持都在拉近你与财务自由的距离！",
                    "收入增长如同复利，点滴积累终成大海！",
                    "今日进度已过半，继续努力向目标前进！",
                    "恭喜！今日目标即将达成，继续加油！",
                    "目标达成！你的每一秒都创造了非凡价值！"
                ];
                
                // 根据进度选择不同的激励语
                const dailyPercentage = (todayIncome / dailyIncome) * 100;
                let index;
                
                if (dailyPercentage >= 100) {
                    index = 10; // 目标达成
                } else if (dailyPercentage >= 80) {
                    index = 9; // 即将达成
                } else if (dailyPercentage >= 50) {
                    index = 8; // 过半
                } else {
                    // 每10元更换一次激励语
                    index = Math.floor(income / 10) % 8;
                }
                
                motivationText.textContent = `"${motivations[index]}"`;
            }
        });
    </script>
</body>
</html># workinghard.github.io
