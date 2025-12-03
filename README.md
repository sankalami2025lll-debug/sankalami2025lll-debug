[index.html](https://github.com/user-attachments/files/23905329/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="format-detection" content="telephone=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="爱心表白">
    <meta name="theme-color" content="#f43f5e">
    <title>爱心表白便签</title>
    
    <!-- 预加载关键资源 -->
    <link rel="preload" href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" as="style">
    
    <!-- Font Awesome -->
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            min-height: 100vh;
            background-color: #fdf2f7;
            cursor: pointer;
            overflow: hidden;
            position: relative;
            transition: background-color 0.5s ease;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .start-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background-color: #fdf2f7;
            z-index: 100;
            transition: opacity 0.8s ease;
        }

        .love-note {
            position: absolute;
            width: clamp(60px, 25vw, 100px);
            height: clamp(60px, 25vw, 100px);
            background-color: #f43f5e;
            transform: rotate(45deg) scale(0);
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: all 0.6s cubic-bezier(0.17, 0.89, 0.32, 1.49);
            z-index: 10;
        }

        .love-note::before,
        .love-note::after {
            content: "";
            position: absolute;
            width: 100%;
            height: 100%;
            background-color: inherit;
            border-radius: 50%;
        }

        .love-note::before {
            top: -50%;
            left: 0;
        }

        .love-note::after {
            top: 0;
            left: -50%;
        }

        .note-text {
            position: relative;
            z-index: 20;
            font-family: 'Arial Rounded MT Bold', sans-serif;
            font-size: clamp(10px, 3vw, 14px);
            color: white;
            text-align: center;
            transform: rotate(-45deg);
            width: 80%;
            line-height: 1.3;
        }

        .love-note.show {
            opacity: 0.9;
            transform: rotate(45deg) scale(1);
        }

        /* 不同颜色的爱心便签 */
        .color-1 { background-color: #f43f5e; }
        .color-2 { background-color: #ec4899; }
        .color-3 { background-color: #8b5cf6; }
        .color-4 { background-color: #ec4899; }
        .color-5 { background-color: #f97316; }
    </style>
</head>
<body>
    <!-- 微信兼容性提示 -->
    <div id="wechat-tip" class="fixed top-0 left-0 w-full bg-pink-500 text-white text-center py-2 text-sm z-200">
        点击屏幕开始 ❤️
    </div>
    
    <div class="start-screen">
        <i class="fa fa-heart text-4xl md:text-6xl text-pink-500 mb-6 animate-pulse"></i>
        <h1 class="text-2xl md:text-3xl lg:text-4xl font-bold text-pink-700 mb-4">点击屏幕开始</h1>

    </div>

    <script>
        // 表白文案列表（50条）
        const loveMessages = [
            "蒋玲，我喜欢你，胜过一切",
            "蒋玲，记得好好吃饭，别饿肚子",
            "天气冷了，我好想见你，蒋玲",
            "蒋玲，你一笑，我的世界都亮了",
            "每天都在期待和你说话，蒋玲",
            "蒋玲，你的存在，让一切都值得",
            "想和你一起看遍所有风景，蒋玲",
            "见不到你的时候，全是想念，蒋玲",
            "蒋玲，你是我藏在心底的秘密",
            "照顾好自己，我会担心的，蒋玲",
            "蒋玲，喜欢你，是我做过最对的事",
            "今天也有在偷偷想你呀，蒋玲",
            "蒋玲，天冷了，记得多穿点衣服",
            "和你在一起的时光最美好，蒋玲",
            "蒋玲，你的眼睛里有星星",
            "想把所有温柔都给你，蒋玲",
            "蒋玲，别熬夜了，早点休息哦",
            "想陪你走过每个春夏秋冬，蒋玲",
            "看到好吃的，第一时间想到你，蒋玲",
            "蒋玲，你就是我要找的那个人",
            "不管多远，心都和你在一起，蒋玲",
            "今天的风，都带着对你的思念，蒋玲",
            "蒋玲，记得多喝水，保持好心情",
            "喜欢你的一切，包括小缺点，蒋玲",
            "蒋玲，想和你一起做很多很多事",
            "天气变凉，注意保暖呀，蒋玲",
            "蒋玲，我的心意，你感受到了吗",
            "见到你，所有烦恼都消失了，蒋玲",
            "蒋玲，你在我心里，很重要",
            "记得按时吃饭，别亏待自己，蒋玲",
            "蒋玲，我想每天都能见到你",
            "你的声音，很动听，蒋玲",
            "蒋玲，天冷了，手冷的话我帮你暖",
            "喜欢你，没有为什么，蒋玲",
            "蒋玲，记得好好照顾自己，我会想你",
            "和你聊天，时间总是过得很快，蒋玲",
            "蒋玲，你一笑，我就没辙了",
            "天气冷了，多喝热水哦，蒋玲",
            "蒋玲，我想和你有很多很多以后",
            "看到美景，想第一时间分享给你，蒋玲",
            "蒋玲，记得吃早餐，对身体好",
            "你是我不期而遇的惊喜，蒋玲",
            "蒋玲，天冷了，别感冒啦",
            "我喜欢你，认真且坚定，蒋玲",
            "蒋玲，记得多休息，别太累了",
            "想牵着你的手，一直走下去，蒋玲",
            "蒋玲，你的可爱，治愈了一切",
            "天气冷了，抱抱你就暖和了，蒋玲",
            "蒋玲，我想每天对你说晚安",
            "你是我最想珍惜的人，蒋玲"
        ];

        const startScreen = document.querySelector('.start-screen');
        const wechatTip = document.getElementById('wechat-tip');
        const body = document.body;
        let hasClicked = false;
        
        // 隐藏微信提示
        setTimeout(() => {
            if (wechatTip) {
                wechatTip.style.opacity = '0';
                setTimeout(() => {
                    wechatTip.style.display = 'none';
                }, 300);
            }
        }, 3000);

        // 点击事件处理
        function handleClick() {
            if (hasClicked) return;
            hasClicked = true;

            // 隐藏开始屏幕
            startScreen.style.opacity = '0';
            setTimeout(() => {
                startScreen.style.display = 'none';
            }, 800);

            // 依次创建并显示爱心便签
            loveMessages.forEach((message, index) => {
                setTimeout(() => {
                    createLoveNote(message);
                }, index * 200); // 0.2秒间隔
            });
        }

        // 添加多种点击事件监听，确保在微信中正常工作
        body.addEventListener('click', handleClick);
        body.addEventListener('touchstart', handleClick, { passive: true });
        body.addEventListener('touchend', handleClick, { passive: true });

        // 创建爱心便签函数
        function createLoveNote(message) {
            // 创建元素
            const note = document.createElement('div');
            const text = document.createElement('div');
            
            // 设置类名和内容
            const colorClass = `color-${Math.floor(Math.random() * 5) + 1}`;
            note.className = `love-note ${colorClass}`;
            text.className = 'note-text';
            text.textContent = message;
            
            note.appendChild(text);
            body.appendChild(note);

            // 获取便签尺寸（使用getComputedStyle获取实际渲染尺寸）
            const noteSize = parseInt(getComputedStyle(note).width);
            
            // 随机位置 - 优化算法确保便签分布更均匀
            const screenWidth = window.innerWidth;
            const screenHeight = window.innerHeight;
            
            // 确保便签完全显示在屏幕内，并留出边距
            const padding = 20; // 边距
            const maxX = screenWidth - noteSize - padding * 2;
            const maxY = screenHeight - noteSize - padding * 2;
            
            // 使用更均匀的随机分布算法
            const randomX = Math.floor(Math.random() * maxX) + padding;
            const randomY = Math.floor(Math.random() * maxY) + padding;
            
            note.style.left = `${randomX}px`;
            note.style.top = `${randomY}px`;

            // 触发显示动画
            setTimeout(() => {
                note.classList.add('show');
            }, 50);

            // 添加轻微浮动效果
            addFloatAnimation(note);
        }

        // 添加轻微浮动效果
        function addFloatAnimation(element) {
            let angle = 0;
            const originalTop = parseInt(element.style.top);
            
            // 使用requestAnimationFrame优化动画性能
            function animate() {
                angle += 0.02;
                const floatY = Math.sin(angle) * 8; // 上下浮动距离
                element.style.top = `${originalTop + floatY}px`;
                requestAnimationFrame(animate);
            }
            
            animate();
        }
    </script>
</body>
</html>
