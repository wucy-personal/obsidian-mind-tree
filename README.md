<div align="center">

# 思维树 · Mind Tree

**AI 驱动的思维导图笔记，让每次操作都激发深度思考，知识如树生长。**

Vue 3 · TypeScript · Obsidian Plugin

[使用手册](https://flowus.cn/jzcy/share/4ce27dd8-615f-4398-aef4-eb0dce751249?code=C5ZPWU) · [反馈建议](https://flowus.cn/jzcy/share/3f2d9d16-0085-43b1-9b78-c721a1574d82?code=C5ZPWU) · [Bilibili](https://space.bilibili.com/481011622)

</div>

---

## 这是什么

**思维树（Mind Tree）** 是一款 [Obsidian](https://obsidian.md) 插件，把**思维导图**与**主题笔记**深度绑定在一起：

- 导图作为**知识骨架**：层级分类本身即是思考路径，让"如何得到知识"也被保存下来。
- 主题承载**信息内容**：每个主题对应一份独立 Markdown 文档，点击即达。
- AI 辅助**深度思考**：把繁杂的笔记自动归位、按知识用途分类、按主题对话扩展。

它不是又一款"画图工具"，而是把你的笔记仓库变成一棵**会生长的知识树**。

---

## 核心功能

### 一、思维树编辑器

- **专属** **`.mtf`** **文件格式**：思维树以 `0_思维树.mtf`（根）和 `_思维树.mt`（链接锚点）形式存在，注册了 Obsidian 自定义视图类型 `mind-tree-file-view`。
- **基于 \[simple-mind-map]**（项目内置修复版 fork）：支持拖拽、搜索、彩虹连线、键盘导航、导图布局 Pro、导出、主题。
- **每视图独立 Vue 应用**：每个 Tab 创建独立的 Vue 3 实例，互不干扰。
- **深色 / 浅色主题**：与 Obsidian 主题联动，提供"思维树默认深色 / 浅色"两套内置主题。
- **防抖自动保存 + 串行写队列**：默认 10 秒间隔，焦点离开文档时也会触发保存，强制退出不会保存。

### 二、主题笔记与文件链接 UID 系统

- **双向索引**：通过 YAML frontmatter 的 `mind-tree-uid` 字段（或非 Markdown 文件的 `%uid%` / `@uid@` 文件名标记），建立思维树节点与实例笔记的双向关联。
- **打开方式可配置**：点击主题直接打开 / 点击主题后图标打开；可指定打开位置。
- **节点标记**：进度、优先级、卡片类型、境界、知识实体、高亮等多种标记，每种都可在设置中独立开关。
- **主题笔记模板**：支持配置默认内容，兼容 Templater 动态语法。
- **Frontmatter 可视化**：可自定义笔记属性的颜色、图标、优先级，导图节点上直接以标签形式呈现。

### 三、AI 助手（思维树 AI）

> 思维树 AI 当前处于**免费体验阶段**，未来将纳入高级会员服务。

#### 1. 节点级 AI 操作

选中任意主题，在右侧抽屉里选择**对话指令**或**自由对话**两种模式：

- **对话指令**：调用预设 / 自定义的指令模板（系统提示词 + 用户提示词），对当前主题笔记执行操作，并把结果保存为子主题。
- **自由对话**：与当前主题内容直接对话，可切换"仅当前主题 / 引用所有父级主题"作为上下文。
- **自定义对话指令**：可在设置中配置任意数量的 AI 指令（命令 ID、显示名称、提示词、图标、标签）。

#### 2. AI 主题分类（收件箱归位）

把 `10【收集】` 目录下的散乱笔记，按 `20【项目】` / `30【领域】` / `40【资源】` 下各主题的 `TOPIC.md` 描述，自动归位到对应主题。失败自动重试两次，结果写入笔记的 `【AI主题】` frontmatter 字段，附 `【AI主题_分类原因】`。

#### 3. AI 笔记任务（可扩展框架）

`AiPkmNoteTaskGenService` 提供了通用的任务框架，已内置：

- **知识用途分类**：把知识类笔记按六层框架（事实 / 结构 / 判断 / 方法 / 风险 / 应用）归类，结果写入 `【AI知识用途】` 字段。

新增任务（摘要、打标签、改写等）只需实现 `I_AiPkmNoteTask` 接口并注册即可。

#### 4. 自带 LLM 模型管理

- 兼容 **OpenAI 格式**的任意供应商（baseUrl + apiKey）。
- 支持配置多组模型，按场景选择（例如 `level1BaseModel` 用作分类等基础任务）。

### 四、导入与对接

- **GET 笔记导入**：通过 Token 从 GET 笔记批量导入到 Obsidian。
- **FlowUs 对接**：内置 `flowus-api-sdk`，支持与 FlowUs 数据互通（开发中）。
- **文件资源管理器右键菜单**：在任意文件夹右键即可"新建思维树"。

### 五、授权体系

- **高级会员授权码**：在「设置 → 高级会员」中输入授权码激活。
- **VIP 限制**：部分高级功能（如节点级 AI、AI 任务批量执行）有数量限制，需授权解除。

---

## 命令一览

通过 `Ctrl/Cmd + P` 唤起 Obsidian 命令面板可执行：

| 命令                     | 说明                                            |
| ------------------------ | ----------------------------------------------- |
| 新建思维树文件至根目录   | 在仓库根目录创建新的思维树链接文件              |
| 工具栏：打开             | 打开右侧思维树工具视图（导入、AI 任务、授权码） |
| 笔记内容：格式化         | 移除标题前后空行、移除所有分割线                |
| 笔记标题：转换为加粗     | 把 Markdown 标题转为加粗，去除标题样式          |
| 笔记标题：转换为 Callout | 把 Markdown 标题转为 Callout 块                 |
| 笔记标题：移除加粗       | 恢复被加粗的标题样式                            |

---

## 设置页（七大标签页）

设置页按功能分组到不同标签页：

- **基础设置**：未加入思维树笔记的处理方式、自动收集排除规则、是否自动关联至根主题。
- **思维脑图**：默认主题、打开时显示层级、标题换行宽度、默认只读模式、自动保存间隔、鼠标滚轮 / 左右键行为、主题标签数量与位置。
- **主题笔记**：打开方式与位置、默认内容、模板路径、节点标记开关、非 Markdown 文件关联标识符。
- **思维树 AI**：AI 助手总开关、LLM 模型列表、自定义对话指令、`level1BaseModel` 选择。
- **更多设置**：导入文件夹位置、GET 笔记 Token、日志等级、调试选项。
- **高级会员**：授权码管理与激活。
- **关于**：理念介绍、公众号、Bilibili、使用手册、反馈渠道。

---

## 安装

### 方式一：从 Obsidian 社区插件市场安装

（暂未上架，敬请期待）

### 方式二：手动安装

1. 从 [发布页](https://space.bilibili.com/481011622) 下载最新版 `main.js`、`manifest.json`、`styles.css`。
2. 在你的 Obsidian 仓库下创建目录 `.obsidian/plugins/mind-tree/`。
3. 把三个文件放进该目录。
4. 在 Obsidian 设置 → 第三方插件中启用「Mind Tree」。

---

## 开发

### 环境要求

- [Node.js](https://nodejs.org)（建议 LTS）
- [Obsidian](https://obsidian.md) ≥ 1.11.1，桌面版（**仅支持桌面端**，移动端暂不支持）

### 推荐编辑器

- [VSCode](https://code.visualstudio.com)
- [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)（Vue 3 语言支持）

### 常用命令

```bash
npm install            # 安装依赖
npm run dev            # 开发模式（热更新，输出 main.js + styles.css）
npm run build          # 发布构建（lint → test → typecheck → esbuild + 混淆）
npm test               # 运行 Jest 测试
npx tsc --noEmit       # 仅类型检查
npm run lint           # 跑全部 linter（prettier + eslint + style-scoped + dependency-cruiser）
npm run pack           # 构建并打包 zip
```

> 把本仓库克隆到 `.obsidian/plugins/obsidian-mind-tree-v1/` 下，`npm run dev` 后在 Obsidian 中重新加载插件即可热更新。

### 架构概览

源码按编号分层，由 `dependency-cruiser` 强制依赖方向（高层依赖低层，反向禁止）：

| 层  | 目录            | 职责                                                  |
| --- | --------------- | ----------------------------------------------------- |
| 10  | `10_types/`     | 类型定义、常量、枚举（基于 `enum-plus`）              |
| 20  | `20_config/`    | 配置、设置加载                                        |
| 30  | `30_repo/`      | 数据访问、LLM 请求、思维导图序列化                    |
| 40  | `40_service/`   | 业务服务（AI 分类、AI 任务、文件链接 UID 索引、授权） |
| 50  | `50_runtime/`   | 运行时：命令、图标、文件菜单                          |
| 60  | `60_ui/`        | Vue 组件、Obsidian 视图                               |
| 61  | `61_styles/`    | CSS / SCSS                                            |
| 70  | `70_providers/` | i18n、日志、消息提示                                  |
| 80  | `80_utils/`     | 纯工具函数                                            |

入口：[`src/main.ts`](src/main.ts)。详细约定见 [`CLAUDE.md`](CLAUDE.md)。

---

## 国际化

- 跟随 Obsidian 语言设置，目前内置**简体中文**与**English**。
- 清理冗余 i18n 键：`npm run i18n:cleanI18nKeys`。

---

## 联系我们

- **公众号**：瑾智初源
- **Bilibili**：[思维树·Mind Tree](https://space.bilibili.com/481011622)
- **使用手册**：[FlowUs 文档](https://flowus.cn/jzcy/share/4ce27dd8-615f-4398-aef4-eb0dce751249?code=C5ZPWU)
- **反馈建议**：[反馈通道](https://flowus.cn/jzcy/share/3f2d9d16-0085-43b1-9b78-c721a1574d82?code=C5ZPWU)
- **授权购买**：[开通 VIP](https://flowus.cn/jzcy/share/66cedaaa-a8b6-456b-b536-d0f341b467ed?code=C5ZPWU)

---

## 许可

MIT License · 作者：瑾智初源
