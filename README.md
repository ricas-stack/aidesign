这是一份可以直接复制并保存为 `README.md` 或者 `.md` 文件的内容。包含了项目介绍、使用方法以及完整的源代码。

你可以直接点击代码块右上角的“复制”按钮，然后粘贴到你的 GitHub 仓库中。

***

```markdown
# Xiaohongshu (Little Red Book) Web UI Clone 📕

这是一个模仿 **小红书 (Xiaohongshu)** 网页版“发现”页面的单页 Demo。项目完全使用原生 HTML、CSS 和 JavaScript 编写，无需任何构建工具或框架。

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)

## ✨ 主要功能 (Features)

*   **瀑布流布局 (Masonry Grid)**：使用 CSS Multi-column 属性实现经典的卡片错落排列效果。
*   **响应式设计 (Responsive)**：根据屏幕宽度自动调整列数（5列 -> 2列），适配桌面端和移动端。
*   **无限滚动 (Infinite Scroll)**：简单的 JS 逻辑模拟触底自动加载更多内容。
*   **模拟数据**：利用 JavaScript 动态生成随机标题、点赞数和用户信息，图片源自 [Lorem Picsum](https://picsum.photos/)。

## 🚀 快速开始 (How to Use)

1.  创建一个新文件，命名为 `index.html`。
2.  将下方的 **完整代码** 复制并粘贴到文件中。
3.  双击 `index.html`，在浏览器中打开即可预览。

## 💻 完整代码 (Source Code)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>小红书 - 你的生活指南</title>
    <style>
        /* --- 基础重置 --- */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background-color: #fbfbfb;
            color: #333;
        }
        a { text-decoration: none; color: inherit; }

        /* --- 顶部导航栏 (Navbar) --- */
        .header {
            position: fixed; top: 0; left: 0; width: 100%; height: 72px;
            background: #fff; box-shadow: 0 1px 4px rgba(0,0,0,0.05);
            display: flex; align-items: center; justify-content: space-between;
            padding: 0 24px; z-index: 1000;
        }
        .logo {
            font-size: 30px; font-weight: bold; color: #ff2442;
            letter-spacing: -1px; margin-right: 40px; cursor: pointer;
        }
        .search-bar { flex: 1; max-width: 500px; position: relative; }
        .search-bar input {
            width: 100%; height: 40px; border-radius: 20px;
            border: 1px solid #e6e6e6; background: #f5f5f5;
            padding: 0 20px 0 40px; font-size: 14px; outline: none; transition: 0.2s;
        }
        .search-bar input:focus { background: #fff; border-color: #ff2442; }
        .search-bar::before {
            content: "🔍"; position: absolute; left: 14px; top: 50%;
            transform: translateY(-50%); font-size: 14px; opacity: 0.5;
        }
        .nav-actions { display: flex; align-items: center; gap: 20px; }
        .nav-item {
            font-size: 16px; color: #333; cursor: pointer;
            padding: 8px 12px; border-radius: 20px; transition: 0.2s;
        }
        .nav-item:hover { background-color: #f5f5f5; }
        .nav-item.active { font-weight: 600; }
        .btn-login {
            background-color: #ff2442; color: #fff; padding: 10px 24px;
            border-radius: 24px; font-weight: 600; cursor: pointer; font-size: 14px;
        }
        .btn-login:hover { background-color: #e01f3a; }

        /* --- 主体内容区域 --- */
        .main-container {
            max-width: 1400px; margin: 90px auto 20px; padding: 0 24px;
        }
        /* --- 瀑布流布局核心 --- */
        .masonry-grid { column-count: 5; column-gap: 20px; }
        @media (max-width: 1200px) { .masonry-grid { column-count: 4; } }
        @media (max-width: 900px)  { .masonry-grid { column-count: 3; } }
        @media (max-width: 600px)  { 
            .masonry-grid { column-count: 2; column-gap: 10px; } 
            .header { padding: 0 10px; } 
            .logo { font-size: 24px; margin-right: 10px; } 
            .btn-login { padding: 6px 14px; }
        }

        /* --- 笔记卡片样式 --- */
        .note-card {
            break-inside: avoid; background: #fff; border-radius: 12px;
            margin-bottom: 20px; overflow: hidden; cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            border: 1px solid transparent; /* Fix for some render issues */
        }
        .note-card:hover {
            transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        .card-image { width: 100%; background-color: #f0f0f0; position: relative; }
        .card-image img { width: 100%; display: block; object-fit: cover; }
        .play-icon {
            position: absolute; top: 10px; right: 10px; background: rgba(0,0,0,0.3);
            color: #fff; padding: 2px 6px; border-radius: 4px; font-size: 10px;
        }
        .card-content { padding: 12px; }
        .card-title {
            font-size: 14px; font-weight: 500; line-height: 1.4; color: #333;
            margin-bottom: 8px; display: -webkit-box; -webkit-line-clamp: 2;
            -webkit-box-orient: vertical; overflow: hidden;
        }
        .card-footer {
            display: flex; justify-content: space-between; align-items: center;
            font-size: 12px; color: #666;
        }
        .user-info { display: flex; align-items: center; gap: 6px; flex: 1; overflow: hidden; }
        .avatar {
            width: 20px; height: 20px; border-radius: 50%;
            background-color: #ddd; object-fit: cover; flex-shrink: 0;
        }
        .username { white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .like-info { display: flex; align-items: center; gap: 4px; }
        .heart-icon:hover { color: #ff2442; }
    </style>
</head>
<body>
    <header class="header">
        <div class="logo">小红书</div>
        <div class="search-bar"><input type="text" placeholder="搜索你感兴趣的内容..."></div>
        <div class="nav-actions">
            <div class="nav-item active">发现</div>
            <div class="nav-item">创作者服务</div>
            <div class="btn-login">登录</div>
        </div>
    </header>

    <main class="main-container">
        <div class="masonry-grid" id="waterfall-container"></div>
    </main>

    <script>
        const mockTitles = [
            "沉浸式回家 | 独居女孩的快乐谁懂啊🏠", "这才是平价彩妆的天花板！学生党必看💄", 
            "杭州周末去哪儿？西湖边的宝藏咖啡馆☕️", "OOTD | 韩系简约穿搭，秋冬必备单品🧥", 
            "减脂餐还能这么好吃？5分钟搞定低卡晚餐🥗", "我的书桌改造计划，氛围感拉满✨", 
            "人生建议：一定要去一次大理！", "零基础学插画，我是如何接单的🎨",
            "这就是为什么我不建议你买那个网红锅❌", "治愈系风景，手机也能拍出大片📸",
            "数码控：iPhone 15 Pro 深度测评📱", "上海探店 | 隐藏在弄堂里的神仙Bistro🍷"
        ];
        const container = document.getElementById('waterfall-container');
        function randomInt(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
        function randomColor() {
            const colors = ['#e1f5fe', '#fce4ec', '#f3e5f5', '#e8f5e9', '#fff3e0'];
            return colors[randomInt(0, colors.length - 1)];
        }

        function createCard(index) {
            const card = document.createElement('div');
            card.className = 'note-card';
            const imgHeight = randomInt(200, 450); 
            const title = mockTitles[index % mockTitles.length];
            const likes = randomInt(10, 5000);
            
            card.innerHTML = `
                <div class="card-image">
                    <img src="https://picsum.photos/300/${imgHeight}?random=${index + Math.random()}" loading="lazy">
                    ${Math.random() > 0.7 ? '<span class="play-icon">▶</span>' : ''}
                </div>
                <div class="card-content">
                    <div class="card-title">${title}</div>
                    <div class="card-footer">
                        <div class="user-info">
                            <div class="avatar" style="background:${randomColor()}"></div>
                            <span class="username">用户${randomInt(1000, 9999)}</span>
                        </div>
                        <div class="like-info">
                            <span class="heart-icon">♡</span>
                            <span>${likes > 1000 ? (likes/1000).toFixed(1)+'k' : likes}</span>
                        </div>
                    </div>
                </div>
            `;
            return card;
        }

        function init() { for (let i = 0; i < 20; i++) container.appendChild(createCard(i)); }
        
        window.addEventListener('scroll', () => {
            if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 500) {
                for (let i = 0; i < 10; i++) container.appendChild(createCard(randomInt(100, 999)));
            }
        });
        init();
    </script>
</body>
</html>
```

## ⚠️ 免责声明

本项目仅供学习 CSS 布局和 JS DOM 操作使用，所有 UI 设计版权归 [小红书](https://www.xiaohongshu.com) 所有。
```
