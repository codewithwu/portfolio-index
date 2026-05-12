# 作品集首页 — 设计文档

## 概述

一个纯前端作品集展示页面，从 `data.json` 读取项目数据，以网格卡片形式展示在首页，点击卡片跳转至对应项目详情页。

## 项目结构

```
portfolio-index/
├── index.html        # 首页（读取 data.json 并渲染）
├── data.json         # 项目元数据
└── html/             # 项目详情页（用户自行放置）
    └── 数据分析Agent系统.html
```

## data.json 结构

```json
[
    {
        "name": "项目名称",
        "path": "html/项目文件名.html",
        "github": "https://github.com/username/repo",
        "star": 1,
        "description": "简短的一句话描述"
    }
]
```

- `star` 数值越小排序越靠前
- `description` 为项目的一句话简介

## 首页设计

### 视觉风格
- 深色科技风，与 `html/` 内的项目详情页保持一致
- 背景：`#0c0f14` 深色
- 卡片：`#1a1f2a` 背景 + `rgba(255,255,255,0.06)` 边框
- 强调色：绿色 `#10b981`、紫色 `#8b5cf6`

### 布局
- 网格卡片布局，每行 3 列
- 响应式：平板（≤1024px）2 列，手机（≤640px）1 列
- 卡片 hover 有微上浮 + 边框高亮效果

### 卡片内容
- 项目名称（居中或左对齐）
- 一句话描述
- GitHub 链接显示在卡片下方
- 点击整张卡片区域跳转到 `path` 对应的 html 文件

### 排序
- 按 `star` 字段升序排列（star 越小越靠前）

## 技术实现

- 纯前端，无后端依赖
- `index.html` 使用 `<script>` fetch `data.json`，动态生成卡片 DOM
- 字体使用 Google Fonts（Noto Sans SC）
- 样式内联或外联均可
