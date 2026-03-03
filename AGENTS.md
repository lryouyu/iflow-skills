# iFlow Skills - 项目上下文文档

## 项目概述

**项目名称**: iFlow Skills  
**项目类型**: iFlow CLI 技能集合  
**项目目标**: 为 iFlow CLI 提供可重用的专业技能扩展，提升开发效率和代码质量

本项目是一个通用的 iFlow CLI 技能库，包含多个专业化的技能（Skills）和命令（Commands），每个技能针对特定的开发场景提供系统化的工作流程和最佳实践指导。项目支持中文界面，并能根据项目环境动态适配不同的代码规范。

## 项目特色

- **通用化设计**: 技能可在不同项目中使用，自动适配项目特定的代码规范
- **智能检测**: 自动检测项目环境，动态加载项目配置和规范
- **中文化支持**: 所有技能和命令均使用中文，提供更好的用户体验
- **双模式运行**: 支持全局安装和项目级安装，灵活适应不同使用场景
- **规范优先**: 强调"先规划，后执行"的开发理念，提高代码质量

## 项目结构

```
iflow-skills/
├── skills/                          # 技能定义目录
│   ├── command-creator/             # 命令创建器技能
│   │   └── SKILL.md                 # 技能定义文件
│   ├── frontend-code-review/        # 前端代码审查技能
│   │   ├── SKILL.md                 # 技能定义文件
│   │   └── references/              # 参考文档
│   │       ├── lvm-standards.md     # 代码标准示例
│   │       └── react-best-practices.md  # React 最佳实践
│   └── spec-mode/                   # 规范优先开发模式技能
│       ├── SKILL.md                 # 技能定义文件
│       ├── README.md                # 技能说明
│       └── examples/                # 示例用例
│           └── shopping-cart/       # 购物车功能示例
│               ├── requirements.md  # 需求文档
│               ├── design.md        # 设计文档
│               └── tasks.md         # 任务清单
├── commands/                        # 命令配置目录
│   ├── command-creator.toml         # 命令创建器配置
│   ├── frontend-code-review.toml    # 前端代码审查配置
│   └── spec-mode.toml               # 规范优先开发模式配置
├── .iflow/                          # iFlow 配置目录（隐藏）
│   ├── project-detection.md         # 项目规范检测逻辑文档
│   └── project.json                 # 项目元数据（自动生成）
├── README.md                        # 项目说明文档
└── AGENTS.md                        # 本文件（项目上下文）
```

## 核心技能说明

### 1. Command Creator (命令创建器)

**技能名称**: `command-creator`  
**用途**: 帮助用户创建自定义 iFlow CLI 命令

**主要功能**:
- 引导用户定义命令描述和提示词
- 支持多种配置格式（TOML 和 Markdown）
- 支持全局和项目级命令安装
- 自动生成命令配置文件

**使用场景**:
- 创建新的自定义命令
- 配置命令定义文件
- 管理已安装的命令

**输出位置**:
- 全局命令: `~/.iflow/commands/`
- 项目命令: `{project}/.iflow/commands/`

**配置格式**:
- **TOML 格式**: 适合复杂命令，支持多行提示词，结构清晰
- **Markdown 格式**: 适合简单命令，格式简洁直观

**特点**:
- 全中文界面，易于理解
- 提供详细的创建向导
- 支持命令分类参考
- 包含验证步骤和最佳实践

### 2. Frontend Code Review (前端代码审查)

**技能名称**: `frontend-code-review`  
**用途**: 对前端代码进行系统性审查，支持动态加载项目规范

**核心特性**:
- **智能环境检测**: 自动判断是项目环境还是全局环境
- **动态规范加载**: 根据项目配置自动加载项目特定的代码规范
- **通用化设计**: 移除了特定项目的硬编码规范，适用于各种前端项目

**审查范围**:
- React/Vue/Angular 等前端框架组件
- TypeScript/JavaScript 文件
- 自定义 Hooks/Composables
- 状态管理代码
- 路由配置
- API 调用层

**审查标准**（动态加载）:
- 代码风格（基于项目的 Prettier 和 ESLint 配置）
- TypeScript 类型安全（基于项目的 tsconfig.json）
- 框架最佳实践（基于项目使用的框架）
- 项目特定的规范（从 AGENTS.md、CONTRIBUTING.md 等文档加载）

**环境检测机制**:
1. 检查是否存在项目配置文件（package.json、tsconfig.json 等）
2. 读取项目的代码规范配置
3. 查找项目规范文档（AGENTS.md、CONTRIBUTING.md 等）
4. 分析现有代码的风格和模式
5. 按优先级合并规范：项目规范 > 技能库规范 > 通用标准

