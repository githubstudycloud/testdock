# Claude Code 快速参考卡

## 🚀 快速启动
```bash
npm install -g @anthropic-ai/claude-code
cd your-project
claude
```

## 📊 工具速查图

```
┌──────────────────────────────────────────────────────────┐
│                     Claude Code 工具体系                   │
├──────────────────────────────────────────────────────────┤
│                                                            │
│  文件操作                搜索工具              系统工具      │
│  ┌────────┐           ┌────────┐          ┌────────┐     │
│  │  Read  │           │  Grep  │          │  Bash  │     │
│  │  Write │           │  Glob  │          │  Task  │     │
│  │  Edit  │           │   LS   │          │TodoWrite│    │
│  └────────┘           └────────┘          └────────┘     │
│                                                            │
│  网络工具                特殊工具              后台管理      │
│  ┌────────┐           ┌────────┐          ┌────────┐     │
│  │WebFetch│           │MultiEdit│         │BashOutput│   │
│  │WebSearch│          │NotebookEdit│      │ KillBash │   │
│  └────────┘           └────────┘          └────────┘     │
│                                                            │
└──────────────────────────────────────────────────────────┘
```

## 🎯 使用决策树

```
需要操作文件？
├─ 读取 → Read
├─ 创建 → Write  
├─ 修改 → Edit / MultiEdit
└─ 搜索 → Grep / Glob

需要执行命令？
├─ 简单命令 → Bash
├─ 后台任务 → Bash (run_in_background)
└─ 复杂任务 → Task

需要管理任务？
├─ 简单任务 → 直接执行
└─ 复杂任务 → TodoWrite

需要网络资源？
├─ 获取网页 → WebFetch
└─ 搜索信息 → WebSearch
```

## 📋 常用操作模板

### 1. 创建新功能
```
1. TodoWrite → 创建任务列表
2. Read → 理解现有代码
3. Write/Edit → 实现功能
4. Bash → 运行测试
5. Git commit → 提交代码
```

### 2. 修复 Bug
```
1. Grep → 定位问题代码
2. Read → 查看上下文
3. Edit → 修复问题
4. Bash → 验证修复
5. Git commit → 提交修复
```

### 3. 代码重构
```
1. TodoWrite → 规划重构步骤
2. Glob → 找到相关文件
3. MultiEdit → 批量修改
4. Bash → 运行测试套件
5. Git commit → 提交重构
```

## 💡 效率提升技巧

### 并行执行
```javascript
// ❌ 低效
执行命令1 → 等待 → 执行命令2 → 等待

// ✅ 高效  
[命令1, 命令2, 命令3] → 并行执行
```

### 批量操作
```javascript
// ❌ 多次 Edit
Edit file1 → Edit file2 → Edit file3

// ✅ 一次 MultiEdit
MultiEdit [file1, file2, file3]
```

### 精确搜索
```javascript
// ❌ 模糊搜索
Grep "function"

// ✅ 精确搜索
Grep "function getUserById" --type js
```

## 🔧 命令组合示例

### 完整的提交流程
```bash
git status && git diff && git log -3
→ 分析更改
→ git add . && git commit -m "message"
```

### 项目初始化
```bash
mkdir project && cd project
→ npm init -y
→ 创建 CLAUDE.md
→ 配置 .gitignore
```

### 依赖安装与测试
```bash
npm install && npm run build && npm test
```

## 📝 Git 提交信息模板

```
<类型>: <简短描述>

<详细说明>

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>
```

类型：
- feat: 新功能
- fix: 修复bug
- docs: 文档更新
- style: 格式调整
- refactor: 代码重构
- test: 测试相关
- chore: 构建/工具

## ⚠️ 注意事项清单

- [ ] 修改前先 Read
- [ ] 敏感信息用别名
- [ ] CLAUDE.md 加入 .gitignore
- [ ] 复杂任务用 TodoWrite
- [ ] 批量操作用 MultiEdit
- [ ] 长任务用后台运行
- [ ] 提交前运行测试

## 🔍 问题诊断

| 症状 | 检查项 | 解决方案 |
|------|--------|---------|
| 无法编辑 | 是否先Read? | 先读取文件 |
| 网络失败 | 代理配置? | 设置HTTP_PROXY |
| 命令失败 | 路径正确? | 使用绝对路径 |
| 搜索无果 | 范围太大? | 限定搜索范围 |

## 🎨 状态指示器

```
任务状态：
⏸ pending    - 待处理
▶ in_progress - 进行中
✅ completed  - 已完成

优先级：
🔴 紧急重要
🟡 重要不紧急
🟢 常规任务
```

## 📈 性能基准

| 操作 | 推荐方式 | 性能提升 |
|------|---------|---------|
| 10个文件编辑 | MultiEdit | 5x |
| 多命令执行 | 并行Bash | 3x |
| 大项目搜索 | Grep+类型过滤 | 10x |
| 复杂任务 | Task代理 | 2x |

## 🌐 代理配置模板

```bash
# Git 代理
git config --global http.proxy http://proxy:port
git config --global https.proxy http://proxy:port

# 环境变量
export HTTP_PROXY=http://proxy:port
export HTTPS_PROXY=http://proxy:port
export NO_PROXY=localhost,127.0.0.1

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 📚 学习路径

```
入门级：
1. 基础命令 → 2. 文件操作 → 3. 简单Git

进阶级：
4. 任务管理 → 5. 批量操作 → 6. 搜索技巧

高级：
7. 自定义工作流 → 8. 性能优化 → 9. 团队协作
```

---
💡 **Pro Tip**: 将此卡片打印或保存为书签，随时查阅！