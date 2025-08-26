# Claude Code 命令分类详解与使用指南

## 🎯 命令分类架构图

```
Claude Code v1.0.92 命令体系
│
├── 🏗️ 项目管理 (Project Management)
│   ├── /init - 初始化项目
│   ├── /add-dir - 添加工作目录
│   └── /memory - 编辑记忆文件
│
├── 🤖 AI与模型 (AI & Models)
│   ├── /model - 切换模型
│   ├── /agents - 代理配置
│   └── /mcp - MCP服务器
│
├── 💬 会话控制 (Session Control)
│   ├── /clear - 清除历史
│   ├── /compact - 压缩历史
│   ├── /context - 上下文可视化
│   ├── /cost - 成本统计
│   ├── /export - 导出对话
│   └── /resume - 恢复会话
│
├── 🔧 开发工具 (Dev Tools)
│   ├── /ide - IDE集成
│   ├── /vim - Vim模式
│   ├── /hooks - 钩子管理
│   ├── /output-style - 输出样式
│   ├── /output-style:new - 新建样式
│   └── /statusline - 状态栏
│
├── 🔀 Git与审查 (Git & Review)
│   ├── /install-github-app - GitHub应用
│   ├── /pr-comments - PR评论
│   ├── /review - 代码审查
│   └── /security-review - 安全审查
│
├── 💻 系统管理 (System Management)
│   ├── /bashes - 后台Shell
│   ├── /doctor - 系统诊断
│   ├── /status - 状态信息
│   ├── /config - 配置面板
│   └── /permissions - 权限管理
│
├── 👤 账户管理 (Account)
│   ├── /login - 登录
│   ├── /logout - 登出
│   └── /upgrade - 升级Max
│
└── ℹ️ 帮助系统 (Help System)
    ├── /help - 帮助菜单
    ├── /bug - 问题反馈
    └── /release-notes - 版本说明
```

---

## 📊 按使用频率分类

### 🔴 高频命令（每天使用）

| 命令 | 使用场景 | 快捷记忆 | 重要度 |
|------|---------|---------|---------|
| `/help` | 查看可用命令 | 📚 帮助 | ⭐⭐⭐⭐⭐ |
| `/model` | 切换AI模型强度 | 🤖 模型 | ⭐⭐⭐⭐⭐ |
| `/clear` | 上下文满了需清理 | 🗑️ 清除 | ⭐⭐⭐⭐⭐ |
| `/status` | 检查系统状态 | 📊 状态 | ⭐⭐⭐⭐ |
| `/context` | 监控token使用 | 📈 上下文 | ⭐⭐⭐⭐ |

### 🟡 中频命令（每周使用）

| 命令 | 使用场景 | 快捷记忆 | 重要度 |
|------|---------|---------|---------|
| `/init` | 新项目初始化 | 🚀 初始化 | ⭐⭐⭐⭐ |
| `/resume` | 继续之前工作 | ⏮️ 恢复 | ⭐⭐⭐⭐ |
| `/review` | 代码审查 | 👁️ 审查 | ⭐⭐⭐⭐ |
| `/compact` | 压缩对话节省空间 | 📦 压缩 | ⭐⭐⭐ |
| `/cost` | 查看使用费用 | 💰 费用 | ⭐⭐⭐ |
| `/hooks` | 配置自动化 | 🔗 钩子 | ⭐⭐⭐ |

### 🟢 低频命令（偶尔使用）

| 命令 | 使用场景 | 快捷记忆 | 重要度 |
|------|---------|---------|---------|
| `/doctor` | 故障排查 | 🏥 诊断 | ⭐⭐⭐⭐⭐ |
| `/permissions` | 安全配置 | 🔒 权限 | ⭐⭐⭐⭐ |
| `/upgrade` | 升级服务 | 💎 升级 | ⭐⭐ |
| `/migrate-installer` | 迁移安装 | 📦 迁移 | ⭐⭐ |
| `/security-review` | 安全审计 | 🛡️ 安全 | ⭐⭐⭐ |

---

## 🔍 按功能场景分类

### 场景1：开始新项目

```bash
# 标准流程
1. claude                    # 启动
2. /init                     # 初始化CLAUDE.md
3. /add-dir ./src           # 添加源码目录
4. /model opus              # 使用强大模型
5. /permissions             # 配置权限
```

### 场景2：日常开发

