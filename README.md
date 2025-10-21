<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SCL-90症状自评量表</title>
    <!-- 引入Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* 全局样式 */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }

        /* 页面样式 */
        .page {
            display: none;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 30px;
            margin-top: 30px;
        }

        .page.active {
            display: block;
            animation: fadeIn 0.5s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        h1, h2, h3 {
            color: #4A90E2;
            margin-bottom: 20px;
        }

        h1 {
            text-align: center;
            margin-bottom: 30px;
        }

        p {
            margin-bottom: 15px;
        }

        /* 按钮样式 */
        .btn {
            display: inline-block;
            background-color: #4A90E2;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s;
            text-decoration: none;
        }

        .btn:hover {
            background-color: #3A80D2;
        }

        .btn-container {
            text-align: center;
            margin-top: 30px;
        }

        /* 进度条样式 */
        .progress-container {
            width: 100%;
            height: 10px;
            background-color: #e0e0e0;
            border-radius: 5px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background-color: #4A90E2;
            width: 0%;
            transition: width 0.3s ease;
        }

        /* 题目样式 */
        .question-container {
            margin-bottom: 30px;
        }

        .question-text {
            font-size: 18px;
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f9f9f9;
            border-radius: 5px;
            border-left: 4px solid #4A90E2;
        }

        .options-container {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .option {
            display: flex;
            align-items: center;
            padding: 10px 15px;
            background-color: #f0f0f0;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .option:hover {
            background-color: #e0e0e0;
        }

        .option.selected {
            background-color: #4A90E2;
            color: white;
        }

        .option input {
            margin-right: 10px;
        }

        /* 导航按钮样式 */
        .nav-buttons {
            display: flex;
            justify-content: space-between;
            margin-top: 30px;
        }

        /* 结果页样式 */
        .result-item {
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f9f9f9;
            border-radius: 5px;
        }

        .result-item h3 {
            margin-bottom: 10px;
            color: #333;
        }

        .factor-table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        .factor-table th, .factor-table td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        .factor-table th {
            background-color: #f0f0f0;
        }

        .factor-table tr:hover {
            background-color: #f9f9f9;
        }

        .highlight {
            background-color: #fff3cd;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            border-left: 4px solid #ffc107;
        }

        .warning {
            background-color: #f8d7da;
            color: #721c24;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            border-left: 4px solid #dc3545;
        }

        .chart-container {
            width: 100%;
            height: 400px;
            margin-bottom: 30px;
        }

        /* 响应式设计 */
        @media (max-width: 600px) {
            .container {
                padding: 10px;
            }

            .page {
                padding: 20px;
            }

            .question-text {
                font-size: 16px;
            }

            .nav-buttons {
                flex-direction: column;
                gap: 10px;
            }

            .nav-buttons .btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 引导页 -->
        <section id="intro" class="page active">
            <h1>SCL-90症状自评量表</h1>
            <div class="intro-content">
                <h2>量表简介</h2>
                <p>SCL-90（Symptom Checklist-90-Revised）是世界上最著名的心理健康测试量表之一，包含90个项目，涵盖了广泛的心理健康问题。该量表用于评估一个人在最近一周内的心理健康状况，包括躯体化、强迫症状、人际关系敏感、抑郁、焦虑、敌对、恐怖、偏执、精神病性等九个维度。</p>
                
                <h2>测评须知</h2>
                <ul>
                    <li>请评定时间范围为"现在"或"最近一个星期"。</li>
                    <li>测试预计时间：约15-20分钟。</li>
                    <li>请根据自己的实际情况如实回答每一个问题。</li>
                    <li>每个问题后有五个选项，请选择最符合您情况的选项。</li>
                </ul>
                
                <h2>评分标准</h2>
                <ul>
                    <li><strong>没有</strong>：表示没有该项症状。</li>
                    <li><strong>很轻</strong>：表示有该项症状，但影响轻微。</li>
                    <li><strong>中等</strong>：表示有该项症状，影响明显。</li>
                    <li><strong>偏重</strong>：表示有该项症状，影响较重。</li>
                    <li><strong>严重</strong>：表示有该项症状，影响严重。</li>
                </ul>
                
                <div class="highlight">
                    <p><strong>注意：</strong>本测评结果仅供参考，不能作为专业诊断依据。如有需要，请关注心理健康问题，及时咨询专业心理咨询师或医生。</p>
                </div>
                
                <div class="btn-container">
                    <button id="start-test" class="btn">开始测试</button>
                </div>
            </div>
        </section>
        
        <!-- 测试页 -->
        <section id="test" class="page">
            <div class="progress-container">
                <div class="progress-bar" id="progress-bar"></div>
            </div>
            
            <div class="question-container">
                <div class="question-text" id="question-text">题目加载中...</div>
                
                <div class="options-container" id="options-container">
                    <!-- 选项将通过JavaScript动态生成 -->
                </div>
            </div>
            
            <div class="nav-buttons">
                <button id="prev-btn" class="btn" disabled>上一题</button>
                <div id="question-counter">问题 <span id="current-question">1</span>/90</div>
                <button id="next-btn" class="btn">下一题</button>
            </div>
        </section>
        
        <!-- 结果页 -->
        <section id="results" class="page">
            <h1>SCL-90测评结果</h1>
            
            <div class="result-item">
                <h3>总体评估</h3>
                <p><strong>总分：</strong><span id="total-score">0</span></p>
                <p><strong>总症状指数：</strong><span id="total-symptom-index">0</span></p>
                <p><strong>阳性项目数：</strong><span id="positive-items">0</span> (得分≥2的项目)</p>
                <p><strong>阴性项目数：</strong><span id="negative-items">0</span> (得分=1的项目)</p>
            </div>
            
            <div class="warning" id="interpretation-warning">
                <!-- 解释性文字将通过JavaScript动态生成 -->
            </div>
            
            <div class="result-item">
                <h3>各因子得分</h3>
                <div class="chart-container">
                    <canvas id="radar-chart"></canvas>
                </div>
                
                <table class="factor-table" id="factor-table">
                    <thead>
                        <tr>
                            <th>因子名称</th>
                            <th>平均分</th>
                            <th>结果解释</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- 因子数据将通过JavaScript动态动态生成 -->
                    </tbody>
                </table>
            </div>
            
            <div class="result-item">
                <h3>结果解释</h3>
                <div id="interpretation">
                    <!-- 解释性文字将通过JavaScript动态生成 -->
                </div>
            </div>
            
            <div class="warning">
                <p><strong>专业免责声明：</strong></p>
                <p>SCL-90是一个筛查工具，其结果不能作为临床诊断的依据。如需专业诊断，请务必咨询精神科医生或专业心理咨询师。</p>
            </div>
            
            <div class="btn-container">
                <button id="restart-test" class="btn">重新测试</button>
            </div>
        </section>
    </div>

    <script>
        // SCL-90题目数据
        const questions = [
            { id: 1, text: "头痛", factor: "躯体化" },
            { id: 2, text: "神经过敏，心中不踏实", factor: "焦虑" },
            { id: 3, text: "头脑中有不必要的想法或字句盘旋", factor: "强迫症状" },
            { id: 4, text: "头晕或昏倒", factor: "躯体化" },
            { id: 5, text: "对异性的兴趣减退", factor: "抑郁" },
            { id: 6, text: "对旁人责备求全", factor: "人际关系敏感" },
            { id: 7, text: "感到别人能控制您的思想", factor: "精神病性" },
            { id: 8, text: "责怪别人制造麻烦", factor: "偏执" },
            { id: 9, text: "忘记性大", factor: "强迫症状" },
            { id: 10, text: "担心自己的衣饰整齐及仪态的端正", factor: "强迫症状" },
            { id: 11, text: "容易烦恼和激动", factor: "敌对" },
            { id: 12, text: "胸痛", factor: "躯体化" },
            { id: 13, text: "害怕空旷的场所或街道", factor: "恐怖" },
            { id: 14, text: "感到自己的精力下降，活动减慢", factor: "抑郁" },
            { id: 15, text: "想结束自己的生命", factor: "抑郁" },
            { id: 16, text: "听到旁人听不到的声音", factor: "精神病性" },
            { id: 17, text: "发抖", factor: "焦虑" },
            { id: 18, text: "感到大多数人都不可信任", factor: "偏执" },
            { id: 19, text: "胃口不好", factor: "其他" },
            { id: 20, text: "容易哭泣", factor: "抑郁" },
            { id: 21, text: "同异性相处时感到害羞不自在", factor: "人际关系敏感" },
            { id: 22, text: "感到受骗，中了圈套或有人想抓住您", factor: "抑郁" },
            { id: 23, text: "无缘无故地突然感到害怕", factor: "焦虑" },
            { id: 24, text: "自己不能控制地大发脾气", factor: "敌对" },
            { id: 25, text: "怕单独出门", factor: "恐怖" },
            { id: 26, text: "经常责怪自己", factor: "抑郁" },
            { id: 27, text: "腰痛", factor: "躯体化" },
            { id: 28, text: "感到难以完成任务", factor: "强迫症状" },
            { id: 29, text: "感到孤独", factor: "抑郁" },
            { id: 30, text: "感到苦闷", factor: "抑郁" },
            { id: 31, text: "过分担忧", factor: "抑郁" },
            { id: 32, text: "对事物不感兴趣", factor: "抑郁" },
            { id: 33, text: "感到害怕", factor: "焦虑" },
            { id: 34, text: "我的感情容易受到伤害", factor: "人际关系敏感" },
            { id: 35, text: "旁人能知道您的私下想法", factor: "精神病性" },
            { id: 36, text: "感到别人不理解您、不同情您", factor: "人际关系敏感" },
            { id: 37, text: "感到人们对您不友好，不喜欢您", factor: "人际关系敏感" },
            { id: 38, text: "做事必须做得很慢以保证做得正确", factor: "强迫症状" },
            { id: 39, text: "心跳得很厉害", factor: "焦虑" },
            { id: 40, text: "恶心或胃部不舒服", factor: "躯体化" },
            { id: 41, text: "感到比不上他人", factor: "人际关系敏感" },
            { id: 42, text: "肌肉酸痛", factor: "躯体化" },
            { id: 43, text: "感到有人在监视您、谈论您", factor: "偏执" },
            { id: 44, text: "难以入睡", factor: "其他" },
            { id: 45, text: "做事必须反复检查", factor: "强迫症状" },
            { id: 46, text: "难以做出决定", factor: "强迫症状" },
            { id: 47, text: "怕乘电车、公共汽车、地铁或火车", factor: "恐怖" },
            { id: 48, text: "呼吸有困难", factor: "躯体化" },
            { id: 49, text: "一阵阵发冷或发热", factor: "躯体化" },
            { id: 50, text: "因为感到害怕而避开某些东西、场合或活动", factor: "恐怖" },
            { id: 51, text: "脑子变空了", factor: "强迫症状" },
            { id: 52, text: "身体发麻或刺痛", factor: "躯体化" },
            { id: 53, text: "喉咙有梗塞感", factor: "躯体化" },
            { id: 54, text: "感到前途没有希望", factor: "抑郁" },
            { id: 55, text: "不能集中注意力", factor: "强迫症状" },
            { id: 56, text: "感到身体的某一部分软弱无力", factor: "躯体化" },
            { id: 57, text: "感到紧张或容易紧张", factor: "焦虑" },
            { id: 58, text: "感到手或脚发重", factor: "躯体化" },
            { id: 59, text: "想到死亡的事", factor: "其他" },
            { id: 60, text: "吃得太多", factor: "其他" },
            { id: 61, text: "当别人看着您或谈论您时感到不自在", factor: "人际关系敏感" },
            { id: 62, text: "有一些不属于您自己的想法", factor: "精神病性" },
            { id: 63, text: "有想打人或伤害他人的冲动", factor: "敌对" },
            { id: 64, text: "醒得太早", factor: "其他" },
            { id: 65, text: "必须反复洗手、点数目或触摸某些东西", factor: "强迫症状" },
            { id: 66, text: "睡得不稳不深", factor: "其他" },
            { id: 67, text: "有想摔坏或破坏东西的冲动", factor: "敌对" },
            { id: 68, text: "有一些别人没有的想法或念头", factor: "偏执" },
            { id: 69, text: "感到对别人神经过敏", factor: "人际关系敏感" },
            { id: 70, text: "在商店或电影院等人多的地方感到不自在", factor: "恐怖" },
            { id: 71, text: "感到任何事情都很困难", factor: "抑郁" },
            { id: 72, text: "一阵阵恐惧或惊恐", factor: "焦虑" },
            { id: 73, text: "感到在公共场合吃东西很不舒服", factor: "人际关系敏感" },
            { id: 74, text: "经常与人争论", factor: "敌对" },
            { id: 75, text: "单独一人时神经很紧张", factor: "恐怖" },
            { id: 76, text: "别人对您的成绩没有做出恰当的评价", factor: "偏执" },
            { id: 77, text: "即使和别人在一起也感到孤单", factor: "精神病性" },
            { id: 78, text: "感到坐立不安心神不定", factor: "焦虑" },
            { id: 79, text: "感到自己没有什么价值", factor: "抑郁" },
            { id: 80, text: "感到熟悉的东西变成陌生或不像是真的", factor: "焦虑" },
            { id: 81, text: "大叫或摔东西", factor: "敌对" },
            { id: 82, text: "害怕会在公共场合昏倒", factor: "恐怖" },
            { id: 83, text: "感到别人想占您的便宜", factor: "偏执" },
            { id: 84, text: "为一些有关性的想法而很苦恼", factor: "精神病性" },
            { id: 85, text: "您认为应该因为自己的过错而受到惩罚", factor: "精神病性" },
            { id: 86, text: "感到要很快把事情做完", factor: "焦虑" },
            { id: 87, text: "感到自己的身体有严重问题", factor: "精神病性" },
            { id: 88, text: "从未感到和其他人很亲近", factor: "精神病性" },
            { id: 89, text: "感到自己有罪", factor: "其他" },
            { id: 90, text: "感到自己的脑子有毛病", factor: "精神病性" }
        ];

        // 因子分组定义
        const factors = {
            "躯体化": {
                items: [1, 4, 12, 27, 40, 42, 48, 49, 52, 53, 56, 58],
                name: "躯体化",
                interpretation: "反映身体上的不适症状，如头痛、肌肉酸痛等。高分可能表示存在明显的躯体不适。"
            },
            "强迫症状": {
                items: [3, 9, 10, 28, 38, 45, 46, 51, 55, 65],
                name: "强迫症状",
                interpretation: "反映不必要的重复行为、思维或冲动。高分可能表示存在明显的强迫症状。"
            },
            "人际关系敏感": {
                items: [6, 21, 34, 36, 37, 41, 61, 69, 73],
                name: "人际关系敏感",
                interpretation: "反映社交互动中的不适感和自卑感。高分可能表示在人际关系方面存在困扰。"
            },
            "抑郁": {
                items: [5, 14, 15, 20, 22, 26, 29, 30, 31, 32, 54, 71, 79],
                name: "抑郁",
                interpretation: "反映低落情绪、绝望感和无价值感。高分可能表示存在明显的抑郁症状。"
            },
            "焦虑": {
                items: [2, 17, 23, 33, 39, 57, 72, 78, 80, 86],
                name: "焦虑",
                interpretation: "反映过度担忧、紧张和恐惧。高分可能表示存在明显的焦虑症状。"
            },
            "敌对": {
                items: [11, 24, 63, 67, 74, 81],
                name: "敌对",
                interpretation: "反映愤怒、易怒和攻击性。高分可能表示存在明显的敌对情绪。"
            },
            "恐怖": {
                items: [13, 25, 47, 50, 70, 75, 82],
                name: "恐怖",
                interpretation: "反映对特定情境或物体的恐惧。高分可能表示存在明显的恐怖症状。"
            },
            "偏执": {
                items: [8, 18, 43, 68, 76, 83],
                name: "偏执",
                interpretation: "反映猜疑、不信任和被害妄想。高分可能表示存在明显的偏执观念。"
            },
            "精神病性": {
                items: [7, 16, 35, 62, 77, 84, 85, 87, 88, 90],
                name: "精神病性",
                interpretation: "反映精神病性症状，如幻觉、妄想和思维障碍。高分可能表示存在精神病性症状。"
            },
            "其他": {
                items: [19, 44, 59, 60, 64, 66, 89],
                name: "其他（睡眠饮食等）",
                interpretation: "反映睡眠、饮食和其他方面的问题。高分可能表示存在睡眠或饮食问题。"
            }
        };

        // 用户数据
        const userResponses = {
            currentQuestion: 0,
            answers: new Array(90).fill(null), // 存储用户对每个问题的回答(1-5分)
            completed: false,
            results: null // 存储计算结果
        };

        // DOM元素
        const pages = document.querySelectorAll('.page');
        const introPage = document.getElementById('intro');
        const testPage = document.getElementById('test');
        const resultsPage = document.getElementById('results');
        
        const startTestBtn = document.getElementById('start-test');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const restartTestBtn = document.getElementById('restart-test');
        
        const progressBar = document.getElementById('progress-bar');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const currentQuestionSpan = document.getElementById('current-question');
        
        // 结果页元素
        const totalScoreSpan = document.getElementById('total-score');
        const totalSymptomIndexSpan = document.getElementById('total-symptom-index');
        const positiveItemsSpan = document.getElementById('positive-items');
        const negativeItemsSpan = document.getElementById('negative-items');
        const interpretationWarningDiv = document.getElementById('interpretation-warning');
        const factorTableBody = document.getElementById('factor-table').querySelector('tbody');
        const interpretationDiv = document.getElementById('interpretation');

        // 切换页面
        function showPage(pageId) {
            pages.forEach(page => {
                page.classList.remove('active');
            });
            document.getElementById(pageId).classList.add('active');
        }

        // 开始测试
        startTestBtn.addEventListener('click', () => {
            showPage('test');
            showQuestion(userResponses.currentQuestion);
        });

        // 显示当前题目
        function showQuestion(index) {
            const question = questions[index];
            questionText.textContent = question.text;
            
            // 清空选项容器
            optionsContainer.innerHTML = '';
            
            // 创建选项
            const options = [
                { value: 1, text: '没有' },
                { value: 2, text: '很轻' },
                { value: 3, text: '中等' },
                { value: 4, text: '偏重' },
                { value: 5, text: '严重' }
            ];
            
            options.forEach(option => {
                const optionDiv = document.createElement('div');
                optionDiv.className = 'option';
                if (userResponses.answers[index] === option.value) {
                    optionDiv.classList.add('selected');
                }
                
                const input = document.createElement('input');
                input.type = 'radio';
                input.name = 'answer';
                input.value = option.value;
                input.id = `option-${option.value}`;
                input.checked = userResponses.answers[index] === option.value;
                
                const label = document.createElement('label');
                label.htmlFor = `option-${option.value}`;
                label.textContent = option.text;
                
                optionDiv.appendChild(input);
                optionDiv.appendChild(label);
                
                optionDiv.addEventListener('click', () => {
                    // 移除其他选项的选中状态
                    document.querySelectorAll('.option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    
                    // 添加当前选项的选中状态
                    optionDiv.classList.add('selected');
                    
                    // 选中对应的单选按钮
                    input.checked = true;
                    
                    // 保存答案
                    userResponses.answers[index] = option.value;
                });
                
                optionsContainer.appendChild(optionDiv);
            });
            
            // 更新进度
            currentQuestionSpan.textContent = index + 1;
            progressBar.style.width = `${((index + 1) / 90) * 100}%`;
            
            // 更新导航按钮状态
            prevBtn.disabled = index === 0;
            nextBtn.textContent = index === 89 ? '完成' : '下一题';
        }

        // 上一题按钮
        prevBtn.addEventListener('click', () => {
            if (userResponses.currentQuestion > 0) {
                userResponses.currentQuestion--;
                showQuestion(userResponses.currentQuestion);
            }
        });

        // 下一题按钮
        nextBtn.addEventListener('click', () => {
            // 检查是否已选择答案
            if (userResponses.answers[userResponses.currentQuestion] === null) {
                alert('请选择一个选项');
                return;
            }
            
            if (userResponses.currentQuestion < 89) {
                userResponses.currentQuestion++;
                showQuestion(userResponses.currentQuestion);
            } else {
                // 完成测试，计算结果
                userResponses.completed = true;
                calculateResults();
                displayResults();
                showPage('results');
            }
        });

        // 计算测试结果
        function calculateResults() {
            // 计算总分
            const totalScore = userResponses.answers.reduce((sum, score) => sum + score, 0);
            
            // 计算各因子分
            const factorScores = {};
            Object.keys(factors).forEach(factorKey => {
                const factor = factors[factorKey];
                const factorAnswers = factor.items.map(itemIndex => userResponses.answers[itemIndex - 1]);
                const factorTotal = factorAnswers.reduce((sum, score) => sum + score, 0);
                factorScores[factorKey] = {
                    rawScore: factorTotal,
                    average: factorTotal / factor.items.length
                };
            });
            
            // 计算阳性项目数
            const positiveItems = userResponses.answers.filter(score => score >= 2).length;
            
            // 保存结果
            userResponses.results = {
                totalScore,
                totalSymptomIndex: totalScore / 90,
                positiveItems,
                negativeItems: 90 - positiveItems,
                factorScores
            };
        }

        // 显示测试结果
        function displayResults() {
            const results = userResponses.results;
            
            // 更新总体评估
            totalScoreSpan.textContent = results.totalScore;
            totalSymptomIndexSpan.textContent = results.totalSymptomIndex.toFixed(2);
            positiveItemsSpan.textContent = results.positiveItems;
            negativeItemsSpan.textContent = results.negativeItems;
            
            // 更新警告信息
            if (results.totalScore > 160 || results.positiveItems > 43) {
                interpretationWarningDiv.innerHTML = `
                    <p><strong>注意：</strong>您的总分(${results.totalScore}分)超过常模标准(160分)，或阳性项目数(${results.positiveItems}项)超过常模标准(43项)，建议进一步关注自身心理健康状况。</p>
                `;
            } else {
                interpretationWarningDiv.innerHTML = `
                    <p><strong>总体评价：</strong>您的总分(${results.totalScore}分)和阳性项目数(${results.positiveItems}项)在常模范围内，总体心理健康状况良好。</p>
                `;
            }
            
            // 更新因子表格
            factorTableBody.innerHTML = '';
            Object.keys(results.factorScores).forEach(factorKey => {
                const factorScore = results.factorScores[factorKey];
                const factor = factors[factorKey];
                
                const row = document.createElement('tr');
                
                const nameCell = document.createElement('td');
                nameCell.textContent = factor.name;
                row.appendChild(nameCell);
                
                const scoreCell = document.createElement('td');
                scoreCell.textContent = factorScore.average.toFixed(2);
                if (factorScore.average > 2) {
                    scoreCell.style.fontWeight = 'bold';
                    scoreCell.style.color = '#dc3545';
                }
                row.appendChild(scoreCell);
                
                const interpretationCell = document.createElement('td');
                interpretationCell.textContent = factor.interpretation;
                if (factorScore.average > 2) {
                    interpretationCell.style.fontWeight = 'bold';
                }
                row.appendChild(interpretationCell);
                
                factorTableBody.appendChild(row);
            });
            
            // 更新结果解释
            let interpretation = '';
            
            // 各因子解释
            Object.keys(results.factorScores).forEach(factorKey => {
                const factorScore = results.factorScores[factorKey].average;
                const factor = factors[factorKey];
                
                if (factorScore > 2) {
                    interpretation += `<p><strong>${factor.name}</strong> (${factorScore.toFixed(2)}分)：${factor.interpretation}您的得分较高，建议关注。</p>`;
                }
            });
            
            if (interpretation === '') {
                interpretation = '<p>您在所有因子上的得分均在正常范围内，表明您当前的心理健康状况良好。</p>';
            }
            
            interpretationDiv.innerHTML = interpretation;
            
            // 创建雷达图
            createRadarChart(results);
        }

        // 创建雷达图
        function createRadarChart(results) {
            const ctx = document.getElementById('radar-chart').getContext('2d');
            
            const factorNames = Object.keys(factors).map(key => factors[key].name);
            const factorValues = Object.keys(factors).map(key => results.factorScores[key].average);
            
            // 销毁已存在的图表（如果有）
            if (window.radarChart) {
                window.radarChart.destroy();
            }
            
            window.radarChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: factorNames,
                    datasets: [{
                        label: '因子分',
                        data: factorValues,
                        backgroundColor: 'rgba(74, 144, 226, 0.2)',
                        borderColor: 'rgba(74, 144, 226, 1)',
                        pointBackgroundColor: 'rgba(74, 144, 226, 1)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgba(74, 144, 226, 1)'
                    }]
                },
                options: {
                    scales: {
                        r: {
                            min: 0,
                            max: 5,
                            ticks: {
                                stepSize: 1
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `因子分: ${context.raw.toFixed(2)}`;
                                }
                            }
                        }
                    }
                }
            });
        }

        // 重新测试按钮
        restartTestBtn.addEventListener('click', () => {
            // 重置用户数据
            userResponses.currentQuestion = 0;
            userResponses.answers = new Array(90).fill(null);
            userResponses.completed = false;
            userResponses.results = null;
            
            // 返回测试页
            showPage('test');
            showQuestion(0);
        });
    </script>
</body>
</html>

