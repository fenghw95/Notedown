---
id: "f28ed6caf6cc58ac"
title: "Claude Code 完全指南：一文掌握的所有基础技巧"
url: "https://mp.weixin.qq.com/s/f-FeLEcIW7xnqcNhGuwbbQ"
favicon: "https://res.wx.qq.com/a/wx_fed/assets/res/NTI4MWU5.ico"
tags: ["claude", "code", "一文掌握的所有基础技巧", "完全指南", "所以将收藏的一篇英文的入门指南翻译了下"]
folder: "AI note for using"
createdAt: "2026-02-08T06:57:27.746Z"
updatedAt: "2026-02-08T06:57:33.739Z"
contentType: "markdown"
version: 1
byline: "三个茅庐"
siteName: "微信公众平台"
publishedTime: "2026-02-08T06:57:27.743Z"
---

> ###### 最近 GLM-4.7 和 MiniMax 2.1 等国产大模型进步明显，价格也更亲民。同时我也看到越来越多人开始使用 Claude Code，所以将收藏的一篇英文的入门指南翻译了下。之所以说是“入门”，是因为 Claude Code 真正玩起来并不局限于写代码：它还可以用来搭建各种自动化流程，比如自动写作、批量处理任务，甚至把它当作电脑上的 AI 助手来用。

### /init —项目初始化

`/init` 命令，Claude 会为给项目做初步扫描，提炼并概括项目的核心特征

Claude 会阅读你的代码库并生成一个 `CLAUDE.md` 文件，包含：

*   构建和测试命令
    
*   关键目录及其用途
    
*   代码约定和模式
    
*   重要的架构决策
    

对于大型项目，还可以创建 `.claude/rules/` 目录来存放模块化的主题特定说明。该目录中的每个 `.md` 文件都会与你的 `CLAUDE.md` 一起自动加载。你甚至可以使用 YAML frontmatter 根据文件路径有条件地应用规则：

```
--- paths: src/api/**/*.ts --- # API 开发规则 - 所有 API 端点必须包含输入验证 
```

`CLAUDE.md` 是大体概括项目，而 `.claude/rules/` 则是针对测试、安全、API 设计或其他值得单独成文的专项补充。

### Memory更新

直接和Claude对话就可以更新`CLAUDE.md`

直接告诉 Claude ：

> “更新 Claude.md：这个项目始终使用 bun 而不是 npm”

### @ 提及 — 即时添加上下文

`@` 提及是给 Claude 提供上下文的最快方式：

*   `@src/auth.ts` — 立即将文件添加到上下文
    
*   `@src/components/` — 引用整个目录
    
*   `@mcp:github` — 启用/禁用 MCP 服务器
    

在 git 仓库中，文件建议速度提升约3倍，并且支持模糊匹配。`@` 是从"我需要上下文"到"Claude 拥有上下文"的最快路径。

* * *

### ! 前缀 — 立即运行 Bash

`!` 前缀立即执行 bash 并将输出注入上下文。无需模型处理。无延迟。不浪费 tokens。不需要多个终端窗口。

只需输入 `!` 后跟你的 bash 命令：

```
! git status ! npm test ! ls -la src/ 
```

这个太方便了，以前我都是新开窗口，然后手动输入命令查看

### 双击 Esc 回退

按两次 `Esc` 就能跳回到一个干净的检查点。可以回退对话、代码更改，或两者都回退。但已运行的 Bash 命令无法撤销。

### Ctrl + R — 反向搜索

搜索历史prompt

<table><thead><tr><th><section><span leaf="">按键</span></section></th><th><section><span leaf="">操作</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">Ctrl+R</span></code></td><td><section><span leaf="">开始反向搜索</span></section></td></tr><tr><td><code><span leaf="">Ctrl+R</span></code><section><span leaf="">（再次）</span></section></td><td><section><span leaf="">循环匹配项</span></section></td></tr><tr><td><code><span leaf="">Enter</span></code></td><td><section><span leaf="">运行它</span></section></td></tr><tr><td><code><span leaf="">Tab</span></code></td><td><section><span leaf="">先编辑</span></section></td></tr></tbody></table>

### 提示词暂存

`Ctrl+S` 保存你的草稿。发送其他内容。准备好后，你的草稿会自动恢复。就像 `git stash`，但用于你的提示词。

### 提示词建议

Claude 可以预测你的下一个问题。

完成任务后，有时你会看到一个灰色的后续建议出现：