**审查输出**:
- 总体评价（质量等级、主要优点、主要问题）
- 详细问题列表（按优先级排序：严重问题、警告、建议）
- 最佳实践建议
- Git Commit 信息（英文，遵循 Conventional Commits）
- PR 描述模板

**通用代码标准**（当无项目规范时使用）:
- 文件命名规范：组件用 PascalCase，工具函数用 camelCase
- 导入顺序：内置模块 → 外部依赖 → 内部模块 → 相对导入
- 组件结构：Hooks → 副作用 → 事件处理器 → 渲染

**参考文档**:
- `skills/frontend-code-review/references/lvm-standards.md` - 代码标准示例
- `skills/frontend-code-review/references/react-best-practices.md` - React 最佳实践
- `.iflow/project-detection.md` - 项目规范检测逻辑

### 3. Spec Mode (规范优先开发模式)

**技能名称**: `spec-mode`  
**用途**: 系统化的开发工作流，强调"先规划，后执行"

**核心理念**: 在编写代码之前进行充分的规划、需求分析和技术设计

**触发条件**:
- 用户提出新功能需求
- 用户报告需要修复的 Bug
- 任何需要编写代码的任务

**自动产出**:
1. **需求文档** (`.iflow/specs/<feature-name>/requirements.md`)
   - 功能概述（背景、目标、范围）
   - 用户故事（用户角色、使用场景、验收标准）
   - 功能需求（功能点列表、非功能性需求）
   - UI/UX 需求（页面结构、交互流程、国际化）
   - 数据需求（数据结构、API 接口、状态管理）
   - 技术约束、依赖与排期、风险评估

2. **设计文档** (`.iflow/specs/<feature-name>/design.md`)
   - 总体设计（架构概览、技术选型、文件结构）
   - 详细设计（组件设计、数据流、API 设计、路由设计、国际化）
   - 类型定义
   - 实现细节（核心逻辑、边界情况、错误处理、性能优化）
   - 测试策略（单元测试、集成测试、E2E 测试）
   - 代码规范（命名规范、代码风格、注释规范）
   - 部署与发布

3. **任务清单** (`.iflow/specs/<feature-name>/tasks.md`)
   - 任务分解（6 个阶段：准备工作、核心开发、集成与配置、测试与优化、代码质量、部署准备）
   - 每个任务的验收标准
   - 任务依赖关系图
   - 进度跟踪表格
   - 风险与缓解措施

**工作流程**:
1. **阶段 1**: 需求分析 - 理解用户需求，分析现有实现
2. **阶段 2**: 技术设计 - 基于需求设计技术方案
3. **阶段 3**: 任务分解 - 将设计分解为可执行任务
4. **阶段 4**: 用户确认 - 向用户展示文档，等待确认
5. **阶段 5**: 执行开发（可选）- 按照任务清单执行

**特点**:
- 全中文文档输出
- 通用化设计，移除了特定技术栈的硬编码
- 支持根据项目规范动态调整工作流程
- 包含学习与优化机制

**示例**:
- `skills/spec-mode/examples/shopping-cart/` - 购物车功能完整示例
  - `requirements.md` - 需求定义
  - `design.md` - 技术设计
  - `tasks.md` - 实现任务清单

## 命令配置

### 命令列表

项目提供了三个预配置的命令文件，位于 `commands/` 目录：

1. **command-creator.toml**
   - 命令创建器的完整配置
   - 包含详细的工作流程和最佳实践

2. **frontend-code-review.toml**
   - 前端代码审查的完整配置
   - 包含环境检测逻辑和规范加载机制

3. **spec-mode.toml**
   - 规范优先开发模式的完整配置
   - 包含三个阶段的详细工作流程

### 命令安装

**全局安装**:
```bash
# 将命令安装到全局
/commands add command-creator
/commands add frontend-code-review
/commands add spec-mode
```

**项目安装**:
```bash
# 将命令安装到当前项目
# 命令会自动检测项目环境并使用项目规范
```

### 命令管理
```bash
# 查看在线命令列表
/commands online

# 查看命令详情
/commands get <id>

# 列出已安装命令
/commands list

# 删除命令
/commands remove <name>
```

## 项目规范检测机制

### 概述

项目规范检测是通用技能库的核心能力，通过动态检测和加载项目规范，确保技能能够在不同项目中正确工作。

### 检测流程

1. **环境检测**
   - 判断当前是项目环境还是全局环境
   - 通过检测项目配置文件（package.json、tsconfig.json、requirements.txt 等）

2. **配置文件读取**
   - 读取项目的各种配置文件
   - 提取项目的代码规范设置

3. **规范文档查找**
   - 自动查找项目中的规范文档（AGENTS.md、CONTRIBUTING.md、docs/standards.md 等）
   - 按照优先级查找不同位置的规范

