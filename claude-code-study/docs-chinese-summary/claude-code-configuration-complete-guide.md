# Claude Code 配置管理完全指南

## 📁 配置文件系统架构

### 配置文件层级（优先级从低到高）

```
配置文件层级结构:
│
├── 全局用户配置 (最低优先级)
│   ├── ~/.claude/settings.json          # 用户全局设置
│   ├── ~/.claude/commands/              # 用户级自定义命令
│   ├── ~/.claude/agents.json           # 代理配置
│   └── ~/.claude/mcp-servers.json      # MCP服务器配置
│
├── 项目级配置 (中等优先级)
│   ├── .claude/settings.json           # 项目设置
│   ├── .claude/commands/               # 项目级命令
│   ├── .claude/agents.json            # 项目代理
│   ├── .claude/mcp-servers.json       # 项目MCP
│   └── .claude/hooks.json             # 钩子配置
│
└── 本地配置 (最高优先级，不提交Git)
    ├── .claude/settings.local.json     # 本地覆盖设置
    ├── .claude/agents.local.json      # 本地代理配置
    └── CLAUDE.md                       # 项目记忆文件
```

---

## ⚙️ 核心配置文件详解

### 1. settings.json - 主配置文件

#### 全局配置示例 (~/.claude/settings.json)
```json
{
  "model": "opus",
  "maxTokens": 4096,
  "autoConfirm": {
    "enabled": true,
    "maxErrors": 3,
    "pauseOnError": true,
    "allowedTools": ["Read", "Grep", "LS", "WebFetch"]
  },
  "permissions": {
    "mode": "auto",
    "allowAll": true,
    "denyPatterns": ["rm -rf", "sudo", "format"],
    "allowedBashCommands": [
      "git *",
      "npm *", 
      "yarn *",
      "docker *",
      "kubectl *",
      "python *",
      "node *"
    ]
  },
  "output": {
    "style": "professional",
    "showTimestamp": true,
    "showCost": true,
    "colorScheme": "dark"
  },
  "ide": {
    "integration": "vscode",
    "autoSave": true,
    "syncCursor": true
  },
  "proxy": {
    "http": "http://192.168.0.98:8800",
    "https": "http://192.168.0.98:8800",
    "bypass": ["localhost", "127.0.0.1", "*.local"]
  }
}
```

#### 项目级配置示例 (.claude/settings.json)
```json
{
  "model": "sonnet",
  "projectName": "MyAwesomeProject",
  "workingDirectories": ["./src", "./tests", "./docs"],
  "permissions": {
    "allowedTools": [
      "Read", "Write", "Edit", "MultiEdit",
      "Grep", "Glob", "LS",
      "Bash", "Task", "TodoWrite"
    ],
    "allowedBashCommands": [
      "npm run *",
      "yarn *",
      "git *",
      "pytest *",
      "docker-compose *"
    ],
    "denyPaths": [
      ".env*",
      "secrets/",
      "private/"
    ]
  },
  "hooks": {
    "beforeEdit": {
      "*.py": "black --check {file}",
      "*.js": "eslint {file}"
    },
    "afterEdit": {
      "*.py": "black {file}",
      "*.js": "prettier --write {file}",
      "*.ts": "prettier --write {file}"
    },
    "beforeCommit": [
      "npm run test",
      "npm run lint"
    ]
  },
  "contextWindow": {
    "maxFiles": 50,
    "maxLines": 10000,
    "autoCompact": true,
    "compactThreshold": 0.8
  }
}
```

---

## 🚀 项目初始化完整流程

### 1. 自动初始化脚本

#### init-project.sh
```bash
#!/bin/bash
# Claude Code 项目初始化脚本

# 创建.claude目录结构
mkdir -p .claude/commands
mkdir -p .claude/agents
mkdir -p .claude/styles

# 创建基础配置文件
cat > .claude/settings.json << 'EOF'
{
  "model": "sonnet",
  "permissions": {
    "allowAll": false,
    "allowedTools": [
      "Read", "Write", "Edit", "MultiEdit",
      "Grep", "Glob", "LS", "Bash"
    ]
  },
  "hooks": {
    "afterEdit": {
      "*.py": "black {file}",
      "*.js": "prettier --write {file}"
    }
  }
}
EOF

# 创建CLAUDE.md模板
cat > CLAUDE.md << 'EOF'
# 项目记忆文件

## 项目信息
- 项目名称: [项目名]
- 技术栈: [技术栈]
- 开发环境: [环境信息]

## 常用命令
```bash
# 启动开发服务器
npm run dev

