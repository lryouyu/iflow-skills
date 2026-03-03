# iFlow Skills - 项目上下文文档

## 项目概述

**项目名称**: iFlow Skills  
**项目类型**: iFlow CLI 技能集合  
**项目目标**: 为 iFlow CLI 提供可重用的专业技能扩展，提升开发效率和代码质量

本项目是一个 iFlow CLI 技能库，包含多个专业化的技能（Skills），每个技能针对特定的开发场景提供系统化的工作流程和最佳实践指导。

## 项目结构

```
iflow-skills/
├── skills/                          # 技能定义目录
│   ├── command-creator/             # 命令创建器技能
│   │   └── SKILL.md                 # 技能定义文件
│   ├── frontend-code-review/        # 前端代码审查技能
│   │   ├── SKILL.md                 # 技能定义文件
│   │   └── references/              # 参考文档
│   │       ├── lvm-standards.md     # LVM 项目代码标准
│   │       └── react-best-practices.md  # React 最佳实践
│   ├── spec-mode/                   # 规范优先开发模式技能
│   │   ├── SKILL.md                 # 技能定义文件
│   │   ├── README.md                # 技能说明
│   │   └── examples/                # 示例用例
│   │       └── shopping-cart/       # 购物车功能示例
│   │           ├── requirements.md  # 需求文档
│   │           ├── design.md        # 设计文档
│   │           └── tasks.md         # 任务清单
│   └── frontend-code-review.md      # 前端代码审查简化版
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
- **TOML 格式**: 适合复杂命令，支持多行提示词
- **Markdown 格式**: 适合简单命令，格式简洁

### 2. Frontend Code Review (前端代码审查)

**技能名称**: `frontend-code-review`  
**用途**: 对 LVM 项目的前端代码进行系统性审查

**技术栈**: Tauri + React + TypeScript + Ant Design + Redux Toolkit

**审查范围**:
- React 组件（`.tsx`, `.jsx`）
- TypeScript/JavaScript 文件（`.ts`, `.js`）
- 自定义 Hooks
- Redux Slices
- 路由配置
- API 调用层

**审查标准**:
- 代码风格（Prettier + ESLint）
- TypeScript 类型安全
- React 最佳实践
- Tauri API 调用规范
- 状态管理规范
- 性能优化
- 安全性检查

**关键规范**:
- **导入顺序**: Node.js 内置 → 外部依赖 → 内部模块（@/）→ 相对导入
- **Props 定义**: 必须使用接口定义，命名格式为 `ComponentNameProps`
- **Tauri API**: 必须使用 `safeInvoke` 封装，指定返回类型
- **Hooks 使用**: 必须在顶层调用，依赖数组完整
- **性能优化**: 使用 `useCallback`、`useMemo`、`React.memo`

**代码风格配置**:
- 打印宽度: 100 字符
- 缩进: 2 空格
- 引号: 单引号（JSX 属性使用双引号）
- 分号: 必须使用
- 尾随逗号: ES5 风格

**审查输出**:
- 总体评价（质量等级、主要优点、主要问题）
- 详细问题列表（按优先级排序）
- 最佳实践建议
- Git Commit 信息（英文，遵循 Conventional Commits）
- PR 描述模板

**参考文档**:
- `skills/frontend-code-review/references/lvm-standards.md` - LVM 项目代码标准
- `skills/frontend-code-review/references/react-best-practices.md` - React 最佳实践

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
   - 功能概述
   - 用户故事
   - 功能需求
   - UI/UX 需求
   - 数据需求
   - 技术约束
   - 依赖与排期
   - 风险评估

2. **设计文档** (`.iflow/specs/<feature-name>/design.md`)
   - 总体设计
   - 详细设计（组件、数据流、API、路由、国际化）
   - 类型定义
   - 实现细节
   - 测试策略
   - 代码规范

3. **任务清单** (`.iflow/specs/<feature-name>/tasks.md`)
   - 任务分解（6 个阶段）
   - 验收标准
   - 任务依赖关系
   - 进度跟踪

**工作流程**:
1. **阶段 1**: 需求分析 - 理解用户需求，分析现有实现
2. **阶段 2**: 技术设计 - 基于需求设计技术方案
3. **阶段 3**: 任务分解 - 将设计分解为可执行任务
4. **阶段 4**: 用户确认 - 向用户展示文档，等待确认
5. **阶段 5**: 执行开发（可选）- 按照任务清单执行

**示例**:
- `skills/spec-mode/examples/shopping-cart/` - 购物车功能完整示例
  - `requirements.md` - 需求定义
  - `design.md` - 技术设计
  - `tasks.md` - 实现任务清单

**学习与优化**:
- 收集用户反馈和纠正
- 更新 Skill 模板
- 沉淀项目知识

## 技术栈参考

### 前端技术栈（LVM 项目）
- **框架**: React 19.1.0
- **UI 组件库**: Ant Design 6.3.0
- **状态管理**: Redux Toolkit 2.11.2
- **路由**: React Router DOM 7.13.0
- **国际化**: i18next 25.8.13
- **桌面框架**: Tauri
- **构建工具**: Bun

### 代码质量工具
- **类型检查**: TypeScript
- **代码格式化**: Prettier
- **代码检查**: ESLint
- **测试框架**: Vitest + React Testing Library（建议）

### 文件命名规范
- 组件: PascalCase（如 `VersionTable.tsx`）
- 工具函数: camelCase（如 `store.ts`）
- 常量/枚举: PascalCase（如 `LangEnum`）
- 自定义 Hook: camelCase + `use` 前缀（如 `useDownload.ts`）
- 类型定义: camelCase（如 `common.ts`）

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

### 代码质量检查
```bash
# 运行 ESLint 检查
bun run lint