4. **代码风格分析**
   - 分析现有代码的导入顺序
   - 识别命名规范
   - 检测状态管理方案

5. **规范合并策略**
   - 按照优先级合并：项目规范 > 技能库规范 > 通用标准
   - 解决规范冲突

### 优先级规则

1. **项目特定规范**（最高优先级）
   - 项目配置文件中的设置
   - 项目规范文档中的规定

2. **技能库参考**（中等优先级）
   - 技能自带的参考文档
   - 通用的最佳实践

3. **通用标准**（最低优先级）
   - 语言/框架官方推荐
   - 行业通用标准

### 支持的项目类型

- **React 项目**: 检测 package.json 中的 react 依赖
- **Vue 项目**: 检测 package.json 中的 vue 依赖
- **Node.js 后端项目**: 检测无前端框架的 Node.js 项目
- **Python 项目**: 检测 requirements.txt 或 pyproject.toml
- **Rust 项目**: 检测 Cargo.toml
- **Go 项目**: 检测 go.mod
- **Java 项目**: 检测 pom.xml 或 build.gradle

### 详细文档

完整的项目规范检测逻辑请参考：
- `.iflow/project-detection.md` - 详细的技术实现文档

## 常用命令

### Git 命令
```bash
# 查看未暂存的更改
git diff HEAD

# 查看已暂存的更改
git diff --staged

# 查看特定文件的更改
git diff HEAD -- <file-path>

# 查看最近 N 次提交的更改
git diff HEAD~N HEAD

# 比较分支
git diff main...HEAD
```

### 命令管理
```bash
# 查看在线命令列表
/commands online

# 查看命令详情
/commands get <id>

# 安装命令
/commands add <id>

# 列出已安装命令
/commands list

# 删除命令
/commands remove <name>
```

## 项目规范要点

### Conventional Commits 规范

所有 Git 提交信息必须使用英文，遵循以下格式：

```
<type>(<scope>): <subject>
```

**类型（type）**:
- `feat`: 新功能
- `fix`: Bug 修复
- `refactor`: 重构（不改变功能）
- `perf`: 性能优化
- `style`: 代码风格改进
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建/CI 相关
- `build`: 构建系统或外部依赖变更

**作用域（scope）**（根据项目调整）:
- `frontend`: 前端代码
- `backend`: 后端代码
- `ui`: UI 组件
- `api`: API 层
- 或其他项目特定的作用域

**主题（subject）规则**:
- 使用祈使语气（"add" 而非 "added"）
- 首字母小写
- 结尾不加句号
- 限制在 50 字符以内

**示例**:
```
feat(frontend): add sidebar collapse feature

refactor(ui): improve component structure

fix(api): correct response handling
```

### 通用开发最佳实践

1. **组件结构**: Hooks → 副作用 → 事件处理器 → 渲染
2. **Hooks 使用**: 必须在顶层调用，依赖数组完整
3. **性能优化**: 使用 `useCallback`、`useMemo`、`React.memo`
4. **错误处理**: 完整的 try-catch 和用户反馈
5. **类型安全**: 必须定义 Props 接口，避免使用 `any`

### 文件命名规范

- **组件**: PascalCase（如 `UserList.tsx`）
- **工具函数**: camelCase（如 `formatDate.ts`）
- **常量**: UPPER_SNAKE_CASE（如 `API_BASE_URL`）
- **自定义 Hooks**: camelCase + `use` 前缀（如 `useAuth.ts`）
- **类型定义**: camelCase（如 `common.ts`）

### 导入顺序

1. Node.js 内置模块
2. 外部依赖
3. 内部模块（使用项目别名）
4. 相对导入

## 开发工作流

### 前端代码审查流程
1. 获取代码变更（git diff）
2. 检测项目环境（全局或项目级）
3. 加载项目规范配置
4. 识别变更类型（新增、修改、删除）
5. 执行自动化检查（ESLint、Prettier、TypeScript）
6. 执行手动审查（代码风格、类型安全、最佳实践）
7. 生成审查报告（中文）
8. 确定审批状态
9. 生成 Commit 信息（英文）和 PR 描述

### 规范优先开发流程
1. 需求分析（生成 requirements.md）
2. 技术设计（生成 design.md）
3. 任务分解（生成 tasks.md）
4. 用户确认
5. 执行开发（按照 tasks.md）
6. 测试与优化
7. 代码提交

### 命令创建流程
1. 收集用户需求
2. 选择配置格式（TOML 或 Markdown）
3. 生成配置文件
4. 确认安装位置（全局或项目级）
5. 生成命令文件
6. 验证命令配置

## 质量保证

