[git提交规范-CSDN博客](https://blog.csdn.net/geng1025/article/details/139761126)





下面的内容为你整理了**Git 官方文档**和来自社区的**权威资料**，并结合实际经验，总结出一份相对全面的 Git 提交规范及示例。需要特别说明的是：**Git 自身并没有强制的“官方”提交格式规范**，只有关于如何使用 `git commit` 的命令说明；提交信息的结构和风格通常是社区或组织（如 Linux Kernel、Conventional Commits 等）在实践中逐渐形成的“惯例”或“最佳实践”。以下信息将尽量引用**官方来源**或**业内广泛采用**的权威文档，并给出示例。

---

# 一、Git 官方资源

1. **Git 官方命令参考 (git-scm.com/docs)**  
   
   - [git commit 官方手册](https://git-scm.com/docs/git-commit)  
     这个手册主要描述了如何使用 `git commit` 命令（如 -m、-a、-v、--amend 等参数），并简要说明了提交消息编写的基础。  
   - 由于官方手册本身更关注“命令的使用方法”，所以并没有提供太多关于“提交信息书写规范”或“风格指南”的建议。

2. **Pro Git 电子书**  
   
   - [Pro Git（第二版）](https://git-scm.com/book/en/v2)（作者：Scott Chacon & Ben Straub，官方推荐）  
   - 这本书在**第 5 章 分布式 Git**、**第 7 章 Git 工具**等章节中会谈到提交历史和提交的最佳实践，但也同样不会强制特定的提交信息结构。  
   - Pro Git 提到了一些常见的好习惯，如“在提交信息中说明理由、保持提交原子性、写好补丁说明”等。

3. **The Git Community Book**  
   
   - [The Git Community Book](https://git-community-book.net/)（已不再频繁更新，但可供参考）  
   - 同样没有非常正式的提交规范指南，更多是操作说明和示例。

> 结论：**“Git 官方”其实只提供了命令级别的文档，对“如何写提交信息”没有做硬性规定**。因此我们更多需要参考“社区最佳实践”或“项目自身的贡献指南（Contribution Guide）”来编写提交。

---

# 二、社区最佳实践与权威资料

下面列举若干**社区常用或权威度较高**的提交信息规范与示例。  

## 1. Linux Kernel 提交信息规范

- **Linux 内核**是 Git 的诞生地（由 Linus Torvalds 最初编写），其提交信息风格具备较高的参考价值。  
- 官方文档：[Submitting Patches: 2) Describe your changes](https://github.com/torvalds/linux/blob/master/Documentation/process/submitting-patches.rst#2-describe-your-changes)（在该文档中搜索 `2) Describe your changes` 段落）  
- 核心要点：  
  1. 提交信息第一行为简短的说明（推荐不超过 50 个字符），用来快速概括变动。  
  2. 接下来空一行，然后在正文部分详细阐述变动原因、实现方式、可能的影响等。  
  3. 如果有“签名”（Signed-off-by）、“链接到相关讨论/Issue”等信息，则会放在正文后或者结尾处。  

一个典型示例可能是：

```
drm: Fix memory leak in intel_modeset_init()

In intel_modeset_init(), the allocated memory for X is not freed
in case of Y. This patch fixes that by [...]

Signed-off-by: Some Developer <developer@example.com>
```

## 2. Conventional Commits 约定式提交规范

- **Conventional Commits**：[官方地址](https://www.conventionalcommits.org/)  

- 这是目前前端/全栈/开源社区**最流行**的提交规范之一，旨在使提交日志**语义化**、**结构化**，并能配合工具自动生成变更日志 (changelog) 和版本号 (semantic versioning)。  

- 基本格式：
  
  ```
  <type>(<scope>): <subject>
  
  <body>
  
  <footer>
  ```
  
  - **type**：如 `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `build`, `ci` 等  
  - **scope**：可选，用于指定改动所在的模块或文件  
  - **subject**：简要描述  
  - **body**：详细描述  
  - **footer**：关联 Issue 或标注 BREAKING CHANGE  

示例：

```
feat(parser): 支持多文件同时解析

解析时会基于目录结构进行多线程处理，提高 30% 的性能。

Closes #123
```

## 3. Udacity / Tim Pope / Chris Beams 等常见写作风格

社区还流传很多**非官方却被广泛推荐**的写作规范总结，例如：  

- [Chris Beams: How to Write a Git Commit Message](https://cbea.ms/git-commit/)  
- [Tim Pope: A git commit message style guide](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)  
- [Udacity: Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)  

这些指南大都强调：  

1. 第一行简短摘要（建议 <= 50 个字符），首字母大写，结尾不加句号。  
2. 摘要和正文之间留一行空行。  
3. 正文每行不超过 72 个字符，详细解释**改动动机**、**实现思路**以及**与之前版本的差别**。  
4. 如果需要列举，可用项目符号 (bullet point)；如需引用 Bug ID / Issue ID / PR ID，也可在正文或脚注中提及。  

**示例**：

```
Add support for multi-file parsing

Parsing now uses a directory-based approach, enabling parallel
processing of multiple files. This improves performance by roughly
30% in our tests.

Signed-off-by: Jane Developer <dev@example.com>
```

---

# 三、综合性提交规范示例

下方示例参考了**Conventional Commits**和**Linux 内核风格**等做法，既**结构化**又**简洁**，适用于大多数团队/项目场景：

## 1. 提交信息结构

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

- **`<type>`**：必填，用来说明本次提交的类别。例如：
  - `feat`：新功能  
  - `fix`：修复 Bug  
  - `docs`：文档修改  
  - `style`：纯粹的格式改动 (空格、缩进、逗号等)  
  - `refactor`：代码重构  
  - `perf`：性能优化  
  - `test`：添加或修改测试用例  
  - `build`：构建过程或依赖的变更  
  - `ci`：持续集成（CI）配置或脚本的变更  
  - `chore`：不影响源代码或测试的零碎改动 (例如脚本、配置等)  
- **`(<scope>)`**：可选，用来说明影响的范围，通常是模块名、文件夹、功能点等。  
- **`<subject>`**：简要描述改动，不超过 50 个字符，开头首字母尽量大写。  
- **`<body>`**：可以用一到多段文字描述**改动动机**和**实现思路**。如需列出具体改动点，可以使用要点或编号。  
- **`<footer>`**：主要用于：
  1. **关联问题或关闭 Issue**（如 `Closes #123`）  
  2. **说明破坏性变更**（Breaking Change）  
  3. 贡献者签名 (如 Linux 内核的 `Signed-off-by`)  

### 示例：修复 bug

```
fix(user-auth): 修复登录时的 Token 过期问题

- 原因：后台 Token 验证中间件逻辑错误，导致旧 Token 在特定场景不会被判定为过期。
- 解决：调整中间件中 Token 解析方法，正确读取并判断过期时间。
- 影响：原有用户无感知，但请注意清理缓存，避免测试环境出现混乱。

Closes #542
```

### 示例：新增功能

```
feat(upload): 新增多文件同时上传功能

通过并行处理队列并配合批量压缩策略，有效减少上传次数，节约带宽与服务器计算资源。
当前仅对图片类型做了多文件支持，视频和其他类型后续再补充。

Closes #110, #112
```

### 示例：重构

```
refactor(api): 提取公共请求逻辑至独立模块

将原先散落在各子模块中的 HTTP 请求逻辑集中封装在 utils/request.js 中，
减少了重复代码，使后续添加新接口时更易维护。
```

### 示例：文档修改

```
docs(README): 更新快速开始章节

- 新增 Docker 环境部署示例
- 修正部分命令的拼写错误
```

---

# 四、提交规范的综合建议

1. **保持一致性**  
   - 不同项目或团队可使用不同风格，但务必在项目协作中保持统一。  
2. **简明扼要**  
   - `subject`（标题行）应在 50 个字符内，开门见山。  
   - 如需详细分析原因或改动方案，可以放在 `body`，并控制在 72~100 字符以内换行。  
3. **结合 Issue / Ticket**  
   - 提交时若对应 Jira、GitHub Issue 等工单系统，可在 footer 中标注，如 `Closes #123`。  
4. **注意原子性 (Atomic Commits)**  
   - 一个提交尽量只解决一件事（一个功能、一个 Bug、一次重构）。避免“打包提交”导致后续回溯困难。  
5. **多使用 `git commit`（不带 -m）**  
   - 这样会自动打开编辑器，便于编写分段、格式化良好的提交信息。  
6. **提交前检查 (lint)**  
   - 如果使用 [commitlint](https://github.com/conventional-changelog/commitlint) 等工具，可以在提交前校验信息格式是否符合团队规范。

---

# 五、参考链接与阅读资料

1. **Git 官方文档**  
   
   - [git-commit(1) Manual Page](https://git-scm.com/docs/git-commit)  
   - [Pro Git 电子书](https://git-scm.com/book/en/v2)

2. **Linux Kernel**  
   
   - [Submitting Patches (官方内核文档)](https://github.com/torvalds/linux/blob/master/Documentation/process/submitting-patches.rst)

3. **Conventional Commits**  
   
   - [官方站点](https://www.conventionalcommits.org/)

4. **社区常见写作风格指南**  
   
   - [Chris Beams: How to Write a Git Commit Message](https://cbea.ms/git-commit/)  
   - [Tim Pope: A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)  
   - [Udacity Git Commit Style Guide](https://udacity.github.io/git-styleguide/)

---

## 结语

- **Git 官方**只给出如何使用命令的说明，并没有统一的“提交信息风格”强制要求。  
- 想要更好地组织提交、自动生成变更日志或进行版本管理，就需要**借助社区规范**或**项目自身的贡献指南**。  
- 上述示例和建议结合了主流的写作风格与社区实践，可以作为你的“最全”综合参考。  
- 在团队内，**最好形成统一的提交约定**并落到实际执行中，这样协作和回溯都会更加高效、清晰。  