# 自动修复 ESLint 问题
bun run lint:fix

# 运行 Prettier 格式化
bun run format

# TypeScript 类型检查
bun run build
# 或
tsc --noEmit
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

**作用域（scope）**:
- `frontend`: 前端代码
- `rust`: 后端 Rust 代码
- `i18n`: 国际化
- `types`: 类型定义
- `api`: API 层
- `ui`: UI 组件

**主题（subject）规则**:
- 使用祈使语气（"add" 而非 "added"）
- 首字母小写
- 结尾不加句号
- 限制在 50 字符以内

**示例**:
```
feat(frontend): add sidebar collapse feature

refactor(ui): improve version table structure

fix(types): correct prop interface definition
```

### React 开发最佳实践
1. **组件结构**: Hooks → 副作用 → 事件处理器 → 渲染
2. **Hooks 使用**: 必须在顶层调用，依赖数组完整
3. **性能优化**: 使用 `useCallback`、`useMemo`、`React.memo`
4. **错误处理**: 完整的 try-catch 和用户反馈
5. **类型安全**: 必须定义 Props 接口，避免使用 `any`

### Tauri API 调用规范
1. **使用 safeInvoke**: 必须使用 `safeInvoke` 封装 Tauri 命令
2. **类型安全**: 显式指定返回类型（如 `safeInvoke<VersionResult>`）
3. **事件监听**: 必须清理事件监听器
4. **错误处理**: 完整的错误处理和用户提示

### 国际化规范
1. **所有用户可见文本**: 必须使用 `t()` 函数
2. **翻译 key**: 使用点号分隔的命名空间（如 `cart.addToCart`）
3. **双语支持**: 中英文双语定义
4. **参数化**: 使用 `{placeholder}` 语法支持参数

## 开发工作流

### 前端代码审查流程
1. 获取代码变更（git diff）
2. 识别变更类型（新增、修改、删除）
3. 执行自动化检查（ESLint、Prettier、TypeScript）
4. 执行手动审查（代码风格、类型安全、最佳实践）
5. 生成审查报告
6. 确定审批状态
7. 生成 Commit 信息和 PR 描述

### 规范优先开发流程
1. 需求分析（生成 requirements.md）
2. 技术设计（生成 design.md）
3. 任务分解（生成 tasks.md）
4. 用户确认
5. 执行开发（按照 tasks.md）
6. 测试与优化
7. 代码提交

## 质量保证

### 代码审查检查清单
- [ ] 代码符合 Prettier 和 ESLint 规则
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
- **LVM 代码标准**: `skills/frontend-code-review/references/lvm-standards.md`
- **React 最佳实践**: `skills/frontend-code-review/references/react-best-practices.md`

### 外部资源
- [React 官方文档](https://react.dev/)
- [TypeScript 官方文档](https://www.typescriptlang.org/)
- [Ant Design 文档](https://ant.design/)
- [Redux Toolkit 文档](https://redux-toolkit.js.org/)
- [Tauri 官方文档](https://tauri.app/)
- [i18next 文档](https://www.i18next.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)

## 常见问题

### Q: 如何创建一个新的自定义命令？
A: 使用 `command-creator` 技能，按照提示定义命令描述和提示词，选择配置格式（TOML 或 Markdown），然后安装到全局或项目级。

### Q: 如何进行前端代码审查？
A: 使用 `frontend-code-review` 技能，提供 git diff 输出，技能将执行系统性审查，生成详细的审查报告和 Commit 信息。

### Q: 如何使用规范优先开发模式？
A: 当提出新功能需求或 Bug 修复时，`spec-mode` 技能会自动触发，生成需求文档、设计文档和任务清单，等待用户确认后开始开发。

### Q: 代码审查输出使用什么语言？
A: 除了 Git Commit 信息必须使用英文（遵循 Conventional Commits 规范）外，所有审查报告内容都使用中文。

### Q: 如何确保代码符合项目规范？
A: 使用 `frontend-code-review` 技能进行审查，它会检查代码是否符合 Prettier、ESLint、TypeScript 和项目特定的代码规范。

## 维护指南

### 添加新技能
1. 在 `skills/` 目录下创建新的技能目录
2. 创建 `SKILL.md` 文件，定义技能元数据和工作流程
3. 如需要，创建参考文档和示例
4. 更新本文档的"核心技能说明"部分

### 更新现有技能
1. 修改对应的 `SKILL.md` 文件
2. 更新参考文档（如有）
3. 更新示例（如有）
4. 测试技能功能

### 更新项目规范
1. 更新 `skills/frontend-code-review/references/lvm-standards.md`
2. 更新本文档的"项目规范要点"部分
3. 确保所有技能都遵循新规范

## 版本历史

- **v1.0.0** (2025-01-XX): 初始版本
  - 包含 command-creator、frontend-code-review、spec-mode 三个核心技能
  - 提供完整的 LVM 项目代码标准和 React 最佳实践参考
  - 包含购物车功能示例

---

**最后更新**: 2025-03-03  
**维护者**: iFlow Skills Team