<table><thead><tr><th><section><span leaf="">按键</span></section></th><th><section><span leaf="">操作</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">Tab</span></code></td><td><section><span leaf="">接受并编辑</span></section></td></tr><tr><td><code><span leaf="">Enter</span></code></td><td><section><span leaf="">接受并立即运行</span></section></td></tr></tbody></table>

Tab 键过去用于自动补全代码。现在它自动补全你的工作流。通过 `/config` 切换此功能。

* * *

## 📁 会话管理

快速恢复最后一次对话和查看历史会话

```
claude --continue # 立即恢复你的最后对话 claude --resume # 显示选择器以选择任何过去的会话 
```

还可以通过 `cleanupPeriodDays` 设置自定义会话保留时长。默认为30天，但你可以设置任意长度，0表示不保留 Claude Code 会话。

### 命名会话

给会话改个更清晰的名字，方便快速回溯和查找历史对话。

```
/rename api-migration # 命名当前会话 /resume api-migration # 按名称恢复 claude --resume api-migration  # 也可以从命令行使用 
```

`/resume` 屏幕会将分叉的会话分组，并支持键盘快捷键：`P` 预览，`R` 重命名。

### Claude Code Remote

在网页上开始任务，在终端中完成：

```
# 在 claude.ai/code 上启动 Claude Code 会话 # 当你离开时它在后台运行 # 稍后，从你的终端： claude --teleport session_abc123 
```

这会拉取并在本地恢复会话。方便在Claude Code各个端切换

### /export — 导出记录

有时你需要记录发生了什么。

https://cdn.adocomplete.com/advent-of-claude-2025/day-24.mp4

`/export` 将你的整个对话导出为 markdown：

*   你发送的每个prompt
    
*   Claude 给出的每个响应
    
*   每个工具调用及其输出
    

* * *

### Vim 模式

输入 `/vim` 解锁完整的 vim 风格编辑：

<table><thead><tr><th><section><span leaf="">命令</span></section></th><th><section><span leaf="">操作</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">h j k l</span></code></td><td><section><span leaf="">导航</span></section></td></tr><tr><td><code><span leaf="">ciw</span></code></td><td><section><span leaf="">修改单词</span></section></td></tr><tr><td><code><span leaf="">dd</span></code></td><td><section><span leaf="">删除行</span></section></td></tr><tr><td><code><span leaf="">w b</span></code></td><td><section><span leaf="">按单词跳转</span></section></td></tr><tr><td><code><span leaf="">A</span></code></td><td><section><span leaf="">在行尾追加</span></section></td></tr></tbody></table>

快速编辑提示词。**适合vim玩家**

### /statusline — 自定义你的视图

Claude Code 在终端底部有一个可自定义的状态栏。

`/statusline` 可以配置：

*   Git 分支和状态
    
*   当前模型
    
*   Token 使用量
    
*   上下文窗口百分比
    
*   自定义脚本
    

### /context — 查看上下文消耗

输入 `/context` 可以查看：

*   系统提示大小
    
*   MCP 服务器提示
    
*   内存文件（CLAUDE.md）
    
*   加载的技能和代理
    
*   对话历史
    

### /stats — 你的使用仪表板

```
2023: "看看我的 GitHub 贡献图" 2025: "看看我的 Claude Code 统计数据" 
```

### /usage — 查看套餐使用限制

只要Claude Code官方套餐才有效

```
/usage → 用可视化进度条检查当前使用情况 /extra-usage  → 购买额外容量 
```

* * *

### Ultrathink

用一个关键词按需触发扩展思考：

```
> ultrathink: 为我们的 API 设计一个缓存层 
```

当你在提示词中包含 `ultrathink` 时，Claude 会在响应前分配多达32k tokens 用于内部推理。对于复杂的架构决策或棘手的调试会话，这可能是表面答案和真正洞察之间的区别。

注意：ultrathink 关键词仅在未设置 MAX\_THINKING\_TOKENS 时有效。设置 MAX\_THINKING\_TOKENS 后，它优先级更高并控制所有请求的思考tokens。

### Plan 模式

很好用的模式，当面对复杂功能时，可以先和Claude Code对话，它会一步步引导你完善需求

按两次 `Shift+Tab` 进入 Plan 模式。Claude 可以：

*   读取和搜索你的代码库
    
*   分析架构
    
*   探索依赖关系
    
*   起草实现计划
    

确认计划后，Claude Code才会开始实施代码

💡 **最佳实践**：对于复杂任务，先用 Plan 模式讨论3-5轮，确保方向正确后再执行。这能避免大量返工。

### 扩展思考（API）

直接使用 Claude API 时，你可以启用扩展思考来查看 Claude 的逐步推理：

```
thinking: { type: "enabled", budget_tokens: 5000 } 
```

