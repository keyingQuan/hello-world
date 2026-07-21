# 作品集展示页 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在 keyingQuan/hello-world 仓库新增影视感深色风作品集首页 + 2 个专业作品子页，发布到 GitHub Pages。

**Architecture:** 纯静态 HTML + 一份共享 CSS，零 JS、零构建、零外部依赖。index.html 为 Pages 默认入口，works/ 下每份作品一个页面，卡片组件化便于后续追加。

**Tech Stack:** HTML5 + CSS3（系统字体栈），git + GitHub Pages 发布，浏览器实测验收。

## Global Constraints

- 工作目录：`/private/tmp/claude-501/-Users-apple-claude/6dd9a989-cdce-4946-98a1-1c32001d3967/scratchpad/hello-world`
- 不修改已有文件：`hand-particles.html`、`spider-world/`、`assets/` 内既有文件
- 页面署名：`权可莹`；邮箱：`quankeying540@gmail.com`；岗位定位文案：`AI短剧生产策略与平台运营`
- 提交身份：`git -c user.name=keyingQuan -c user.email=keyingQuan@users.noreply.github.com commit ...`
- 强调色：`#e8b664`（琥珀金）；背景 `#0f1115`；卡片 `#1a1d24`；正文 `#c9cdd6`；标题 `#f2f3f5`
- 手机宽度（375px）不得出现横向滚动；表格外层包 `overflow-x:auto` 容器
- 源文档：`/Users/apple/claude/作品集/案例一页-AI质检工作流.md`、`/Users/apple/claude/作品集/方法论一页-内容质量标准.md`
- 转换保真：不增删事实性内容，仅做排版；文内「脱敏」声明保留

---

### Task 1: 共享样式 + 首页

**Files:**
- Create: `assets/portfolio.css`
- Create: `index.html`

**Interfaces:**
- Produces: `assets/portfolio.css` 中的类名供 Task 2/3 子页复用：`page`（内容容器）、`work-body`（文档正文排版）、`back-link`（返回链接）、`table-wrap`（表格横滚容器）、`tag`（能力标签）

- [ ] **Step 1: 写 assets/portfolio.css**

```css
:root{
  --bg:#0f1115; --card:#1a1d24; --line:#2a2e38;
  --text:#c9cdd6; --head:#f2f3f5; --dim:#8a90a0; --accent:#e8b664;
}
*{box-sizing:border-box;margin:0;padding:0}
html{-webkit-text-size-adjust:100%}
body{background:var(--bg);color:var(--text);
  font-family:-apple-system,BlinkMacSystemFont,"PingFang SC","Hiragino Sans GB","Microsoft YaHei",sans-serif;
  line-height:1.75;font-size:16px}
a{color:var(--accent);text-decoration:none}
a:hover{text-decoration:underline}
.page{max-width:860px;margin:0 auto;padding:48px 20px 64px}

/* ---- 首页 ---- */
.hero{padding:40px 0 8px;border-bottom:1px solid var(--line)}
.hero h1{color:var(--head);font-size:34px;letter-spacing:.02em}
.hero .role{color:var(--accent);font-size:19px;margin-top:6px;font-weight:600}
.hero .contact{color:var(--dim);margin-top:14px;font-size:14px}
.hero .contact a{color:var(--dim)}
.hero .contact a:hover{color:var(--accent)}
.section{margin-top:44px}
.section>h2{color:var(--head);font-size:15px;letter-spacing:.35em;
  text-transform:uppercase;border-left:3px solid var(--accent);padding-left:12px}
.section .sub{color:var(--dim);font-size:13px;margin-top:6px}
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(320px,1fr));gap:16px;margin-top:20px}
.card{display:block;background:var(--card);border:1px solid var(--line);
  border-radius:10px;padding:22px 22px 20px;transition:border-color .15s,transform .15s}
.card:hover{border-color:var(--accent);transform:translateY(-2px);text-decoration:none}
.card .tag{display:inline-block;color:var(--accent);border:1px solid var(--accent);
  border-radius:999px;font-size:12px;padding:1px 10px;margin-bottom:12px}
.card h3{color:var(--head);font-size:18px;line-height:1.45}
.card p{color:var(--dim);font-size:14px;margin-top:8px}
footer{margin-top:56px;padding-top:20px;border-top:1px solid var(--line);
  color:var(--dim);font-size:13px}

/* ---- 作品子页 ---- */
.back-link{display:inline-block;color:var(--dim);font-size:14px;margin-bottom:28px}
.back-link:hover{color:var(--accent);text-decoration:none}
.work-body h1{color:var(--head);font-size:26px;line-height:1.4;margin-bottom:8px}
.work-body h2{color:var(--head);font-size:19px;margin:34px 0 12px;
  border-left:3px solid var(--accent);padding-left:12px}
.work-body h3{color:var(--head);font-size:16px;margin:22px 0 8px}
.work-body p{margin:10px 0}
.work-body ul,.work-body ol{padding-left:22px;margin:10px 0}
.work-body li{margin:6px 0}
.work-body blockquote{border-left:3px solid var(--line);color:var(--dim);
  padding:2px 0 2px 14px;margin:14px 0;font-size:14px}
.work-body strong{color:var(--head)}
.table-wrap{overflow-x:auto;margin:14px 0;border:1px solid var(--line);border-radius:8px}
.work-body table{border-collapse:collapse;width:100%;min-width:560px;font-size:14px}
.work-body th{background:#20242e;color:var(--head);text-align:left}
.work-body th,.work-body td{border-bottom:1px solid var(--line);padding:10px 12px;vertical-align:top}
.work-body tr:last-child td{border-bottom:none}
@media(max-width:480px){
  .hero h1{font-size:28px}
  .page{padding-top:32px}
}
```

