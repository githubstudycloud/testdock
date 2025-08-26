# Claude Code 完整文档

## 概述

Claude Code 是 Anthropic 开发的智能编码助手工具，它：
- 运行在终端环境中
- 理解你的代码库
- 通过自然语言命令帮助你更快地编码
- 可以执行常规任务、解释复杂代码、处理 git 工作流

## 安装与启动

### 系统要求
- Node.js 18+ 
- npm 或其他包管理器

### 安装命令
```bash
npm install -g @anthropic-ai/claude-code
```

### 启动
在项目目录中运行：
```bash
claude
```

## 核心功能

### 1. 代码理解与分析
- 读取和理解项目结构
- 分析代码依赖关系
- 解释复杂代码逻辑
- 提供代码建议和优化方案

### 2. 代码编写与修改
- 自动生成代码
- 修改现有代码
- 重构代码结构
- 添加注释和文档

### 3. Git 工作流支持
- 创建提交信息
- 管理分支
- 处理合并冲突
- 创建 Pull Request

### 4. 项目管理
- 文件操作（创建、修改、删除）
- 目录管理
- 项目配置
- 依赖管理

## 内置工具

### 文件操作工具
- **Read**: 读取文件内容
- **Write**: 创建新文件
- **Edit**: 编辑现有文件
- **MultiEdit**: 批量编辑文件

### 搜索工具
- **Grep**: 强大的正则表达式搜索
- **Glob**: 文件模式匹配
- **LS**: 列出目录内容

### 系统工具
- **Bash**: 执行 shell 命令
- **Task**: 启动专门的代理任务
- **TodoWrite**: 管理任务列表

### 网络工具
- **WebFetch**: 获取网页内容
- **WebSearch**: 搜索网络信息

## 常用命令

### 系统命令
- `/help` - 获取帮助信息
- `/bug` - 报告问题
- `/model` - 切换AI模型
- `/settings` - 配置设置
- `/exit` 或 `/quit` - 退出程序

### 工作流命令
- 创建提交: 自动分析更改并生成提交信息
- 创建PR: 自动创建 Pull Request
- 代码审查: 分析代码质量和潜在问题

## 配置文件

### CLAUDE.md
项目记忆文件，用于存储：
- 项目特定信息
- 开发规范
- 常用命令
- 服务器配置
- API密钥（应加入.gitignore）

### .claudeignore
类似于 .gitignore，指定 Claude Code 应忽略的文件和目录

## 最佳实践

### 1. 项目初始化
- 在项目根目录创建 CLAUDE.md
- 配置 .claudeignore
- 设置项目特定的规范

### 2. 代码操作
- 先读取文件再修改
- 使用描述性的任务说明
- 保持任务聚焦和具体

### 3. 安全考虑
- 敏感信息存储在 CLAUDE.md 并加入 .gitignore
- 使用别名代替真实的服务器信息
- 不在代码中硬编码密码

### 4. 性能优化
- 使用批量操作工具（MultiEdit）
- 并行执行独立任务
- 合理使用搜索范围

## 高级功能

### 1. 任务管理 (TodoWrite)
- 创建结构化任务列表
- 跟踪任务进度
- 标记任务状态（pending, in_progress, completed）

### 2. 专门代理 (Task)
可用的代理类型：
- **general-purpose**: 通用任务处理
- **statusline-setup**: 状态栏配置
- **output-style-setup**: 输出样式设置
- **code-reviewer**: 代码审查（未来功能）

### 3. 后台任务
- 使用 `run_in_background` 参数运行长时间任务
- 使用 BashOutput 监控输出
- 使用 KillBash 终止任务

## 网络代理配置

对于内网环境，需要配置代理：

### Git 代理配置
```bash
git config --global http.proxy http://代理服务器:端口
git config --global https.proxy http://代理服务器:端口
```

### 环境变量配置
```bash
export HTTP_PROXY=http://代理服务器:端口
export HTTPS_PROXY=http://代理服务器:端口
```

## 故障排查

### 常见问题
1. **无法连接外网**: 配置代理设置
2. **权限错误**: 检查文件权限
3. **命令失败**: 使用 /bug 报告问题

### 日志和调试
- 查看错误信息
- 使用详细模式
- 检查网络连接

## 社区与支持

- **GitHub Issues**: https://github.com/anthropics/claude-code/issues
- **Discord 社区**: Claude Developers Discord
- **官方文档**: https://docs.anthropic.com/en/docs/claude-code/overview

## 数据隐私

- 反馈数据保留 30 天
- 不用于模型训练
- 仅用于产品改进和问题修复

## 版本更新

定期更新以获得最新功能：
```bash
npm update -g @anthropic-ai/claude-code
```

检查当前版本：
```bash
npm list -g @anthropic-ai/claude-code
```

## 许可和条款

- 商业服务条款: https://www.anthropic.com/legal/commercial-terms
- 隐私政策: https://www.anthropic.com/legal/privacy

---

*本文档基于 Claude Code 官方信息整理，持续更新中。*