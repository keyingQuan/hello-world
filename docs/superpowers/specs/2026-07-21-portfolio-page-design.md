# 作品集展示页设计文档

日期：2026-07-21 ｜ 状态：已获用户确认

## 目标与受众

为权可莹求职「AI短剧生产策略与平台运营」岗位提供一个放进简历的公开作品集链接。核心受众是 HR 与面试官，成功标准：**3 分钟内看懂"她是谁、会什么、证据在哪"**。

## 范围

**做**：
- 新建 `index.html` 作为 GitHub Pages 默认首页（https://keyingquan.github.io/hello-world/）
- 将本地 4 份 markdown 文档转为同风格 HTML 子页面，置于 `works/`：
  1. 拉片报告：Step Aside / A+ Heiress（`/Users/apple/claude/作品集/拉片报告-StepAside-AplusHeiress.md`）→ `works/lapian-stepaside.html` ｜证明：内容拆解与方法论输出能力
  2. 案例一页：AI 质检工作流（`案例一页-AI质检工作流.md`）→ `works/ai-qc-workflow.html` ｜证明：AI 工作流设计与落地能力
  3. 方法论一页：内容质量标准（`方法论一页-内容质量标准.md`）→ `works/content-quality.html` ｜证明：内容质量体系化能力
  4. 剧本样张 · 写作模板（`剧本样张-写作模板.md`）→ `works/script-template.html` ｜证明：剧本创作规范能力
- 首页收录已有互动 Demo：hand-particles.html、spider-world/（定位"AI 协作动手能力佐证"）
- 新增共享样式 `assets/portfolio.css`
- 更新 README 指向新首页

**不做**：
- 不公开：面试作战卡×2、业绩数据登记表、剧本初稿
- 不改动已有 hand-particles.html、spider-world/ 及 assets 内既有文件
- 无 JS、无构建工具、无外部 CDN 依赖

## 页面结构

**首页** index.html，从上到下：
1. 头部：权可莹 · AI 短剧生产策略与平台运营 · quankeying540@gmail.com · GitHub 链接
2. 专业作品区：4 张卡片（标题 + 一句能力证明），链向 works/ 子页
3. 互动 Demo 区：2 张卡片
4. 页脚：更新日期 + 邮箱

**子页面**：统一排版（标题层级/表格/引用块），顶部「← 返回作品集」。

## 视觉

影视感深色风：深色背景 + 海报式卡片 + 单一强调色；响应式（手机优先可读，禁止横向滚动）；中文系统字体栈。

## 风险与处理

- **敏感信息**：4 份文档上线前逐份扫描（公司内部数据、真实业绩数字、同事姓名），命中先标注征询用户，脱敏后再发布。
- **首页变化**：加 index.html 后 Pages 根路径由 README 渲染页变为作品集首页（预期行为，已告知用户）。
- **网络**：用户走代理访问 GitHub，push 失败可重试。

## 验收标准

1. 首页 3 分钟传达定位与证据链
2. 线上实测所有链接可点（浏览器逐页验证，桌面 + 手机宽度）
3. 手机宽度无横向滚动
4. 4 份子页面排版统一、无敏感信息