- [ ] **Step 2: 写 index.html**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>权可莹 · 作品集 | AI短剧生产策略与平台运营</title>
<meta name="description" content="权可莹的作品集：AI 短剧质检工作流、内容质量方法论与互动 Demo。">
<link rel="stylesheet" href="assets/portfolio.css">
</head>
<body>
<div class="page">
  <header class="hero">
    <h1>权可莹</h1>
    <div class="role">AI短剧生产策略与平台运营</div>
    <div class="contact">
      <a href="mailto:quankeying540@gmail.com">quankeying540@gmail.com</a>
      &nbsp;·&nbsp;
      <a href="https://github.com/keyingQuan" target="_blank" rel="noopener">github.com/keyingQuan</a>
    </div>
  </header>

  <section class="section">
    <h2>专业作品</h2>
    <div class="sub">真实项目沉淀，均已脱敏</div>
    <div class="cards">
      <a class="card" href="works/ai-qc-workflow.html">
        <span class="tag">AI 工作流</span>
        <h3>AI 短剧成片自动化质检工作流</h3>
        <p>从 0 到 1 主导：多模态模型联合判定 + 分层规则引擎，3 周上线，token 成本降约 87%。证明 AI 工作流设计与落地能力。</p>
      </a>
      <a class="card" href="works/content-quality.html">
        <span class="tag">方法论</span>
        <h3>短剧内容质量判断标准</h3>
        <p>单集结构评分卡、爽点节奏公式、海外本地化避雷清单。证明内容质量体系化能力。</p>
      </a>
    </div>
  </section>

  <section class="section">
    <h2>互动 Demo</h2>
    <div class="sub">与 AI 协作完成的可玩作品，佐证动手落地能力</div>
    <div class="cards">
      <a class="card" href="hand-particles.html">
        <span class="tag">互动视觉</span>
        <h3>手势粒子宇宙</h3>
        <p>摄像头手势识别驱动的粒子交互实验（需授权摄像头）。</p>
      </a>
      <a class="card" href="spider-world/">
        <span class="tag">网页游戏</span>
        <h3>蜘蛛大世界</h3>
        <p>物理摆荡玩法的网页小游戏。</p>
      </a>
    </div>
  </section>

  <footer>最近更新：2026-07-21 · <a href="mailto:quankeying540@gmail.com">quankeying540@gmail.com</a></footer>
</div>
</body>
</html>
```

- [ ] **Step 3: 本地启动静态服务验证**

Run: `cd <工作目录> && python3 -m http.server 8090`（后台）→ 浏览器打开 `http://localhost:8090/`
Expected: 深色首页渲染正常；4 张卡片可见；两张 demo 卡点击可达已有作品；375px 宽度无横向滚动