# 运行测试
npm test

# 构建项目
npm run build
```

## 开发规范
- 代码风格: [风格指南]
- 命名规则: [命名规则]
- 提交规范: [提交规范]

## 架构说明
[项目架构描述]

## API配置
- 基础URL: DEV_API_URL
- 认证方式: [认证方式]

## 部署配置
- 开发环境: DEV_SERVER
- 生产环境: PROD_SERVER
EOF

# 添加到.gitignore
echo "" >> .gitignore
echo "# Claude Code 配置" >> .gitignore
echo ".claude/settings.local.json" >> .gitignore
echo ".claude/agents.local.json" >> .gitignore
echo "CLAUDE.md" >> .gitignore

echo "Claude Code 项目初始化完成！"
```

### 2. CLAUDE.md 高级模板

```markdown
# 项目记忆文件 - [PROJECT_NAME]

## 🎯 项目概述
- **项目名称**: MyProject
- **版本**: v1.0.0
- **技术栈**: React + Node.js + PostgreSQL
- **架构模式**: 微服务架构

## 🏗️ 项目结构
```
project/
├── frontend/          # React前端
├── backend/           # Node.js后端
├── database/          # 数据库脚本
├── docker/           # Docker配置
└── docs/             # 文档
```

## 🔧 开发环境
### 本地开发
```bash
# 启动前端
cd frontend && npm run dev

# 启动后端
cd backend && npm run start:dev

# 启动数据库
docker-compose up -d postgres
```

### 测试命令
```bash
# 单元测试
npm run test

# 集成测试
npm run test:integration

