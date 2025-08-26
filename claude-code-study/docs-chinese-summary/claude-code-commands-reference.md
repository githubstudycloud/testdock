# Claude Code 完整命令参考手册

## 📋 目录
- [斜杠命令 (Slash Commands)](#斜杠命令-slash-commands)
- [内置工具 (Built-in Tools)](#内置工具-built-in-tools)
- [CLI 命令行参数](#cli-命令行参数)
- [自定义命令](#自定义命令)
- [配置文件](#配置文件)
- [使用示例](#使用示例)

---

## 斜杠命令 (Slash Commands)

### 核心系统命令

| 命令 | 功能描述 | 使用示例 | 适用场景 |
|------|---------|---------|---------|
| `/help` | 显示所有可用命令及说明 | `/help` | 需要查看命令列表时 |
| `/model` | 切换AI模型 (Opus/Sonnet/Haiku) | `/model opus` | 需要更强大或更快速的模型 |
| `/settings` | 配置Claude Code设置 | `/settings` | 调整权限、模型、token限制等 |
| `/bug` | 报告问题或反馈 | `/bug 发现了一个错误` | 遇到问题需要报告时 |
| `/exit` 或 `/quit` | 退出Claude Code | `/exit` | 结束会话 |
| `/plan` | 进入计划模式 | `/plan` | 需要规划复杂任务时 |
| `/bashes` | 查看后台运行的shell任务 | `/bashes` | 管理后台进程 |
| `/hooks` | 配置钩子函数 | `/hooks` | 设置编辑前后的自动化操作 |
| `/install-github-app` | 安装GitHub应用 | `/install-github-app` | 自动化PR审查 |
| `/init` | 初始化项目上下文 | `/init` | 开始新项目时 |
| `/allowed-tools` | 管理工具权限 | `/allowed-tools` | 控制可用工具 |
| `/resume` | 恢复之前的会话 | `/resume` | 继续上次的工作 |

### 高级命令选项

| 命令变体 | 功能 | 说明 |
|---------|------|------|
| `/model` | 显示当前模型 | 不带参数时显示当前使用的模型 |
| `/model opus` | 切换到Opus模型 | 最强大但较慢 |
| `/model sonnet` | 切换到Sonnet模型 | 平衡性能和速度 |
| `/model haiku` | 切换到Haiku模型 | 最快但能力有限 |
| `/settings maxTokens` | 设置最大token数 | 控制响应长度 |
| `/settings permissions` | 设置权限 | 控制工具访问权限 |

---

## 内置工具 (Built-in Tools)

### 文件操作工具

| 工具名 | 功能 | 参数 | 使用示例 | 注意事项 |
|--------|------|------|---------|----------|
| **Read** | 读取文件内容 | `file_path`, `limit`, `offset` | 读取 `src/main.js` | 修改前必须先读取 |
| **Write** | 创建新文件 | `file_path`, `content` | 创建配置文件 | 会覆盖已存在文件 |
| **Edit** | 编辑文件 | `file_path`, `old_string`, `new_string`, `replace_all` | 修改代码片段 | 需要精确匹配文本 |
| **MultiEdit** | 批量编辑 | `file_path`, `edits[]` | 多处修改 | 比多次Edit更高效 |
| **NotebookEdit** | 编辑Jupyter笔记本 | `notebook_path`, `cell_id`, `new_source` | 修改notebook单元格 | 支持.ipynb文件 |

### 搜索工具

| 工具名 | 功能 | 参数 | 使用示例 | 特点 |
|--------|------|------|---------|------|
| **Grep** | 正则搜索 | `pattern`, `path`, `glob`, `type`, `-A/-B/-C`, `-i`, `-n` | 搜索函数定义 | 使用ripgrep引擎 |
| **Glob** | 文件模式匹配 | `pattern`, `path` | 找所有JS文件 | 支持通配符 |
| **LS** | 列出目录 | `path`, `ignore[]` | 查看项目结构 | 需要绝对路径 |

### 系统工具

| 工具名 | 功能 | 参数 | 使用示例 | 高级选项 |
|--------|------|------|---------|----------|
| **Bash** | 执行shell命令 | `command`, `timeout`, `run_in_background` | 运行测试、构建 | 支持后台运行 |
| **BashOutput** | 获取后台输出 | `bash_id`, `filter` | 查看长时间任务输出 | 配合后台任务使用 |
| **KillBash** | 终止后台任务 | `shell_id` | 停止运行中的任务 | 管理后台进程 |
| **Task** | 启动专门代理 | `subagent_type`, `prompt`, `description` | 处理复杂任务 | 多种代理类型 |

### 任务管理工具

| 工具名 | 功能 | 参数 | 使用场景 | 状态选项 |
|--------|------|------|---------|----------|
| **TodoWrite** | 写入任务列表 | `todos[]` (content, status, activeForm) | 管理复杂项目 | pending/in_progress/completed |
| **TodoRead** | 读取任务列表 | 无 | 查看当前任务 | - |

### 网络工具

| 工具名 | 功能 | 参数 | 使用示例 | 限制 |
|--------|------|------|---------|------|
| **WebFetch** | 获取网页内容 | `url`, `prompt` | 获取文档、API响应 | 可能需要代理 |
| **WebSearch** | 网络搜索 | `query`, `allowed_domains`, `blocked_domains` | 搜索最新信息 | 仅美国可用 |

---

## CLI 命令行参数

### 启动选项

| 参数 | 简写 | 功能 | 使用示例 |
|------|------|------|----------|
| `--continue` | `-c` | 继续上次会话 | `claude -c` |
| `--resume` | - | 选择历史会话恢复 | `claude --resume` |
| `--prompt` | `-p` | 无头模式执行 | `claude -p "修复bug"` |
| `--output-format` | - | 输出格式 | `claude --output-format stream-json` |
| `--dangerously-skip-permissions` | - | 跳过权限提示 | `claude --dangerously-skip-permissions` |
| `--help` | `-h` | 显示CLI帮助 | `claude --help` |
| `--version` | `-v` | 显示版本信息 | `claude --version` |

### 环境变量

| 变量名 | 作用 | 示例值 |
|--------|------|--------|
| `CLAUDE_API_KEY` | API密钥 | `sk-xxx...` |
| `CLAUDE_MODEL` | 默认模型 | `opus` |
| `HTTP_PROXY` | HTTP代理 | `http://proxy:8080` |
| `HTTPS_PROXY` | HTTPS代理 | `http://proxy:8080` |

---

## 自定义命令

### 创建自定义斜杠命令

#### 1. 项目级命令
```markdown
# 位置：.claude/commands/deploy.md

Deploy the application to production

Arguments: $ENVIRONMENT

Steps:
1. Run tests
2. Build the application  
3. Deploy to $ENVIRONMENT
```

#### 2. 用户级命令
```markdown
# 位置：~/.claude/commands/review.md

Review code changes

!git diff --cached
Please review these changes for:
- Code quality
- Potential bugs
- Performance issues
```

### 命令语法特性

| 特性 | 语法 | 示例 | 说明 |
|------|------|------|------|
| 参数传递 | `$ARGUMENTS` | `/deploy prod` → `$ARGUMENTS = prod` | 接收用户输入 |
| Bash执行 | `!command` | `!git status` | 执行前运行bash |
| 文件引用 | `@filepath` | `@src/config.js` | 包含文件内容 |
| 命名空间 | 目录结构 | `git/commit.md` → `/git:commit` | 组织相关命令 |

---

## 配置文件

### 配置文件层级

| 文件路径 | 优先级 | 作用域 | 用途 |
|----------|--------|--------|------|
| `~/.claude/settings.json` | 低 | 用户级 | 全局默认设置 |
| `.claude/settings.json` | 中 | 项目级 | 项目特定设置 |
| `.claude/settings.local.json` | 高 | 本地级 | 本地覆盖（不提交） |

### settings.json 示例

```json
{
  "model": "opus",
  "maxTokens": 4096,
  "permissions": {
    "allowedTools": [
      "Read", "Write", "Edit", "MultiEdit",
      "Grep", "Glob", "LS",
      "Bash", "Task", "TodoWrite",
      "WebFetch", "WebSearch"
    ],
    "allowedBashCommands": [
      "git *",
      "npm *",
      "python *"
    ]
  },
  "hooks": {
    "afterEdit": {
      "*.py": "black {file}",
      "*.js": "prettier --write {file}"
    },
    "beforeCommit": "npm test"
  }
}
```

### CLAUDE.md 记忆文件

| 节点 | 内容 | 示例 |
|------|------|------|
| 项目信息 | 名称、技术栈、架构 | `技术栈：React + Node.js` |
| 开发规范 | 代码风格、命名规则 | `使用camelCase命名` |
| 常用命令 | 构建、测试、部署 | `npm run build` |
| API密钥 | 敏感信息（需.gitignore） | `API_KEY=xxx` |
| 特殊说明 | 项目特定注意事项 | `注意数据库连接池配置` |

---

## 使用示例

### 基础操作流程

```bash
# 1. 启动Claude Code
$ claude

# 2. 初始化项目上下文
> /init

# 3. 查看可用命令
> /help

# 4. 切换到强大模型
> /model opus

# 5. 执行任务
> 请帮我创建一个用户认证模块

# 6. 查看后台任务
> /bashes

# 7. 保存会话退出
> /exit
```

### 复杂任务示例

```bash
# 使用任务管理
> 创建一个完整的REST API，包含用户管理、认证和数据CRUD操作

# Claude会自动使用TodoWrite创建任务列表：
# 1. 设计API结构
# 2. 创建数据模型
# 3. 实现认证中间件
# 4. 开发CRUD端点
# 5. 添加错误处理
# 6. 编写测试
# 7. 创建文档
```

### Git工作流示例

```bash
# 提交代码
> 提交我的更改，使用常规提交格式

# Claude会执行：
# 1. git status - 查看更改
# 2. git diff - 分析修改
# 3. git add - 暂存文件
# 4. git commit - 创建提交
```

### 高级搜索示例

```bash
# 搜索特定模式
> 找出所有包含async函数的JavaScript文件

# Claude使用Grep工具：
# grep "async function" --type js
```

---

## 工具权限矩阵

| 工具类别 | 默认权限 | 可配置项 | 安全建议 |
|---------|---------|---------|---------|
| 文件读取 | ✅ 允许 | 路径限制 | 限制敏感目录 |
| 文件写入 | ⚠️ 需确认 | 文件类型限制 | 禁止系统文件 |
| 命令执行 | ⚠️ 需确认 | 命令白名单 | 仅允许安全命令 |
| 网络访问 | ✅ 允许 | 域名限制 | 限制外部API |
| 任务代理 | ✅ 允许 | 代理类型 | 按需启用 |

---

## 故障排查

### 常见问题及解决方案

| 问题 | 原因 | 解决方法 | 预防措施 |
|------|------|---------|---------|
| 命令无响应 | API连接问题 | 检查网络和代理 | 配置稳定代理 |
| 权限被拒绝 | 工具未授权 | 使用`/allowed-tools` | 预先配置权限 |
| 文件编辑失败 | 未先读取 | 先Read再Edit | 遵循工作流程 |
| 后台任务丢失 | 会话中断 | 使用`/bashes`查看 | 定期检查状态 |
| 模型切换失败 | 配额限制 | 检查API限制 | 合理使用配额 |

---

## 最佳实践建议

### 命令使用优先级

1. **优先使用内置工具**：比bash命令更安全高效
2. **批量操作优先**：MultiEdit > 多次Edit
3. **精确搜索优先**：Grep with type > 通用搜索
4. **任务管理优先**：复杂任务用TodoWrite

### 安全配置建议

1. **最小权限原则**：仅授予必要的工具权限
2. **命令白名单**：限制可执行的bash命令
3. **敏感信息隔离**：使用CLAUDE.md + .gitignore
4. **定期审查权限**：检查settings.json配置

---

*文档版本：2024.12*
*基于Claude Code最新版本编写*
*持续更新中...*