- [ ] **Step 4: Commit**

```bash
git add assets/portfolio.css index.html
git commit -m "feat: 作品集首页与共享样式"
```

---

### Task 2: 作品子页 · AI 质检工作流

**Files:**
- Create: `works/ai-qc-workflow.html`

**Interfaces:**
- Consumes: `../assets/portfolio.css` 的 `page / back-link / work-body / table-wrap` 类
- Produces: 首页卡片链接目标 `works/ai-qc-workflow.html`

- [ ] **Step 1: 用统一骨架转换源文档**

骨架（两个子页通用，仅 `<title>` 与正文不同）：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI 短剧成片自动化质检工作流 | 权可莹</title>
<link rel="stylesheet" href="../assets/portfolio.css">
</head>
<body>
<div class="page">
  <a class="back-link" href="../index.html">← 返回作品集</a>
  <article class="work-body">
    <!-- 正文：由源 md 按映射规则逐节转换 -->
  </article>
</div>
</body>
</html>
```

转换映射（保真，不增删事实）：`# → h1`，`## → h2`，`> 引用行 → blockquote`，列表 → `ul/ol`，`**x**` → `<strong>`，表格 → `<div class="table-wrap"><table>…</table></div>`。源文件：`/Users/apple/claude/作品集/案例一页-AI质检工作流.md`（30 行，含"问题/方案/结果/我在其中的角色"四节与文末脱敏声明，全部保留）。

- [ ] **Step 2: 浏览器验证**

打开 `http://localhost:8090/works/ai-qc-workflow.html`
Expected: 返回链接可回首页；四节齐全；数字加粗高亮；375px 无横滚

- [ ] **Step 3: Commit**

```bash
git add works/ai-qc-workflow.html
git commit -m "feat: 作品页——AI质检工作流案例"
```

---

### Task 3: 作品子页 · 内容质量标准

**Files:**
- Create: `works/content-quality.html`

**Interfaces:**
- Consumes: 同 Task 2 骨架与映射规则
- Produces: 首页卡片链接目标 `works/content-quality.html`

- [ ] **Step 1: 转换源文档**

`<title>短剧内容质量判断标准 | 权可莹</title>`；源文件：`/Users/apple/claude/作品集/方法论一页-内容质量标准.md`（28 行，三节：评分卡表格 / 节奏公式 / 避雷清单）。评分卡表格必须包 `table-wrap`（5 行 3 列，手机端横滚）。

- [ ] **Step 2: 浏览器验证**

打开 `http://localhost:8090/works/content-quality.html`
Expected: 表格在 375px 下容器内横滚、页面本身无横滚；三节齐全

- [ ] **Step 3: Commit**

```bash
git add works/content-quality.html
git commit -m "feat: 作品页——内容质量标准方法论"
```

---

### Task 4: README 更新

**Files:**
- Modify: `README.md`（顶部加作品集入口，保留原有内容结构）

- [ ] **Step 1: 在 README 顶部加入**

```markdown
# 权可莹 · 作品集

**👉 在线访问：https://keyingquan.github.io/hello-world/**

AI短剧生产策略与平台运营 · 专业作品 + 互动 Demo
```

（README 其余既有作品表保留；如已有重复表述则合并为一处。）

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: README 指向作品集首页"
```

---

### Task 5: 终检、发布与线上验收

- [ ] **Step 1: 敏感信息终检**

Run: `grep -rn -e '业绩' -e '面试' -e '内部' -e '客户' index.html works/`
Expected: 仅命中"脱敏声明"类合法文案；如命中其他内容，停下来向用户确认

- [ ] **Step 2: Push**

```bash
git push origin main
```
（用户走代理，失败最多重试 3 次）

- [ ] **Step 3: 线上验收（浏览器实测）**

打开 `https://keyingquan.github.io/hello-world/`（Pages 构建约 1 分钟，未生效则稍候重试）
Expected（对应 spec 验收标准）：
1. 首页头部/两区/页脚齐全，定位一眼可读
2. 4 张卡片 + 返回链接线上逐一点通
3. 切 375px 宽度：无横向滚动，表格容器内横滚
4. 两个作品页排版统一

- [ ] **Step 4: 关闭本地服务，向用户交付线上链接**
