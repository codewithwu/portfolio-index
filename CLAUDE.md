## 项目概述

portfolio-index 是一个纯前端作品集展示项目，用于展示个人开发的项目。

### 核心文件

- `index.html` — 首页，从 `data.json` 读取数据渲染项目卡片
- `data.json` — 项目元数据（name, path, github, star, description）
- `html/` — 项目详情页文件夹，每个项目一个 HTML 文件

### data.json 字段说明

| 字段 | 作用 | 备注 |
|------|------|------|
| name | 项目名称 | 显示在卡片上 |
| path | 项目详情页路径 | 相对于根目录，如 `html/xxx.html` |
| github | Github 仓库地址 | 会在项目详情页顶部显示 |
| star | 排序字段 | 数值越小越靠前 |
| description | 项目简短描述 | 显示在卡片上 |

### github 字段同步要求

**重要：** `data.json` 中的 `github` 字段必须同步到对应的 `html/` 项目详情页中。

- 每次更新 `data.json` 后，检查对应的 html 文件是否包含该 github 链接
- 如果 html 文件中没有 github 链接，必须在页面顶部 header 区域添加，格式如下：
  ```html
  <a href="https://github.com/username/repo" style="display: inline-block; margin-top: 16px; color: var(--accent-emerald); text-decoration: none; font-size: 0.95rem;">Github仓库: https://github.com/username/repo</a>
  ```
- 链接位置：放在 `<div class="header">` 内的标题下方，与 `header-subtitle` 平级
- 如果 html 文件中已有该 github 链接，则无需重复添加

### 新增项目流程

1. 在 `html/` 文件夹下创建项目详情页
2. 在 `data.json` 中添加对应的项目数据
3. 确保 `github` 字段的链接已添加到对应 html 文件的顶部
4. 按 star 值设置排序（新增项目通常是 star 最大的，排最后）

### 视觉风格

- 深色科技风，与 `html/` 内的项目详情页保持一致
- 背景 `#0c0f14`，卡片 `#1a1f2a`，强调色 emerald `#10b981` / violet `#8b5cf6`
- Google Fonts: Noto Sans SC

---
