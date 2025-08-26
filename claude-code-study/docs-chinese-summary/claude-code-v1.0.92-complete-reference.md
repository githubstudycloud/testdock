# Claude Code v1.0.92 完整命令参考手册

## 📌 版本信息
- **版本**: Claude Code v1.0.92
- **更新日期**: 2024年12月
- **文档类型**: 官方命令完整列表

---

## 🚀 使用模式

### REPL模式（交互式）
```bash
claude                    # 启动交互式会话
```

### 非交互模式
```bash
claude -p "question"      # 单次执行命令
claude -h                 # 查看所有命令行选项
```

---

## 📋 完整命令列表（共40个命令）

### 🎯 常用任务示例
| 任务类型 | 示例命令 | 说明 |
|---------|---------|------|
| 询问代码库 | `How does foo.py work?` | 理解代码逻辑 |
| 编辑文件 | `Update bar.ts to...` | 修改代码 |
| 修复错误 | `cargo build` | 运行构建命令 |
| 运行命令 | `/help` | 执行斜杠命令 |
| 运行Bash | `!ls` | 执行shell命令 |

---

## 🔧 交互模式命令详解

### 📁 文件与目录管理

| 命令 | 功能 | 使用示例 | 新版特性 |
|------|------|---------|---------|
| `/add-dir` | 添加新的工作目录 | `/add-dir /path/to/project` | ✅ 多目录支持 |
| `/init` | 初始化CLAUDE.md文件 | `/init` | 生成代码库文档 |
| `/memory` | 编辑Claude记忆文件 | `/memory` | 管理项目记忆 |

### 🤖 AI模型与代理

| 命令 | 功能 | 使用示例 | 可选参数 |
|------|------|---------|---------|
| `/agents` | 管理代理配置 | `/agents` | 配置专门代理 |
| `/model` | 设置AI模型 | `/model opus` | opus/sonnet/haiku |
| `/mcp` | 管理MCP服务器 | `/mcp` | 🆕 新功能 |

### 💻 系统与环境

| 命令 | 功能 | 使用示例 | 重要性 |
|------|------|---------|---------|
| `/bashes` | 列出和管理后台bash | `/bashes` | ⭐⭐⭐⭐ |
| `/doctor` | 诊断安装和设置 | `/doctor` | ⭐⭐⭐⭐⭐ |
| `/status` | 显示状态信息 | `/status` | ⭐⭐⭐⭐⭐ |
| `/config` | 打开配置面板 | `/config` | ⭐⭐⭐⭐ |
| `/permissions` | 管理工具权限 | `/permissions` | ⭐⭐⭐⭐⭐ |

### 📊 会话与上下文管理

| 命令 | 功能 | 使用示例 | 特点 |
|------|------|---------|------|
| `/clear` | 清除对话历史 | `/clear` | 释放上下文 |
| `/compact` | 压缩历史保留摘要 | `/compact [instructions]` | 🆕 智能压缩 |
| `/context` | 可视化上下文使用 | `/context` | 彩色网格显示 |
| `/cost` | 显示成本和时长 | `/cost` | 💰 费用统计 |
| `/export` | 导出对话 | `/export` | 文件或剪贴板 |
| `/resume` | 恢复对话 | `/resume` | 继续之前会话 |

### 🔧 开发工具集成

| 命令 | 功能 | 使用示例 | 支持工具 |
|------|------|---------|----------|
| `/hooks` | 管理钩子配置 | `/hooks` | 工具事件钩子 |
| `/ide` | 管理IDE集成 | `/ide` | 🆕 IDE状态 |
| `/vim` | 切换编辑模式 | `/vim` | Vim/Normal模式 |
| `/output-style` | 设置输出样式 | `/output-style` | 自定义样式 |
| `/output-style:new` | 创建自定义样式 | `/output-style:new` | 🆕 样式创建 |
| `/statusline` | 设置状态栏UI | `/statusline` | 🆕 UI定制 |

### 🔀 Git与代码审查

| 命令 | 功能 | 使用示例 | GitHub集成 |
|------|------|---------|------------|
| `/install-github-app` | 设置GitHub Actions | `/install-github-app` | 自动化CI/CD |
| `/pr-comments` | 获取PR评论 | `/pr-comments` | 🆕 PR协作 |
| `/review` | 审查Pull Request | `/review` | 代码审查 |
| `/security-review` | 安全审查 | `/security-review` | 🔒 安全检查 |