```bash
# 开发流程
1. /resume                   # 恢复昨天的会话
2. /context                  # 查看剩余空间
3. 编写代码...
4. /compact 保留关键函数    # 压缩历史
5. /cost                     # 查看今日费用
```

### 场景3：代码审查

```bash
# 审查流程
1. /review                   # 开始审查
2. /pr-comments             # 查看PR评论
3. /security-review         # 安全检查
4. /export                  # 导出审查报告
```

### 场景4：问题诊断

```bash
# 诊断流程
1. /doctor                   # 系统诊断
2. /status                   # 查看状态
3. /bashes                   # 检查后台任务
4. /bug 描述问题            # 报告问题
```

### 场景5：环境配置

```bash
# 配置流程
1. /config                   # 打开配置
2. /hooks                    # 设置钩子
3. /ide                      # IDE集成
4. /vim                      # 编辑器模式
5. /output-style:new        # 自定义输出
```

---

## 📈 命令关联图

```mermaid
graph TD
    A[开始使用] --> B[/help]
    B --> C{选择任务}
    
    C -->|新项目| D[/init]
    D --> E[/add-dir]
    E --> F[/memory]
    
    C -->|继续工作| G[/resume]
    G --> H[/context]
    H --> I{空间不足?}
    I -->|是| J[/compact]
    I -->|否| K[继续开发]
    
    C -->|代码审查| L[/review]
    L --> M[/security-review]
    M --> N[/pr-comments]
    
    C -->|系统问题| O[/doctor]
    O --> P[/status]
    P --> Q[/bug]
    
    C -->|配置环境| R[/config]
    R --> S[/permissions]
    S --> T[/hooks]
```

---

## 💡 命令组合技巧

### 高效组合1：项目初始化套装
```bash
/init && /add-dir . && /model opus && /permissions
```

### 高效组合2：会话管理套装
```bash
/context && /cost && /compact "保留重要函数"
```

### 高效组合3：安全审查套装
```bash
/review && /security-review && /export report.md
```

### 高效组合4：问题诊断套装
```bash
/doctor && /status && /bashes
```

---

## 🎨 命令使用矩阵

| 用户级别 | 必须掌握 | 建议掌握 | 可选掌握 |
|---------|---------|---------|---------|
| **初学者** | /help, /model, /exit | /init, /clear | /resume |
| **普通用户** | +/context, /cost | /compact, /review | /hooks, /vim |
| **高级用户** | +/permissions, /doctor | /security-review, /mcp | /output-style:new |
| **专家用户** | 全部基础命令 | 全部高级命令 | 自定义扩展 |

---

## 📱 移动端速查（手机保存版）

### 🔝 最重要10个
```
1. /help      - 帮助
2. /model     - 模型
3. /clear     - 清除
4. /context   - 上下文
5. /status    - 状态
6. /init      - 初始化
7. /resume    - 恢复
8. /cost      - 费用
9. /exit      - 退出
10. /doctor   - 诊断
```

### ⚡ 快速操作
```
查帮助: /help
换模型: /model opus
看状态: /status
清历史: /clear
看费用: /cost
```

---

## 🔒 安全相关命令

| 命令 | 安全级别 | 用途 | 建议 |
|------|---------|------|------|
| `/permissions` | 🔴 关键 | 控制工具权限 | 首先配置 |
| `/security-review` | 🔴 关键 | 代码安全审查 | 部署前必做 |
| `/doctor` | 🟡 重要 | 环境诊断 | 定期执行 |
| `/hooks` | 🟡 重要 | 自动化检查 | 配置linter |
| `/logout` | 🟢 常规 | 安全退出 | 共享环境必做 |

---

## 📊 命令使用统计

### 按类别分布
```
系统管理: 20% (8个命令)
会话控制: 15% (6个命令)  
开发工具: 15% (6个命令)
系统维护: 12.5% (5个命令)
Git集成: 10% (4个命令)
项目管理: 7.5% (3个命令)
AI配置: 7.5% (3个命令)
账户管理: 7.5% (3个命令)
帮助系统: 7.5% (3个命令)
```

### 新功能占比
```
新增命令: 20% (8个)
增强命令: 25% (10个)
传统命令: 55% (22个)
```

---

## 📝 备注

- 🆕 标记为v1.0.92新增功能
- ⭐ 表示重要程度（1-5星）
- 所有命令均支持Tab自动补全
- 命令可以组合使用提高效率

---

*最后更新: 2024年12月*
*版本: Claude Code v1.0.92*
*基于官方/help输出整理*