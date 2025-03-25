<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>吃什么决策助手</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FF6B6B;
            --secondary-color: #4ECDC4;
            --background-color: #F8F9FA;
            --text-color: #2D3436;
        }

        body {
            font-family: 'Poppins', 'Microsoft YaHei', sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 30px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        h1 {
            color: var(--primary-color);
            font-size: 2.5rem;
            margin-bottom: 2rem;
            text-align: center;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .step {
            transition: all 0.3s ease;
            padding: 2rem;
            border-radius: 10px;
            background: linear-gradient(145deg, #ffffff, #f8f9fa);
            margin-bottom: 1.5rem;
        }

        h2 {
            color: var(--secondary-color);
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
            border-bottom: 2px solid var(--secondary-color);
            padding-bottom: 0.5rem;
        }

        button {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
            background: #ff5252;
        }

        #backButton {
            position: fixed;
            top: 20px;
            left: 20px;
            background: var(--secondary-color);
            padding: 8px 20px;
            font-size: 0.9rem;
            z-index: 100;
        }

        #shopList p {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin: 10px 0;
            transition: transform 0.2s ease;
            cursor: pointer;
        }

        #shopList p:hover {
            transform: translateX(10px);
            background: linear-gradient(90deg, #f8f9fa, #fff);
        }

        .step-animation {
            animation: slideIn 0.5s ease forwards;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            h1 {
                font-size: 2rem;
            }
            button {
                width: 100%;
                margin: 8px 0;
            }
        }
    </style>
</head>
<body>
    <button id="backButton" style="display:none;" onclick="goBack()">← 返回上一步</button>
    <div class="container">
        <h1>不要再问吃啥</h1>
        
        <div id="step1" class="step step-animation">
            <h2>STEP 1. 选择用餐时段</h2>
            <button onclick="selectMeal('早餐')">🌅 早餐</button>
            <button onclick="selectMeal('中午')">☀️ 午餐</button>
            <button onclick="selectMeal('晚上')">🌙 晚餐</button>
            <button onclick="selectMeal('夜宵')">🌃 夜宵</button>
        </div>

        <div id="step2" class="step" style="display:none;">
            <h2>STEP 2. 选择想吃类型</h2>
            <div id="foodButtons" class="button-group"></div>
        </div>

        <div id="step3" class="step" style="display:none;">
            <h2>STEP 3. 推荐优质店铺</h2>
            <div id="shopList"></div>
        </div>
    </div>

    <script>
        const foodData = {
            "早餐": {
                "胡辣汤": ["星河葱花饼胡辣汤", "铺安街建宏胡辣汤", "东湖早市胡辣汤"],
                "水煎包": ["老钱家水煎包", "禹都小刚锅贴"],
                "葱花饼": ["闻喜葱花饼馍花馆", "黄河早市"],
                "豆腐脑油条": ["明收源对面", "西花园对面", "早市"],
                "饼子夹": ["随便路边吧没得推荐"],
                "包子": ["瞪眼包", "铺安街小笼包", "王老二包子", "其他不知道了"],
                "不吃": ["是不是起不来？"]
            },
            "中午": {
                "大盘鸡": ["蛋蛋家老五炒鸡", "星河摇滚炒鸡", "腾龙大盘鸡总店", "随便就近一家鸡店"],
                "面条": ["存才", "隆兴美", "酱香面馆", "裤带面", "兰州拉面", "平陆油泼面", "中银路小王鱼加面", "龙凤手擀面", "随便就近一家炒面"],
                "羊肉泡": ["王剑羊肉泡", "颐祥阁水盆羊肉", "东郭胖子羊汤", "北相羊肉胡卜", "随便就近一家羊汤"],
                "火锅干锅铜锅": ["清水阁", "一尊黄牛", "辣鸭头", "绛州铜火锅", "论斤的清水涮", "王婆大虾"],
                "鱼": ["任老师", "二峰鱼馆"],
                "米饭炒菜": ["豪德湘菜", "杨家附近湘菜", "冉师川菜", "九九九甘美", "阿宝川菜", "运中后门盖饭", "条山街蜀火爆江湖菜", "南山湘", "0359家常菜", "哈尔滨粥屋"],
                "自助": ["潮汕牛肉自助", "李姑婆牛肉火锅", "深航", "寿喜烧", "九田家", "比格"],
                "麻辣烫米线": ["随便就近吧"],
                "铁锅炖": ["原王庄里", "就近一家"],  // 已修改
                "汉堡披萨": ["你们不吃这个"]
            },
            "晚上": {
                "铁锅炖": ["原王庄里", "就近一家"],
                "涮锅": ["清水阁", "论斤的清水涮", "李姑婆牛肉火锅", "摸错门"],
                "烧烤": ["老王", "烤大侠", "喜洋洋", "满堂红", "东盛羊蝎子烤羊腿"],
                "自助": ["同中午的"],
                "小吃": ["御沁园", "禹都小吃", "各种鸭货蛋蛋最爱"]
            },
            "夜宵": {
                "喝酒": ["花你别乱点"],
                "洗澡": ["杨你别乱点"],
                "唱歌": ["钱哥你别乱点"]
            }
        };

        let selectedMeal, selectedFood;

        function selectMeal(meal) {
            selectedMeal = meal;
            animateStepTransition('step1', 'step2');
            document.getElementById("backButton").style.display = "block";
            populateButtons(foodData[meal]);
        }

        function selectFood(food) {
            selectedFood = food;
            animateStepTransition('step2', 'step3');
            displayShops(foodData[selectedMeal][food]);
        }

        function populateButtons(items) {
            const container = document.getElementById("foodButtons");
            container.innerHTML = "";
            Object.keys(items).forEach(item => {
                const button = document.createElement("button");
                button.innerHTML = `${getFoodEmoji(item)} ${item}`;
                button.onclick = () => selectFood(item);
                container.appendChild(button);
            });
        }

        function displayShops(shops) {
            const container = document.getElementById("shopList");
            container.innerHTML = shops.map(shop => `
                <p>📍 ${shop}</p>
            `).join("");
        }

        function animateStepTransition(fromId, toId) {
            const fromElement = document.getElementById(fromId);
            const toElement = document.getElementById(toId);
            
            fromElement.style.opacity = 0;
            fromElement.style.transform = "translateY(20px)";
            fromElement.style.display = "none";
            
            toElement.style.display = "block";
            setTimeout(() => {
                toElement.style.opacity = 1;
                toElement.style.transform = "translateY(0)";
            }, 50);
        }

        function goBack() {
            const step2 = document.getElementById("step2");
            const step3 = document.getElementById("step3");
            
            if (step3.style.display === "block") {
                animateStepTransition('step3', 'step2');
            } else if (step2.style.display === "block") {
                animateStepTransition('step2', 'step1');
                document.getElementById("backButton").style.display = "none";
            }
        }

        function getFoodEmoji(food) {
            const emojiMap = {
                "胡辣汤": "🥣",
                "水煎包": "🥟",
                "葱花饼": "🫓",
                "豆腐脑油条": "🥖",
                "饼子夹": "🥪",
                "包子": "🥟",
                "大盘鸡": "🍗",
                "面条": "🍜",
                "羊肉泡": "🍲",
                "火锅干锅铜锅": "🍲",
                "鱼": "🐟",
                "米饭炒菜": "🍚",
                "自助": "🍽️",
                "铁锅炖": "🥘",
                "涮锅": "🍢",
                "烧烤": "🍖",
                "小吃": "🍡"
            };
            return emojiMap[food] || "🍴";
        }
    </script>
</body>
</html>