### 代码审查检查清单
- [ ] 代码符合项目的 Prettier 和 ESLint 规则
- [ ] TypeScript 类型定义完整且正确
- [ ] 导入顺序正确
- [ ] 组件结构清晰，职责单一
- [ ] Hooks 使用规范
- [ ] 错误处理完善
- [ ] 性能优化到位
- [ ] 无安全漏洞
- [ ] 可访问性考虑
- [ ] 代码注释充分（复杂逻辑）

### 提交前检查
- [ ] 所有文件通过 ESLint 检查
- [ ] 所有文件符合 Prettier 格式
- [ ] TypeScript 编译通过
- [ ] 所有组件有 Props 接口
- [ ] 所有 Hooks 有完整依赖数组
- [ ] 使用 useCallback 缓存传递给子组件的函数
- [ ] 使用 useMemo 缓存昂贵的计算
- [ ] 副作用正确清理
- [ ] 错误处理完整

## 参考资源

### 内部文档
- **项目上下文**: `AGENTS.md`（本文件）
- **项目说明**: `README.md`
- **项目规范检测**: `.iflow/project-detection.md`
- **代码标准示例**: `skills/frontend-code-review/references/lvm-standards.md`
- **React 最佳实践**: `skills/frontend-code-review/references/react-best-practices.md`

### 外部资源
- [React 官方文档](https://react.dev/)
- [TypeScript 官方文档](https://www.typescriptlang.org/)
- [Vue 官方文档](https://vuejs.org/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Prettier 文档](https://prettier.io/)
- [ESLint 文档](https://eslint.org/)

## 常见问题

### Q: 如何创建一个新的自定义命令？
A: 使用 `command-creator` 技能，按照提示定义命令描述和提示词，选择配置格式（TOML 或 Markdown），然后安装到全局或项目级。

### Q: 如何进行前端代码审查？
A: 使用 `frontend-code-review` 技能，提供 git diff 输出。技能会自动检测项目环境，加载项目特定的代码规范，然后执行系统性审查，生成详细的审查报告和 Commit 信息。

### Q: 如何使用规范优先开发模式？
A: 当提出新功能需求或 Bug 修复时，`spec-mode` 技能会自动触发，生成需求文档、设计文档和任务清单，等待用户确认后开始开发。

### Q: 代码审查输出使用什么语言？
A: 除了 Git Commit 信息必须使用英文（遵循 Conventional Commits 规范）外，所有审查报告内容都使用中文。

### Q: 如何确保代码符合项目规范？
A: 使用 `frontend-code-review` 技能进行审查，它会自动检测项目环境，加载项目特定的代码规范，然后检查代码是否符合这些规范。如果项目没有明确规范，则使用通用的最佳实践。

### Q: 全局安装和项目安装有什么区别？
A: 全局安装的命令在所有项目中都使用通用的最佳实践；项目安装的命令会自动检测项目环境，使用项目特定的代码规范。项目命令的优先级高于全局命令。

### Q: 支持哪些项目类型？
A: 支持多种项目类型，包括 React、Vue、Node.js、Python、Rust、Go、Java 等。技能会自动检测项目类型并加载相应的规范。

## 维护指南

### 添加新技能
1. 在 `skills/` 目录下创建新的技能目录
2. 创建 `SKILL.md` 文件，定义技能元数据和工作流程
3. 确保所有内容使用中文
4. 移除客制化内容，使其成为通用技能
5. 如需要，创建参考文档和示例
6. 在 `commands/` 目录创建对应的 command 配置文件
7. 更新本文档的"核心技能说明"部分

### 更新现有技能
1. 修改对应的 `SKILL.md` 文件
2. 更新对应的 command 配置文件
3. 更新参考文档（如有）
4. 更新示例（如有）
5. 测试技能功能
6. 确保中文化完整性

### 更新项目规范检测
1. 修改 `.iflow/project-detection.md` 文档
2. 添加新的项目类型支持
3. 更新配置文件读取逻辑
4. 测试环境检测功能
5. 确保向后兼容性

### 更新项目规范
1. 更新通用代码标准
2. 更新本文档的"项目规范要点"部分
3. 确保所有技能都遵循新规范
4. 更新参考文档

## 版本历史

- **v1.0.0** (2025-03-03): 初始版本
  - 包含 command-creator、frontend-code-review、spec-mode 三个核心技能
  - 提供完整的 command 配置文件
  - 实现项目规范检测机制
  - 全部内容中文化
  - 移除客制化内容，实现通用化设计
  - 支持全局和项目级安装

---

**最后更新**: 2025-03-03  
**维护者**: iFlow Skills Team  
**仓库**: https://github.com/lryouyu/iflow-skills.git