# Claude Code 快速配置模板包

## 🚀 一键配置脚本

### setup-claude-project.sh
```bash
#!/bin/bash
# Claude Code 项目一键配置脚本
# 使用方法: ./setup-claude-project.sh [项目名称]

PROJECT_NAME=${1:-"MyProject"}
CLAUDE_DIR=".claude"

echo "🚀 开始配置 Claude Code 项目: $PROJECT_NAME"

# 1. 创建目录结构
echo "📁 创建目录结构..."
mkdir -p "$CLAUDE_DIR"/{commands,agents,styles,logs,templates}
mkdir -p docs

# 2. 基础配置文件
echo "⚙️ 生成配置文件..."

# settings.json - 主配置
cat > "$CLAUDE_DIR/settings.json" << EOF
{
  "model": "sonnet",
  "projectName": "$PROJECT_NAME",
  "maxTokens": 4096,
  "workingDirectories": ["./src", "./tests", "./docs"],
  "autoConfirm": {
    "enabled": true,
    "maxErrors": 3,
    "pauseOnError": true,
    "cooldownMinutes": 5
  },
  "permissions": {
    "allowAll": true,
    "allowedTools": [
      "Read", "Write", "Edit", "MultiEdit",
      "Grep", "Glob", "LS", "Bash", "Task", "TodoWrite",
      "WebFetch", "WebSearch", "NotebookEdit"
    ],
    "allowedBashCommands": [
      "git *", "npm *", "yarn *", "python *", "node *",
      "docker *", "ls *", "cat *", "grep *", "find *",
      "test *", "build *", "start *", "dev *"
    ],
    "deniedPatterns": [
      "rm -rf /", "sudo rm", "format *", "del /q",
      "shutdown", "reboot", "kill -9"
    ]
  },
  "hooks": {
    "afterEdit": {
      "*.py": "black {file} && isort {file}",
      "*.js": "prettier --write {file}",
      "*.ts": "prettier --write {file}",
      "*.json": "prettier --write {file}"
    },
    "beforeCommit": [
      "npm run lint",
      "npm run test"
    ]
  },
  "output": {
    "style": "professional",
    "showTimestamp": true,
    "showCost": true
  },
  "errorHandling": {
    "maxConsecutiveErrors": 3,
    "pauseDuration": "5m",
    "autoRecover": true
  }
}
EOF

# 3. Agents 配置
cat > "$CLAUDE_DIR/agents.json" << 'EOF'
{
  "agents": {
    "code-reviewer": {
      "enabled": true,
      "model": "opus",
      "systemPrompt": "专业代码审查员，关注代码质量、安全性和最佳实践。",
      "tools": ["Read", "Grep", "Write"],
      "autoTrigger": {
        "events": ["beforeCommit"],
        "filePatterns": ["*.js", "*.py", "*.ts"]
      }
    },
    "test-generator": {
      "enabled": true,
      "model": "sonnet", 
      "systemPrompt": "自动生成高质量的单元测试和集成测试。",
      "tools": ["Read", "Write", "Bash"],
      "autoTrigger": {
        "events": ["afterEdit"],
        "filePatterns": ["*.js", "*.py"],
        "excludePatterns": ["*test*", "*spec*"]
      }
    },
    "doc-writer": {
      "enabled": true,
      "model": "haiku",
      "systemPrompt": "自动生成和维护代码文档。",
      "tools": ["Read", "Write", "Grep"],
      "autoTrigger": {
        "events": ["afterEdit"],
        "conditions": ["missingComments", "hasPublicMethods"]
      }
    }
  }
}
EOF

# 4. MCP 服务器配置
cat > "$CLAUDE_DIR/mcp-servers.json" << 'EOF'
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"],
      "env": {
        "ALLOWED_DIRECTORIES": "."
      },
      "enabled": true
    },
    "git-integration": {
      "command": "python",
      "args": ["-m", "mcp_server_git"],
      "workingDirectory": "./",
      "enabled": true
    }
  }
}
EOF

# 5. CLAUDE.md 记忆文件
cat > "CLAUDE.md" << EOF
# $PROJECT_NAME 项目记忆文件

## 🎯 项目概述
- **项目名称**: $PROJECT_NAME
- **创建日期**: $(date +%Y-%m-%d)
- **技术栈**: [待填写]
- **架构**: [待填写]

## 🏗️ 项目结构
\`\`\`
$PROJECT_NAME/
├── src/              # 源代码
├── tests/            # 测试文件
├── docs/             # 文档
├── .claude/          # Claude配置
└── CLAUDE.md         # 本文件
\`\`\`

## 🔧 开发环境

### 本地开发启动
\`\`\`bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 运行测试
npm test
\`\`\`

### 构建和部署
\`\`\`bash
# 构建项目
npm run build

# 部署到开发环境
npm run deploy:dev

# 部署到生产环境
npm run deploy:prod
\`\`\`

## 📋 开发规范

### Git 提交规范
- feat: 新功能
- fix: 修复bug
- docs: 文档更新
- style: 代码格式
- refactor: 重构
- test: 测试
- chore: 构建/工具

### 代码风格
- JavaScript/TypeScript: Prettier + ESLint
- Python: Black + Flake8
- 命名: camelCase (JS), snake_case (Python)

## 🔐 敏感信息配置

### 服务器信息
- **开发服务器**: DEV_SERVER_IP
- **测试服务器**: TEST_SERVER_IP  
- **生产服务器**: PROD_SERVER_IP
- **数据库**: DB_HOST:DB_PORT

### API配置
- **API Base URL**: API_BASE_URL
- **API Key**: API_KEY_SECRET
- **Database URL**: DATABASE_URL

### 部署命令
\`\`\`bash
# SSH到开发服务器
ssh user@DEV_SERVER_IP

# SSH到生产服务器
ssh user@PROD_SERVER_IP
\`\`\`

## 📚 重要链接
- [项目文档](./docs/)
- [API文档](./docs/api.md)
- [部署指南](./docs/deployment.md)

## ⚠️ 注意事项
1. 敏感信息不要提交到Git
2. 部署前运行完整测试套件
3. 生产环境变更需要审查
4. 定期备份重要数据

## 🐛 常见问题
### 依赖安装失败
\`\`\`bash
rm -rf node_modules package-lock.json
npm install
\`\`\`

### 端口被占用
\`\`\`bash
lsof -ti:3000 | xargs kill -9
\`\`\`

### 权限问题
\`\`\`bash
sudo chown -R \$(whoami) .
\`\`\`

---
*最后更新: $(date +%Y-%m-%d)*
*维护者: [你的名字]*
EOF

# 6. 输出样式配置
cat > "$CLAUDE_DIR/styles/professional.json" << 'EOF'
{
  "name": "professional",
  "colors": {
    "primary": "#2563eb",
    "success": "#16a34a",
    "warning": "#d97706", 
    "error": "#dc2626",
    "info": "#0891b2"
  },
  "formatting": {
    "codeBlock": {
      "background": "#f8fafc",
      "border": "1px solid #e2e8f0",
      "padding": "12px"
    }
  },
  "icons": {
    "success": "✅",
    "error": "❌",
    "warning": "⚠️",
    "info": "ℹ️",
    "task": "📋"
  }
}
EOF

# 7. Hooks 配置
cat > "$CLAUDE_DIR/hooks.json" << 'EOF'
{
  "beforeEdit": {
    "*.py": ["python -m py_compile {file}"],
    "*.js": ["node -c {file}"],
    "*.json": ["python -m json.tool {file} > /dev/null"]
  },
  "afterEdit": {
    "*.py": ["black {file}", "isort {file}"],
    "*.js": ["prettier --write {file}"],
    "*.ts": ["prettier --write {file}"],
    "package.json": ["npm audit fix"]
  },
  "onError": {
    "maxRetries": 3,
    "retryDelay": 2000,
    "actions": [
      {
        "condition": "syntax_error",
        "action": "revert_last_edit"
      },
      {
        "condition": "permission_denied",
        "action": "skip_and_continue" 
      }
    ]
  }
}
EOF

# 8. 权限配置模板
cat > "$CLAUDE_DIR/permissions-template.json" << 'EOF'
{
  "autoConfirm": true,
  "allowAll": true,
  "maxErrors": 3,
  "pauseOnError": true,
  "allowedTools": ["*"],
  "allowedBashCommands": ["git *", "npm *", "python *"],
  "deniedPatterns": ["rm -rf /", "sudo rm", "format", "shutdown"]
}
EOF

# 9. 环境变量模板
cat > ".env.template" << 'EOF'
# Claude Code 环境变量模板
CLAUDE_API_KEY=your_api_key_here
CLAUDE_MODEL=sonnet
CLAUDE_AUTO_CONFIRM=true
CLAUDE_MAX_ERRORS=3

# 项目相关
PROJECT_NAME=MyProject
NODE_ENV=development

# 数据库
DATABASE_URL=postgresql://user:pass@localhost/db

# API配置
API_BASE_URL=https://api.example.com
API_KEY=your_api_key

# 服务器配置 (使用别名)
DEV_SERVER_IP=192.168.0.127
PROD_SERVER_IP=your_prod_server
EOF

# 10. .gitignore 更新
cat >> ".gitignore" << 'EOF'

# Claude Code 配置
.claude/settings.local.json
.claude/agents.local.json
.claude/mcp-servers.local.json
.claude/logs/
CLAUDE.md
.env
EOF

# 11. 创建快捷启动脚本
cat > "claude-start.sh" << 'EOF'
#!/bin/bash
# Claude Code 快捷启动脚本

# 设置环境变量
export CLAUDE_AUTO_CONFIRM=true
export CLAUDE_MAX_ERRORS=3

# 检查配置文件
if [ ! -f ".claude/settings.json" ]; then
    echo "❌ Claude配置文件不存在，请先运行 setup-claude-project.sh"
    exit 1
fi

# 启动Claude Code
echo "🚀 启动 Claude Code..."
claude --config .claude/settings.json "$@"
EOF

chmod +x claude-start.sh

# 12. 创建配置验证脚本
cat > "$CLAUDE_DIR/validate-config.sh" << 'EOF'
#!/bin/bash
# 配置验证脚本

echo "🔍 验证 Claude Code 配置..."

# 检查必需文件
files=("settings.json" "agents.json" "mcp-servers.json" "hooks.json")
for file in "${files[@]}"; do
    if [ -f ".claude/$file" ]; then
        echo "✅ $file 存在"
    else
        echo "❌ $file 不存在"
    fi
done

# 检查JSON格式
for file in "${files[@]}"; do
    if [ -f ".claude/$file" ]; then
        if python -m json.tool ".claude/$file" > /dev/null 2>&1; then
            echo "✅ $file JSON格式正确"
        else
            echo "❌ $file JSON格式错误"
        fi
    fi
done

# 检查CLAUDE.md
if [ -f "CLAUDE.md" ]; then
    echo "✅ CLAUDE.md 存在"
else
    echo "❌ CLAUDE.md 不存在"
fi

echo "🎉 配置验证完成！"
EOF

chmod +x "$CLAUDE_DIR/validate-config.sh"

# 13. 创建使用说明
cat > "CLAUDE-README.md" << EOF
# Claude Code 项目配置说明

## 🎯 快速开始

1. **启动Claude Code**:
   \`\`\`bash
   ./claude-start.sh
   \`\`\`

2. **验证配置**:
   \`\`\`bash
   ./.claude/validate-config.sh
   \`\`\`

## 📁 文件结构说明

- \`.claude/\` - Claude配置目录
  - \`settings.json\` - 主配置文件
  - \`agents.json\` - AI代理配置
  - \`mcp-servers.json\` - MCP服务器配置
  - \`hooks.json\` - 钩子函数配置
- \`CLAUDE.md\` - 项目记忆文件（敏感信息，已加入.gitignore）
- \`claude-start.sh\` - 快捷启动脚本

## ⚙️ 主要功能

### 自动化配置
- ✅ 3次错误自动暂停
- ✅ 代码格式化钩子
- ✅ 自动权限管理
- ✅ 智能代理协助

### 安全配置
- 🔒 敏感文件自动忽略
- 🔒 危险命令黑名单
- 🔒 权限最小化原则

## 🔧 自定义配置

修改 \`.claude/settings.json\` 中的配置项：

- \`model\`: AI模型选择 (opus/sonnet/haiku)
- \`autoConfirm\`: 自动确认设置
- \`permissions\`: 权限控制
- \`hooks\`: 自动化钩子

## 🚨 常见问题

1. **权限错误**: 运行 \`chmod +x claude-start.sh\`
2. **配置错误**: 运行 \`.claude/validate-config.sh\`
3. **JSON格式错误**: 使用在线JSON验证器检查

## 📞 获取帮助

- 在Claude Code中输入 \`/help\`
- 使用 \`/doctor\` 诊断问题
- 使用 \`/bug\` 报告问题

---
*配置生成于: $(date)*
EOF

echo ""
echo "🎉 Claude Code 项目配置完成！"
echo ""
echo "📋 已创建的文件:"
echo "   - .claude/ (配置目录)"  
echo "   - CLAUDE.md (项目记忆文件)"
echo "   - claude-start.sh (启动脚本)"
echo "   - CLAUDE-README.md (使用说明)"
echo ""
echo "🚀 下一步:"
echo "   1. 编辑 CLAUDE.md 填入项目具体信息"
echo "   2. 运行 ./claude-start.sh 启动Claude"
echo "   3. 使用 /init 初始化项目上下文"
echo ""
echo "🔍 验证配置: ./.claude/validate-config.sh"
echo ""
```

