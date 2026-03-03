# iFlow Skills

<div align="center">

**为 iFlow CLI 提供可重用的专业技能扩展**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![iFlow CLI](https://img.shields.io/badge/iFlow-CLI-green.svg)](https://github.com/lryouyu/iflow-cli)

</div>

## 项目简介

iFlow Skills 是一个通用的 iFlow CLI 技能库，包含多个专业化的技能（Skills）和命令（Commands），每个技能针对特定的开发场景提供系统化的工作流程和最佳实践指导。

### 核心特性

- 🎯 **通用化设计** - 技能可在不同项目中使用，自动适配项目特定的代码规范
- 🔍 **智能检测** - 自动检测项目环境，动态加载项目配置和规范
- 🌏 **中文化支持** - 所有技能和命令均使用中文，提供更好的用户体验
- 🔄 **双模式运行** - 支持全局安装和项目级安装，灵活适应不同使用场景
- 📋 **规范优先** - 强调"先规划，后执行"的开发理念，提高代码质量

## 核心技能

### 1. Command Creator（命令创建器）

帮助用户创建自定义 iFlow CLI 命令。

**主要功能**：
- 引导用户定义命令描述和提示词
- 支持 TOML 和 Markdown 两种配置格式
- 支持全局和项目级命令安装
- 自动生成命令配置文件

**使用场景**：
- 创建新的自定义命令
- 配置命令定义文件
- 管理已安装的命令

**调用方式**：
```
/commands online        # 查看在线命令列表
/commands add <id>      # 安装命令
/commands list          # 列出已安装命令
```

### 2. Frontend Code Review（前端代码审查）

对前端代码进行系统性审查，支持动态加载项目规范。

**核心特性**：
- **智能环境检测** - 自动判断是项目环境还是全局环境
- **动态规范加载** - 根据项目配置自动加载项目特定的代码规范
- **通用化设计** - 移除了特定项目的硬编码规范，适用于各种前端项目

**审查范围**：
- React/Vue/Angular 等前端框架组件
- TypeScript/JavaScript 文件
- 自定义 Hooks/Composables
- 状态管理代码
- 路由配置
- API 调用层

**审查输出**：
- 总体评价（质量等级、主要优点、主要问题）
- 详细问题列表（按优先级排序：严重问题、警告、建议）
- 最佳实践建议
- Git Commit 信息（英文，遵循 Conventional Commits）
- PR 描述模板

### 3. Spec Mode（规范优先开发模式）

系统化的开发工作流，强调"先规划，后执行"。

**核心理念**：在编写代码之前进行充分的规划、需求分析和技术设计

**自动产出**：
1. **需求文档** (`.iflow/specs/<feature-name>/requirements.md`)
   - 功能概述、用户故事、功能需求
   - UI/UX 需求、数据需求
   - 技术约束、依赖与排期、风险评估

2. **设计文档** (`.iflow/specs/<feature-name>/design.md`)
   - 总体设计（架构、技术选型、文件结构）
   - 详细设计（组件、数据流、API、路由）
   - 类型定义、实现细节、测试策略

3. **任务清单** (`.iflow/specs/<feature-name>/tasks.md`)
   - 任务分解（6 个阶段）
   - 每个任务的验收标准
   - 任务依赖关系图、进度跟踪

**触发条件**：
- 用户提出新功能需求
- 用户报告需要修复的 Bug
- 任何需要编写代码的任务

## 项目结构

```
iflow-skills/
├── skills/                          # 技能定义目录
│   ├── command-creator/             # 命令创建器技能
│   │   └── SKILL.md
│   ├── frontend-code-review/        # 前端代码审查技能
│   │   ├── SKILL.md
│   │   └── references/              # 参考文档
│   │       └── react-best-practices.md
│   └── spec-mode/                   # 规范优先开发模式技能
│       ├── SKILL.md
│       ├── README.md
│       └── examples/                # 示例用例
│           └── shopping-cart/       # 购物车功能示例
│               ├── requirements.md
│               ├── design.md
│               └── tasks.md
├── commands/                        # 命令配置目录
│   ├── command-creator.toml
│   ├── frontend-code-review.toml
│   └── spec-mode.toml
├── .iflow/                          # iFlow 配置目录
│   ├── project-detection.md         # 项目规范检测逻辑
│   └── project.json
├── AGENTS.md                        # 项目上下文文档
└── README.md                        # 本文件
```

## 快速开始

### 安装技能

#### 方式一：全局安装

将命令安装到全局，所有项目都可以使用：

```bash
/commands add command-creator
/commands add frontend-code-review
/commands add spec-mode
```

#### 方式二：项目安装

将命令安装到当前项目，命令会自动检测项目环境并使用项目规范：

```bash
# 在项目根目录执行
/commands add <command-name>
```

### 使用技能

#### 使用 Command Creator

```
/commands online        # 查看在线命令列表
/commands get <id>      # 查看命令详情
/commands list          # 列出已安装命令
```

#### 使用 Frontend Code Review

提供 git diff 输出进行代码审查：

```bash
# 查看未暂存的更改
git diff HEAD

# 查看已暂存的更改
git diff --staged
```

技能会自动检测项目环境，加载项目特定的代码规范，然后执行系统性审查。

#### 使用 Spec Mode

当提出新功能需求或 Bug 修复时，技能会自动触发：

```
用户：我想要实现一个购物车功能
```

技能会自动生成需求文档、设计文档和任务清单，等待用户确认后开始开发。

## 项目规范检测

本项目实现了智能的项目规范检测机制，确保技能能够在不同项目中正确工作。

### 检测流程

1. **环境检测** - 判断当前是项目环境还是全局环境
2. **配置文件读取** - 读取项目的各种配置文件
3. **规范文档查找** - 自动查找项目中的规范文档
4. **代码风格分析** - 分析现有代码的风格和模式
5. **规范合并** - 按优先级合并规范

### 优先级规则

1. **项目特定规范**（最高优先级）
2. **技能库参考**（中等优先级）
3. **通用标准**（最低优先级）

### 支持的项目类型

- React 项目
- Vue 项目
- Node.js 后端项目
- Python 项目
- Rust 项目
- Go 项目
- Java 项目

## 代码规范

### Conventional Commits 规范

所有 Git 提交信息必须使用英文，遵循以下格式：

```
<type>(<scope>): <subject>
```

**类型（type）**：
- `feat` - 新功能
- `fix` - Bug 修复
- `refactor` - 重构（不改变功能）
- `perf` - 性能优化
- `style` - 代码风格改进
- `docs` - 文档更新
- `test` - 测试相关
- `chore` - 构建/CI 相关
- `build` - 构建系统或外部依赖变更

**示例**：
```
feat(frontend): add sidebar collapse feature
refactor(ui): improve component structure
fix(api): correct response handling
```

### 文件命名规范

- **组件**：PascalCase（如 `UserList.tsx`）
- **工具函数**：camelCase（如 `formatDate.ts`）
- **常量**：UPPER_SNAKE_CASE（如 `API_BASE_URL`）
- **自定义 Hooks**：camelCase + `use` 前缀（如 `useAuth.ts`）
- **类型定义**：camelCase（如 `common.ts`）

### 导入顺序

1. Node.js 内置模块
2. 外部依赖
3. 内部模块（使用项目别名）
4. 相对导入

## 贡献指南

欢迎贡献！请遵循以下步骤：

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'feat: add some amazing feature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

## 开发指南

### 添加新技能

1. 在 `skills/` 目录下创建新的技能目录
2. 创建 `SKILL.md` 文件，定义技能元数据和工作流程
3. 确保所有内容使用中文
4. 移除客制化内容，使其成为通用技能
5. 在 `commands/` 目录创建对应的 command 配置文件
6. 更新 `AGENTS.md` 文档

### 更新现有技能

1. 修改对应的 `SKILL.md` 文件
2. 更新对应的 command 配置文件
3. 更新参考文档（如有）
4. 更新示例（如有）
5. 测试技能功能

## 文档

- [项目上下文](AGENTS.md) - 项目的详细上下文信息
- [项目规范检测](.iflow/project-detection.md) - 项目规范检测逻辑文档
- [React 最佳实践](skills/frontend-code-review/references/react-best-practices.md)

## 常见问题

### Q: 如何创建一个新的自定义命令？
A: 使用 `command-creator` 技能，按照提示定义命令描述和提示词，选择配置格式（TOML 或 Markdown），然后安装到全局或项目级。

### Q: 如何进行前端代码审查？
A: 使用 `frontend-code-review` 技能，提供 git diff 输出。技能会自动检测项目环境，加载项目特定的代码规范，然后执行系统性审查。

### Q: 代码审查输出使用什么语言？
A: 除了 Git Commit 信息必须使用英文（遵循 Conventional Commits 规范）外，所有审查报告内容都使用中文。

### Q: 全局安装和项目安装有什么区别？
A: 全局安装的命令在所有项目中都使用通用的最佳实践；项目安装的命令会自动检测项目环境，使用项目特定的代码规范。

### Q: 支持哪些项目类型？
A: 支持多种项目类型，包括 React、Vue、Node.js、Python、Rust、Go、Java 等。

## 版本历史

- **v1.0.0** (2025-03-03)
  - 初始版本发布
  - 包含 command-creator、frontend-code-review、spec-mode 三个核心技能
  - 提供完整的 command 配置文件
  - 实现项目规范检测机制
  - 全部内容中文化
  - 移除客制化内容，实现通用化设计

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 联系方式

- 仓库：https://github.com/lryouyu/iflow-skills.git
- 问题反馈：[GitHub Issues](https://github.com/lryouyu/iflow-skills/issues)

---

**最后更新**：2025-03-03