### 👤 账户与认证

| 命令 | 功能 | 使用示例 | 账户管理 |
|------|------|---------|----------|
| `/login` | 切换Anthropic账户 | `/login` | 多账户支持 |
| `/logout` | 退出账户 | `/logout` | 安全登出 |
| `/upgrade` | 升级到Max版本 | `/upgrade` | 💎 高级功能 |

### 📝 帮助与反馈

| 命令 | 功能 | 使用示例 | 链接 |
|------|------|---------|------|
| `/help` | 显示帮助 | `/help` | 命令列表 |
| `/bug` | 提交反馈 | `/bug` | 问题报告 |
| `/release-notes` | 查看发布说明 | `/release-notes` | 🆕 版本更新 |

### 🔧 系统维护

| 命令 | 功能 | 使用示例 | 用途 |
|------|------|---------|------|
| `/migrate-installer` | 迁移安装方式 | `/migrate-installer` | npm全局→本地 |
| `/exit` | 退出REPL | `/exit` | 结束会话 |

---

## 📊 命令分类统计

| 类别 | 命令数量 | 重要命令 |
|------|---------|---------|
| 文件管理 | 3个 | `/init`, `/memory`, `/add-dir` |
| AI配置 | 3个 | `/model`, `/agents`, `/mcp` |
| 系统环境 | 5个 | `/status`, `/doctor`, `/permissions` |
| 会话管理 | 6个 | `/context`, `/cost`, `/resume` |
| 开发工具 | 6个 | `/ide`, `/vim`, `/hooks` |
| Git集成 | 4个 | `/review`, `/security-review` |
| 账户管理 | 3个 | `/login`, `/logout`, `/upgrade` |
| 帮助反馈 | 3个 | `/help`, `/bug`, `/release-notes` |
| 系统维护 | 2个 | `/migrate-installer`, `/exit` |

---

## 🆕 v1.0.92 新增功能

### 新增命令（标记为🆕）
1. **`/mcp`** - MCP服务器管理
2. **`/compact`** - 智能压缩对话历史
3. **`/ide`** - IDE集成管理
4. **`/output-style:new`** - 创建自定义输出样式
5. **`/statusline`** - 状态栏UI配置
6. **`/pr-comments`** - PR评论获取
7. **`/security-review`** - 安全审查功能
8. **`/release-notes`** - 版本说明查看

### 增强功能
- 📊 `/context` - 彩色网格可视化
- 💰 `/cost` - 详细费用统计
- 🔧 `/hooks` - 更强大的钩子系统
- 📁 `/add-dir` - 多目录支持

---

## 🎯 快速参考卡

### TOP 10 最常用命令
1. `/help` - 查看帮助
2. `/model` - 切换模型
3. `/status` - 查看状态
4. `/context` - 上下文使用
5. `/clear` - 清除历史
6. `/resume` - 恢复会话
7. `/exit` - 退出程序
8. `/init` - 初始化项目
9. `/review` - 代码审查
10. `/cost` - 费用统计

### 命令前缀速记
- `/` - 斜杠命令
- `!` - Bash命令
- `>` - 自然语言任务
- `@` - 文件引用

---

## 📚 官方资源

- **官方文档**: https://docs.anthropic.com/s/claude-code
- **GitHub仓库**: https://github.com/anthropics/claude-code
- **问题反馈**: 使用 `/bug` 命令
- **版本更新**: 使用 `/release-notes` 查看

---

## 💡 专业提示

### 效率技巧
1. **使用Tab补全**: 命令支持Tab自动补全
2. **组合使用**: 命令可以组合使用提高效率
3. **自定义样式**: 使用 `/output-style:new` 定制输出
4. **监控成本**: 定期使用 `/cost` 监控使用费用

### 最佳实践
1. 开始项目先 `/init` 创建CLAUDE.md
2. 使用 `/doctor` 诊断环境问题
3. 配置 `/permissions` 确保安全
4. 定期 `/compact` 压缩对话历史
5. 使用 `/hooks` 自动化工作流

---

*📅 更新日期: 2024年12月*
*🔖 版本: Claude Code v1.0.92*
*📝 基于官方help输出整理*