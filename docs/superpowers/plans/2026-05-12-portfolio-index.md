# 作品集首页 — 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建作品集首页 `index.html`，从 `data.json` 读取数据渲染网格卡片

**Architecture:** 纯前端单页应用，无后端依赖，Fetch API 读取 JSON，动态 DOM 生成

**Tech Stack:** 原生 HTML/CSS/JS，Google Fonts (Noto Sans SC)

---

## 文件结构

```
portfolio-index/
├── index.html        # 新建：首页
├── data.json         # 修改：增加 description 字段
└── html/             # 已存在：项目详情页
```

---

## Task 1: 更新 data.json 增加 description 字段

**Files:**
- Modify: `data.json`

- [ ] **Step 1: 更新 data.json，添加 description 字段**

```json
[
    {
        "name": "数据分析Agent系统",
        "path": "html/数据分析Agent系统.html",
        "github": "https://github.com/codewithwu/car-sales-agent",
        "star": 1,
        "description": "基于规则引擎 + LangGraph 工作流的汽车销售数据智能查询系统"
    }
]
```

---

## Task 2: 创建 index.html 首页

**Files:**
- Create: `index.html`

- [ ] **Step 1: 编写 index.html**

完整代码如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cooper's Portfolio</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-primary: #0c0f14;
            --bg-secondary: #141922;
            --bg-card: #1a1f2a;
            --accent-emerald: #10b981;
            --accent-violet: #8b5cf6;
            --text-primary: #f8fafc;
            --text-secondary: #94a3b8;
            --text-muted: #64748b;
            --border-subtle: rgba(255, 255, 255, 0.06);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background: var(--bg-primary);
            font-family: 'Noto Sans SC', system-ui, sans-serif;
            color: var(--text-primary);
            min-height: 100vh;
        }

        .bg-pattern {
            position: fixed;
            inset: 0;
            background-image:
                radial-gradient(circle at 1px 1px, rgba(16, 185, 129, 0.06) 1px, transparent 0);
            background-size: 40px 40px;
            pointer-events: none;
            z-index: 0;
        }

        .glow {
            position: fixed;
            border-radius: 50%;
            filter: blur(120px);
            opacity: 0.12;
            pointer-events: none;
            z-index: 0;
        }
        .glow-1 { top: -100px; left: 10%; width: 500px; height: 500px; background: var(--accent-emerald); }
        .glow-2 { bottom: -150px; right: 5%; width: 600px; height: 600px; background: var(--accent-violet); }

        .container {
            position: relative;
            z-index: 1;
            max-width: 1200px;
            margin: 0 auto;
            padding: 60px 32px;
        }

        .header {
            text-align: center;
            margin-bottom: 64px;
        }

        .header h1 {
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 12px;
            background: linear-gradient(135deg, var(--text-primary) 0%, var(--accent-emerald) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .header p {
            color: var(--text-secondary);
            font-size: 1rem;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
        }

        .card {
            background: var(--bg-card);
            border: 1px solid var(--border-subtle);
            border-radius: 16px;
            padding: 28px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            text-decoration: none;
            color: inherit;
            display: block;
        }

        .card:hover {
            transform: translateY(-4px);
            border-color: rgba(255, 255, 255, 0.15);
            box-shadow: 0 16px 32px rgba(0, 0, 0, 0.3);
        }

        .card-name {
            font-size: 1.25rem;
            font-weight: 700;
            margin-bottom: 12px;
            color: var(--text-primary);
        }

        .card-desc {
            font-size: 0.9rem;
            color: var(--text-secondary);
            line-height: 1.6;
            margin-bottom: 16px;
        }

        .card-github {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 0.85rem;
            color: var(--accent-emerald);
            text-decoration: none;
        }

        .card-github:hover {
            text-decoration: underline;
        }

        .empty {
            text-align: center;
            color: var(--text-muted);
            padding: 60px 0;
            grid-column: 1 / -1;
        }

        @media (max-width: 1024px) {
            .grid { grid-template-columns: repeat(2, 1fr); }
        }

        @media (max-width: 640px) {
            .grid { grid-template-columns: 1fr; }
            .container { padding: 40px 16px; }
            .header h1 { font-size: 2rem; }
        }
    </style>
</head>
<body>
    <div class="bg-pattern"></div>
    <div class="glow glow-1"></div>
    <div class="glow glow-2"></div>

    <div class="container">
        <div class="header">
            <h1>Cooper's Portfolio</h1>
            <p>Some projects I've worked on</p>
        </div>
        <div class="grid" id="projects"></div>
    </div>

    <script>
        async function loadProjects() {
            try {
                const res = await fetch('data.json');
                const data = await res.json();

                data.sort((a, b) => a.star - b.star);

                const container = document.getElementById('projects');

                if (data.length === 0) {
                    container.innerHTML = '<div class="empty">No projects yet</div>';
                    return;
                }

                container.innerHTML = data.map(item => `
                    <a href="${item.path}" class="card">
                        <div class="card-name">${item.name}</div>
                        <div class="card-desc">${item.description || ''}</div>
                        ${item.github ? `<span class="card-github">${item.github}</span>` : ''}
                    </a>
                `).join('');
            } catch (e) {
                document.getElementById('projects').innerHTML =
                    '<div class="empty">Failed to load projects</div>';
            }
        }

        loadProjects();
    </script>
</body>
</html>
```

---

## Task 3: 验证

**Files:**
- None (verification only)

- [ ] **Step 1: 确认文件存在且格式正确**

检查点：
1. `index.html` 已创建
2. `data.json` 包含 `description` 字段
3. 浏览器打开 `index.html` 能正确显示卡片
4. 点击卡片能跳转对应 html 文件

---