## 🎛️ 高级配置模板

### 企业级配置模板
```json
{
  "enterprise": {
    "security": {
      "strictMode": true,
      "auditLog": true,
      "encryptSensitive": true,
      "twoFactorAuth": false
    },
    "compliance": {
      "dataRetention": "30d",
      "logLevel": "detailed",
      "anonymizeData": true
    },
    "performance": {
      "maxConcurrentTasks": 5,
      "timeoutSeconds": 300,
      "cacheEnabled": true,
      "rateLimiting": true
    }
  }
}
```

### 开发团队配置模板  
```json
{
  "team": {
    "collaboration": {
      "sharedMemory": true,
      "crossProjectContext": true,
      "teamHooks": true
    },
    "standards": {
      "codeStyle": "airbnb",
      "commitConvention": "conventional",
      "branchStrategy": "gitflow"
    },
    "automation": {
      "autoReview": true,
      "autoTest": true,
      "autoFormat": true,
      "autoDeploy": false
    }
  }
}
```

## 📊 配置优先级矩阵

| 配置层级 | 文件位置 | 优先级 | 用途 |
|---------|---------|--------|------|
| 全局用户 | ~/.claude/ | 1 (最低) | 默认设置 |
| 项目级 | .claude/ | 2 | 项目特定 |
| 本地覆盖 | .claude/*.local.json | 3 (最高) | 本地调试 |
| 环境变量 | CLAUDE_* | 4 (运行时) | 动态配置 |

## 🔄 配置更新脚本

### update-claude-config.sh
```bash
#!/bin/bash
# Claude Code 配置更新脚本

echo "🔄 更新 Claude Code 配置..."

# 备份现有配置
backup_dir=".claude/backup/$(date +%Y%m%d_%H%M%S)"
mkdir -p "$backup_dir"
cp -r .claude/*.json "$backup_dir/" 2>/dev/null

# 下载最新配置模板
curl -s "https://raw.githubusercontent.com/your-repo/claude-templates/main/settings.json" > .claude/settings.new.json

# 合并配置（保留用户自定义）
python3 << 'EOF'
import json

# 读取现有和新配置
with open('.claude/settings.json', 'r') as f:
    current = json.load(f)
with open('.claude/settings.new.json', 'r') as f:
    new = json.load(f)

# 智能合并
def merge_config(current, new):
    for key, value in new.items():
        if key not in current:
            current[key] = value
        elif isinstance(value, dict) and isinstance(current[key], dict):
            merge_config(current[key], value)
    return current

merged = merge_config(current, new)

# 保存合并结果
with open('.claude/settings.json', 'w') as f:
    json.dump(merged, f, indent=2)
EOF

# 验证配置
if .claude/validate-config.sh; then
    echo "✅ 配置更新成功！"
    rm .claude/settings.new.json
else
    echo "❌ 配置验证失败，恢复备份..."
    cp "$backup_dir"/*.json .claude/
fi
```

这个配置包提供了从基础到企业级的完整Claude Code配置方案，让你能够快速建立一个高效、安全、自动化的开发环境！