# E2E测试
npm run test:e2e
```

## 📋 开发规范

### 代码风格
- **JavaScript**: ESLint + Prettier
- **Python**: Black + Flake8
- **CSS**: Styled-components
- **命名**: camelCase for JS, snake_case for Python

### Git提交规范
```
feat: 新功能
fix: 修复bug  
docs: 文档更新
style: 代码格式调整
refactor: 重构
test: 测试相关
chore: 构建/工具相关
```

### 分支策略
- `main`: 生产分支
- `develop`: 开发分支
- `feature/*`: 功能分支
- `hotfix/*`: 紧急修复

## 🔐 敏感信息配置

### 服务器信息
- **开发服务器**: 192.168.0.127
- **测试服务器**: TEST_SERVER_IP
- **生产服务器**: PROD_SERVER_IP
- **数据库**: DB_HOST:5432

### API密钥
- **第三方服务API**: API_KEY_SERVICE
- **数据库密码**: DB_PASSWORD
- **JWT密钥**: JWT_SECRET

### 部署配置
```bash
# 开发环境
ssh user@192.168.0.127
docker-compose -f docker-compose.dev.yml up -d

# 生产环境  
ssh user@PROD_SERVER_IP
docker-compose -f docker-compose.prod.yml up -d
```

## 📚 重要文档链接
- [API文档](http://localhost:3001/docs)
- [数据库设计](./docs/database.md)  
- [部署指南](./docs/deployment.md)

## ⚠️ 注意事项
1. 数据库迁移前先备份
2. 生产部署需要运行全套测试
3. API变更需要更新文档
4. 敏感配置不要硬编码

## 🐛 常见问题
### 数据库连接失败
检查端口是否被占用：`netstat -tulpn | grep 5432`

### 前端构建失败
清除缓存：`npm run clean && npm install`

### Docker容器启动失败
检查端口冲突：`docker ps -a`

---
*最后更新: 2024-12-XX*
*维护者: [你的名字]*
```

---

## 🤖 Agents 配置详解

### agents.json 配置文件

```json
{
  "agents": {
    "code-reviewer": {
      "enabled": true,
      "model": "opus",
      "systemPrompt": "你是一个专业的代码审查员，专注于代码质量、安全性和最佳实践。",
      "tools": ["Read", "Grep", "Write"],
      "autoTrigger": {
        "events": ["afterCommit", "beforePR"],
        "filePatterns": ["*.js", "*.py", "*.ts"]
      },
      "config": {
        "checkSecurity": true,
        "checkPerformance": true,
        "checkStyle": true,
        "generateReport": true
      }
    },
    "test-generator": {
      "enabled": true,
      "model": "sonnet",
      "systemPrompt": "专门为代码生成单元测试和集成测试。",
      "tools": ["Read", "Write", "Bash"],
      "autoTrigger": {
        "events": ["afterEdit"],
        "filePatterns": ["*.js", "*.py"],
        "excludePatterns": ["*test*", "*spec*"]
      },
      "config": {
        "testFramework": "jest",
        "coverage": 80,
        "includeEdgeCases": true
      }
    },
    "documentation-bot": {
      "enabled": true,
      "model": "haiku",
      "systemPrompt": "自动生成和更新代码文档。",
      "tools": ["Read", "Write", "Grep"],
      "autoTrigger": {
        "events": ["afterEdit"],
        "filePatterns": ["*.js", "*.py", "*.ts"],
        "conditions": ["hasPublicMethods", "missingComments"]
      },
      "config": {
        "docStyle": "JSDoc",
        "includeExamples": true,
        "updateReadme": true
      }
    },
    "deployment-assistant": {
      "enabled": false,
      "model": "opus",
      "systemPrompt": "协助部署和DevOps任务。",
      "tools": ["Bash", "Read", "Write"],
      "permissions": {
        "allowedCommands": ["docker", "kubectl", "git"],
        "requireConfirmation": true
      },
      "config": {
        "environment": "development",
        "autoRollback": true,
        "healthCheck": true
      }
    }
  },
  "globalSettings": {
    "maxConcurrentAgents": 3,
    "timeoutSeconds": 300,
    "errorHandling": {
      "maxRetries": 3,
      "pauseOnError": true,
      "notifyOnFailure": true
    }
  }
}
```

---

## 🔌 MCP (Model Context Protocol) 配置

### mcp-servers.json 配置文件

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"],
      "env": {
        "ALLOWED_DIRECTORIES": "/home/user/projects"
      },
      "enabled": true,
      "description": "文件系统访问服务器"
    },
    "database": {
      "command": "python",
      "args": ["-m", "mcp_server_database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db"
      },
      "enabled": true,
      "description": "数据库查询服务器"
    },
    "web-scraper": {
      "command": "node",
      "args": ["./mcp-servers/web-scraper.js"],
      "env": {
        "USER_AGENT": "Claude-MCP/1.0",
        "RATE_LIMIT": "100"
      },
      "enabled": true,
      "description": "网页抓取服务器"
    },
    "git-integration": {
      "command": "python",
      "args": ["-m", "mcp_server_git"],
      "workingDirectory": "./",
      "env": {
        "GIT_CONFIG_GLOBAL": "false"
      },
      "enabled": true,
      "description": "Git操作增强服务器"
    },
    "api-client": {
      "command": "node",
      "args": ["./mcp-servers/api-client.js"],
      "env": {
        "API_BASE_URL": "https://api.example.com",
        "API_KEY": "${API_KEY}",
        "TIMEOUT": "30000"
      },
      "enabled": false,
      "description": "API客户端服务器"
    }
  },
  "globalConfig": {
    "timeout": 30000,
    "maxServers": 5,
    "autoRestart": true,
    "logging": {
      "level": "info",
      "file": ".claude/mcp.log"
    }
  }
}
```

### 自定义MCP服务器示例

#### web-scraper.js
```javascript
#!/usr/bin/env node

const { MCPServer } = require('@modelcontextprotocol/sdk');
const axios = require('axios');
const cheerio = require('cheerio');

class WebScraperServer extends MCPServer {
  constructor() {
    super('web-scraper', '1.0.0');
    
    this.addTool('scrape_url', {
      description: '抓取网页内容',
      inputSchema: {
        type: 'object',
        properties: {
          url: { type: 'string', description: '要抓取的URL' },
          selector: { type: 'string', description: 'CSS选择器（可选）' }
        },
        required: ['url']
      }
    });
  }

  async handleTool(name, args) {
    if (name === 'scrape_url') {
      return await this.scrapeUrl(args.url, args.selector);
    }
  }

  async scrapeUrl(url, selector) {
    try {
      const response = await axios.get(url, {
        timeout: 10000,
        headers: {
          'User-Agent': process.env.USER_AGENT || 'Claude-MCP/1.0'
        }
      });

      const $ = cheerio.load(response.data);
      
      if (selector) {
        return $(selector).text().trim();
      }
      
      return {
        title: $('title').text(),
        content: $('body').text().substring(0, 5000),
        links: $('a').map((i, el) => ({
          text: $(el).text().trim(),
          href: $(el).attr('href')
        })).get().slice(0, 20)
      };
    } catch (error) {
      throw new Error(`抓取失败: ${error.message}`);
    }
  }
}

new WebScraperServer().start();
```

---

## 🎨 IDE集成和输出风格配置

### IDE集成配置

#### VSCode 集成 (.vscode/settings.json)
```json
{
  "claude-code.integration": {
    "enabled": true,
    "autoStart": true,
    "showInStatusBar": true,
    "keyBindings": {
      "askClaude": "ctrl+shift+c",
      "explainCode": "ctrl+shift+e",
      "generateTest": "ctrl+shift+t"
    }
  },
  "claude-code.workspace": {
    "syncCursor": true,
    "autoSave": true,
    "highlightEdits": true
  }
}
```

### 输出风格配置

#### styles.json - 自定义输出样式
```json
{
  "styles": {
    "professional": {
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
          "padding": "12px",
          "borderRadius": "6px"
        },
        "table": {
          "border": "1px solid #d1d5db",
          "headerBg": "#f9fafb",
          "padding": "8px 12px"
        }
      },
      "icons": {
        "success": "✅",
        "error": "❌", 
        "warning": "⚠️",
        "info": "ℹ️",
        "task": "📋",
        "code": "💻"
      }
    },
    "minimal": {
      "colors": {
        "primary": "#000000",
        "success": "#008000",
        "error": "#ff0000"
      },
      "formatting": {
        "removeEmojis": true,
        "compactMode": true
      }
    },
    "cyberpunk": {
      "colors": {
        "primary": "#00ff00",
        "secondary": "#ff00ff",
        "accent": "#00ffff"
      },
      "formatting": {
        "useNeonEffects": true,
        "glitchText": false
      }
    }
  }
}
```

---

## 🔐 权限管理和自动化配置

### 完全自动化权限配置

#### permissions-auto.json
```json
{
  "permissions": {
    "mode": "auto",
    "autoConfirm": {
      "enabled": true,
      "maxErrors": 3,
      "pauseOnError": true,
      "cooldownMinutes": 5,
      "errorPatterns": [
        "permission denied",
        "access denied", 
        "file not found",
        "command not found"
      ]
    },
    "allowAll": {
      "tools": true,
      "bashCommands": false
    },
    "allowedTools": [
      "Read", "Write", "Edit", "MultiEdit",
      "Grep", "Glob", "LS",
      "Bash", "Task", "TodoWrite",
      "WebFetch", "WebSearch",
      "NotebookEdit", "BashOutput"
    ],
    "allowedBashCommands": [
      "git *",
      "npm *",
      "yarn *", 
      "python *",
      "node *",
      "docker *",
      "kubectl *",
      "ls *",
      "cat *",
      "grep *",
      "find *",
      "test *",
      "build *"
    ],
    "deniedPatterns": [
      "rm -rf /",
      "sudo rm",
      "format *",
      "del /q",
      "shutdown",
      "reboot"
    ],
    "safeguards": {
      "confirmDestructive": true,
      "backupBeforeEdit": true,
      "dryRunFirst": false,
      "maxFileSize": "10MB",
      "maxConcurrentOperations": 5
    }
  }
}
```

### Hooks 配置 - 错误处理和自动化

#### hooks.json
```json
{
  "hooks": {
    "beforeEdit": {
      "*.py": [
        "python -m py_compile {file}",
        "black --check {file}"
      ],
      "*.js": [
        "node -c {file}",
        "eslint {file}"
      ],
      "*.json": [
        "python -m json.tool {file} > /dev/null"
      ]
    },
    "afterEdit": {
      "*.py": [
        "black {file}",
        "isort {file}"
      ],
      "*.js": [
        "prettier --write {file}"
      ],
      "*.ts": [
        "prettier --write {file}",
        "tsc --noEmit {file}"
      ]
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
        },
        {
          "condition": "file_not_found",
          "action": "create_missing_file"
        }
      ]
    },
    "errorHandling": {
      "pauseAfterErrors": 3,
      "pauseDuration": "5m",
      "notificationMethod": "console",
      "logErrors": true,
      "logFile": ".claude/errors.log"
    }
  }
}
```

---

## 🚨 错误处理和安全配置

### 防止3次错误暂停配置

#### error-handling.json
```json
{
  "errorHandling": {
    "enabled": true,
    "maxConsecutiveErrors": 3,
    "errorWindow": "10m",
    "actions": {
      "onMaxErrors": "pause",
      "pauseDuration": "5m",
      "escalation": [
        {
          "errorCount": 1,
          "action": "log"
        },
        {
          "errorCount": 2, 
          "action": "warn"
        },
        {
          "errorCount": 3,
          "action": "pause"
        },
        {
          "errorCount": 5,
          "action": "disable_auto_mode"
        }
      ]
    },
    "recovery": {
      "autoRecover": true,
      "recoveryDelay": "30s",
      "maxRecoveryAttempts": 3,
      "fallbackMode": "manual"
    },
    "monitoring": {
      "logLevel": "info",
      "logFile": ".claude/errors.log",
      "rotateLog": true,
      "maxLogSize": "10MB"
    }
  }
}
```

### 启动脚本 - 自动配置

#### setup-claude.sh
```bash
#!/bin/bash

# 设置默认配置
CLAUDE_DIR=".claude"
mkdir -p "$CLAUDE_DIR"/{commands,agents,styles,logs}

# 复制配置模板
cp templates/settings.json "$CLAUDE_DIR/settings.json"
cp templates/permissions.json "$CLAUDE_DIR/permissions.json"
cp templates/hooks.json "$CLAUDE_DIR/hooks.json"

# 设置权限
chmod 600 "$CLAUDE_DIR"/*.json

# 初始化环境变量
echo "export CLAUDE_AUTO_CONFIRM=true" >> ~/.bashrc
echo "export CLAUDE_MAX_ERRORS=3" >> ~/.bashrc
echo "export CLAUDE_PROXY=http://192.168.0.98:8800" >> ~/.bashrc

# 创建快捷启动脚本
cat > start-claude.sh << 'EOF'
#!/bin/bash
export CLAUDE_AUTO_CONFIRM=true
export CLAUDE_MAX_ERRORS=3

claude --config .claude/settings.json "$@"
EOF

chmod +x start-claude.sh

echo "Claude Code 配置完成！"
echo "使用 ./start-claude.sh 启动"
```

---

## 📋 完整配置检查清单

### ✅ 基础配置
- [ ] 创建 .claude 目录结构
- [ ] 配置 settings.json
- [ ] 设置 CLAUDE.md 记忆文件
- [ ] 配置 .gitignore 排除敏感文件

### ✅ 权限和安全
- [ ] 配置权限白名单
- [ ] 设置错误处理机制
- [ ] 配置安全防护模式
- [ ] 设置代理（如需要）

### ✅ 高级功能
- [ ] 配置 Agents 代理
- [ ] 设置 MCP 服务器
- [ ] 配置 IDE 集成
- [ ] 自定义输出样式

### ✅ 自动化
- [ ] 设置 Hooks 钩子
- [ ] 配置自动确认模式
- [ ] 设置错误暂停机制
- [ ] 配置自动恢复

---

这个配置指南涵盖了Claude Code的所有主要配置选项，可以帮助你实现完全自动化的开发体验。记住要根据具体项目需求调整配置！