Claude 在响应前会在思考块中显示其推理过程。对于调试复杂逻辑或理解 Claude 的决策很有用。

* * *

### Sandbox 模式

```
"我可以运行 npm install 吗？" [允许] "我可以运行 npm test 吗？" [允许] "我可以 cat 这个文件吗？" [允许] "我可以抚摸那只狗吗？" [允许] ×100 
```

`/sandbox` 让你一次性定义边界。避免Claude Code在实施代码的过程中一次一次手动确认。

你获得了速度和真正的安全性。最新版本支持通配符语法，如 `mcp__server__*` 用于允许整个 MCP 服务器。

### YOLO 模式

\--dangerously-skip-permissions 开启全自动模式

```
claude --dangerously-skip-permissions 
```

这个标志对所有事情说 yes。它的名字中有"danger"是有原因的——明智使用，最好在隔离环境或可信操作中。

⚠️ **警告**：建议在测试环境中使用，因为Claude Code可能把你整个项目都删掉

### Hooks

Hooks 是在预定生命周期事件运行的 shell 命令：

*   `PreToolUse` / `PostToolUse`：工具执行前后
    
*   `PermissionRequest`：自动批准或拒绝权限请求
    
*   `Notification`：响应 Claude 的通知
    
*   `SubagentStart` / `SubagentStop`：监控代理生成
    

通过 `/hooks` 或在 `.claude/settings.json` 中配置它们。

使用 hooks 来阻止危险命令、发送通知、记录操作或与外部系统集成。这是对概率性 AI 的确定性控制。

* * *

### Headless 模式

Claude Code 用作强大的 CLI 工具进行脚本和自动化：

```
claude -p "修复 lint 错误" claude -p "列出所有函数" | grep "async" git diff | claude -p "解释这些更改" echo "审查这个 PR" | claude -p --json 
```

AI 进入你的管道。`-p` 标志非交互式运行 Claude 并直接输出到 stdout。

💡 **CI/CD 集成示例**：

```
# 在 CI 中自动审查代码 git diff main | claude -p "Review this diff for security issues" 
```

### Commands — 可复用的prompt

将任何提示词保存到 markdown 文件，它就成为一个可以接受参数的斜杠命令：：

```
/daily-standup → 运行你的晨会例行提示 /explain $ARGUMENTS → /explain src/auth.ts 
```

* * *

## 浏览器集成

Claude Code 可以与浏览器交互。

### Claude Code + Chrome

*   导航页面
    
*   点击按钮和填写表单
    
*   读取控制台错误
    
*   检查 DOM
    
*   截图
    

* * *

### Subagents（子代理）

运行Claude后的窗口可以当做是主Agent，负责执行当前对话。而Subagents则独立主Agent运行，不占用主Agent上下文，适合运行独立的功能，既可以节约上下文，也可以加速任务执行

每个Subagents都：

*   获得自己的 200k 上下文窗口
    
*   执行专门任务
    
*   与其他并行运行
    
*   将输出合并回主代理
    

💡 **实际应用**：

```
主代理：重构核心架构 子代理1：更新所有测试 子代理2：更新文档 子代理3：检查向后兼容性 
```

### Agent Skills

Skills 是教 Claude 专业任务的指令、脚本和资源的文件夹。这个玩法就千人千面了，完全可以利用skill自助编排工作流。这里就不展开了

Skills可以打包一次，分享给别人使用（**由于Skills里允许包含可执行的代码，所以使用别人的Skills时一定要注意安全，网上已经爆出好几起Skills投毒行为**）

### Plugins

Plugins可以把一套“可复用的能力/工作流”打包起来，分享给不同的人用，比如将命令、代理、skills、hooks 和 MCP 服务器打包到一个包中

```
/plugin install my-setup 
```

### 语言服务器协议（LSP）集成

语言服务器协议（LSP）支持为 Claude 提供 IDE 级别的代码智能：

LSP 集成提供：

*   **即时诊断**：Claude 在每次编辑后立即看到错误和警告
    
*   **代码导航**：跳转到定义、查找引用和悬停信息
    
*   **语言感知**：代码符号的类型信息和文档
    

就像在 IDE 里一样，诊断代码。

### Claude Agent SDK

在Claude Code的基础上，封装了一层调用Claude Code CLI的框架，方便开发者快速调用Claude Code的能力，进行开发

```
import { query } from '@anthropic-ai/claude-agent-sdk'; for await (const msg of query({   prompt: "为 src/ 中所有公共函数生成 markdown API 文档",   options: {     allowedTools: ["Read", "Write", "Glob"],     permissionMode: "acceptEdits"   } })) {   if (msg.type === 'result')      console.log("文档已生成:", msg.result); } 
```

* * *

## 快速参考

### 键盘快捷键

<table><thead><tr><th><section><span leaf="">快捷键</span></section></th><th><section><span leaf="">操作</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">!command</span></code></td><td><section><span leaf="">立即执行 bash</span></section></td></tr><tr><td><code><span leaf="">Esc Esc</span></code></td><td><section><span leaf="">回退对话/代码</span></section></td></tr><tr><td><code><span leaf="">Ctrl+R</span></code></td><td><section><span leaf="">反向搜索历史</span></section></td></tr><tr><td><code><span leaf="">Ctrl+S</span></code></td><td><section><span leaf="">暂存当前提示词</span></section></td></tr><tr><td><code><span leaf="">Shift+Tab</span></code><section><span leaf="">&nbsp;(×2)</span></section></td><td><section><span leaf="">切换 plan 模式</span></section></td></tr><tr><td><code><span leaf="">Alt+P</span></code><section><span leaf="">&nbsp;/&nbsp;</span><code><span leaf="">Option+P</span></code></section></td><td><section><span leaf="">切换模型</span></section></td></tr><tr><td><code><span leaf="">Ctrl+O</span></code></td><td><section><span leaf="">切换详细模式</span></section></td></tr><tr><td><code><span leaf="">Tab</span></code><section><span leaf="">&nbsp;/&nbsp;</span><code><span leaf="">Enter</span></code></section></td><td><section><span leaf="">接受提示词建议</span></section></td></tr></tbody></table>

### 基础命令

<table><thead><tr><th><section><span leaf="">命令</span></section></th><th><section><span leaf="">用途</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">/init</span></code></td><td><section><span leaf="">为你的项目生成 CLAUDE.md</span></section></td></tr><tr><td><code><span leaf="">/context</span></code></td><td><section><span leaf="">查看 token 消耗</span></section></td></tr><tr><td><code><span leaf="">/stats</span></code></td><td><section><span leaf="">查看使用统计</span></section></td></tr><tr><td><code><span leaf="">/usage</span></code></td><td><section><span leaf="">检查速率限制</span></section></td></tr><tr><td><code><span leaf="">/vim</span></code></td><td><section><span leaf="">启用 vim 模式</span></section></td></tr><tr><td><code><span leaf="">/config</span></code></td><td><section><span leaf="">打开配置</span></section></td></tr><tr><td><code><span leaf="">/hooks</span></code></td><td><section><span leaf="">配置生命周期 hooks</span></section></td></tr><tr><td><code><span leaf="">/sandbox</span></code></td><td><section><span leaf="">设置权限边界</span></section></td></tr><tr><td><code><span leaf="">/export</span></code></td><td><section><span leaf="">导出对话为 markdown</span></section></td></tr><tr><td><code><span leaf="">/resume</span></code></td><td><section><span leaf="">恢复过去的会话</span></section></td></tr><tr><td><code><span leaf="">/rename</span></code></td><td><section><span leaf="">命名当前会话</span></section></td></tr><tr><td><code><span leaf="">/theme</span></code></td><td><section><span leaf="">打开主题选择器</span></section></td></tr><tr><td><code><span leaf="">/terminal-setup</span></code></td><td><section><span leaf="">配置终端集成</span></section></td></tr></tbody></table>

### CLI 标志

<table><thead><tr><th><section><span leaf="">标志</span></section></th><th><section><span leaf="">用途</span></section></th></tr></thead><tbody><tr><td><code><span leaf="">-p "prompt"</span></code></td><td><section><span leaf="">Headless/print 模式</span></section></td></tr><tr><td><code><span leaf="">--continue</span></code></td><td><section><span leaf="">恢复最后会话</span></section></td></tr><tr><td><code><span leaf="">--resume</span></code></td><td><section><span leaf="">选择要恢复的会话</span></section></td></tr><tr><td><code><span leaf="">--resume name</span></code></td><td><section><span leaf="">按名称恢复会话</span></section></td></tr><tr><td><code><span leaf="">--teleport id</span></code></td><td><section><span leaf="">恢复 web 会话</span></section></td></tr><tr><td><code><span leaf="">--dangerously-skip-permissions</span></code></td><td><section><span leaf="">YOLO 模式</span></section></td></tr></tbody></table>

* * *

## 参考资料：

https://adocomplete.com/advent-of-claude-2025/

https://code.claude.com/docs/en/skills

https://github.com/anthropics/claude-code/blob/main/plugins/README.md
