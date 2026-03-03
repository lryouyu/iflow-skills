---
name: spec-mode
description: Specification-First Development Mode - Systematic planning and execution workflow for new features and bug fixes. Automatically generates spec documents (requirements, design, tasks) and enforces plan-before-code discipline. Triggers on new feature requests or bug reports.
---

# Spec Mode (规范优先开发模式)

## 概述

Spec Mode 是一种系统化的开发工作流,强调在编写代码之前先进行充分的规划、需求分析和技术设计。通过生成规范文档来确保开发方向的正确性,减少返工,提高代码质量。

**核心理念**: "先规划,后执行"

**触发时机**:
- 用户提出新功能需求
- 用户报告需要修复的 Bug
- 任何需要编写代码的任务

**自动产出**:
- `.iflow/specs/<feature-name>/requirements.md` - 需求定义文档
- `.iflow/specs/<feature-name>/design.md` - 技术设计文档
- `.iflow/specs/<feature-name>/tasks.md` - 实现任务清单

---

## 工作流程

### 阶段 1: 需求分析 (Requirements Analysis)

#### 1.1 理解用户需求
与用户深入沟通,明确:
- **功能目标**: 这个功能要解决什么问题?
- **用户场景**: 谁会使用?在什么场景下使用?
- **验收标准**: 怎么判断功能是否完成?
- **优先级**: P0(必须有)/P1(应该有)/P2(可以有)
- **依赖关系**: 是否依赖其他功能或资源?

#### 1.2 分析现有实现
在制定新计划前,必须:
1. 阅读相关功能的现有代码
2. 理解项目架构和代码规范
3. 识别可复用的组件和模式
4. 找出潜在的技术债务

**检查清单**:
- [ ] 阅读 `AGENTS.md` 了解项目上下文
- [ ] 查看相关功能的实现代码
- [ ] 理解项目的代码风格和规范
- [ ] 识别可复用的组件/工具函数
- [ ] 检查是否已有类似功能实现

#### 1.3 生成需求文档
创建 `requirements.md`,包含以下内容:

```markdown
# [功能名称] 需求定义

## 1. 功能概述

### 1.1 背景
[为什么需要这个功能?解决了什么问题?]

### 1.2 目标
[功能的主要目标是什么?]

### 1.3 范围
- **包含**: [包含的功能点]
- **不包含**: [明确不包含的功能点]

## 2. 用户故事

### 2.1 用户角色
[描述使用该功能的用户角色]

### 2.2 使用场景
**场景 1**: [描述具体场景]
- 用户输入: [用户做什么]
- 系统响应: [系统如何响应]
- 预期结果: [用户期望得到什么]

### 2.3 验收标准
- [ ] 验收标准 1
- [ ] 验收标准 2
- [ ] 验收标准 3

## 3. 功能需求

### 3.1 功能点列表
| ID | 功能描述 | 优先级 | 复杂度 |
|----|---------|-------|--------|
| FR-01 | [功能描述] | P0 | 低/中/高 |
| FR-02 | [功能描述] | P1 | 低/中/高 |

### 3.2 非功能性需求
- **性能**: [性能要求]
- **安全性**: [安全要求]
- **可访问性**: [无障碍要求]
- **兼容性**: [浏览器/平台兼容性]

## 4. UI/UX 需求

### 4.1 页面/组件结构
[描述涉及的页面和组件]

### 4.2 交互流程
[描述用户操作流程,可配合流程图]

### 4.3 国际化需求
- 是否需要支持多语言: [是/否]
- 需要的翻译 key: [列出所有需要翻译的文本]

## 5. 数据需求

### 5.1 数据结构
[描述涉及的数据类型和结构]

### 5.2 API 接口
| 接口名称 | 方法 | 描述 | 参数 | 返回值 |
|---------|------|------|------|--------|
| [接口名] | GET/POST/PUT/DELETE | [描述] | [参数] | [返回值] |

### 5.3 状态管理
- 是否需要全局状态: [是/否]
- 需要管理的状态字段: [列出字段]

## 6. 技术约束

- 框架/库限制: [技术栈要求]
- 浏览器兼容性: [支持的浏览器版本]
- 第三方依赖: [可用的第三方库]

## 7. 依赖与排期

### 7.1 前置依赖
- [依赖 1]: [状态]
- [依赖 2]: [状态]

### 7.2 预估工作量
| 任务 | 预估工时 |
|------|---------|
| 需求分析 | [X]h |
| 设计 | [X]h |
| 开发 | [X]h |
| 测试 | [X]h |
| **总计** | **[X]h** |

## 8. 风险评估

| 风险 | 影响 | 概率 | 应对措施 |
|------|------|------|---------|
| [风险描述] | 高/中/低 | 高/中/低 | [应对方案] |

## 9. 参考资源

- 相关文档: [链接]
- 设计稿: [链接]
- 类似实现: [代码路径]
```

---

### 阶段 2: 技术设计 (Technical Design)

#### 2.1 架构设计
基于需求和现有代码分析,设计技术方案:

**设计原则**:
- 遵循项目现有架构
- 复用现有组件和工具
- 保持代码风格一致
- 考虑可扩展性和可维护性

#### 2.2 生成设计文档
创建 `design.md`,包含以下内容:

```markdown
# [功能名称] 技术设计

## 1. 总体设计

### 1.1 架构概览
[描述功能在整体架构中的位置,可配合架构图]

### 1.2 技术选型
- 前端框架: React 19.1.0
- UI 组件库: Ant Design 6.3.0
- 状态管理: Redux Toolkit (如需要)
- 路由: React Router DOM 7.13.0
- 国际化: i18next 25.8.13

### 1.3 文件结构
```
src/
├── features/
│   └── [feature-name]/
│       ├── components/       # 功能相关组件
│       ├── pages/           # 页面组件
│       ├── hooks/           # 自定义 Hooks
│       ├── api/             # API 调用
│       ├── types/           # 类型定义
│       └── utils/           # 工具函数
├── shared/
│   └── components/          # 可复用组件(如需要)
└── core/
    ├── constants/           # 常量定义(如需要)
    └── types/               # 共享类型(如需要)
```

## 2. 详细设计

### 2.1 组件设计

#### 新增组件
| 组件名 | 文件路径 | 职责 | Props | 依赖 |
|--------|---------|------|-------|------|
| [ComponentName] | [path] | [描述] | [Props接口] | [依赖] |

#### 复用组件
- [ComponentName] - 从 [路径] 复用
- [ComponentName] - 从 [路径] 复用

### 2.2 数据流设计

#### 2.2.1 状态管理
**本地状态**:
- 使用 `useState` 管理组件内部状态

**全局状态**(如需要):
```typescript
// features/[feature-name]/[feature-name]Slice.ts
interface [FeatureName]State {
  // 状态字段
}

const [featureName]Slice = createSlice({
  name: '[featureName]',
  initialState,
  reducers: {
    // actions
  },
});
```

#### 2.2.2 数据流向
```
用户操作 → 组件事件 → API调用 → 后端处理 → 更新状态 → UI渲染
```

### 2.3 API 设计

#### 2.3.1 API 端点
如果需要后端支持,定义 API 接口:

| 端点 | 方法 | 描述 | 请求体 | 响应体 |
|------|------|------|--------|--------|
| /api/[endpoint] | GET/POST/PUT/DELETE | [描述] | [类型] | [类型] |

#### 2.3.2 API 调用封装
```typescript
// features/[feature-name]/api/index.ts
import { safeInvoke } from '@/api/tauri';
import { CommandEnum } from '@/core/constants/enum';

export const [functionName] = async (params: ParamsType): Promise<ResponseType> => {
  return safeInvoke<ResponseType>(CommandEnum.COMMAND_NAME, params);
};
```

### 2.4 路由设计
```typescript
// app/routes/index.tsx
{
  path: '[route-path]',
  element: <[PageComponent] />,
}
```

### 2.5 国际化设计

#### 新增翻译 keys
```json
// features/i18n/locales/zh.json
{
  "[featureName]": {
    "[key1]": "中文文本",
    "[key2]": "中文文本"
  }
}

// features/i18n/locales/en.json
{
  "[featureName]": {
    "[key1]": "English text",
    "[key2]": "English text"
  }
}
```

#### 使用方式
```typescript
const { t } = useTranslation();
t('[featureName].[key1]');
```

## 3. 类型定义

### 3.1 数据类型
```typescript
// features/[feature-name]/types/index.ts
export interface [DataType] {
  // 字段定义
}

export type [UnionType] = 'option1' | 'option2';
```

### 3.2 Props 类型
```typescript
// features/[feature-name]/components/[ComponentName]/index.tsx
export interface [ComponentName]Props {
  // Props 定义
}
```

## 4. 实现细节

### 4.1 核心逻辑
[描述核心算法或业务逻辑]

### 4.2 边界情况处理
| 场景 | 处理方式 |
|------|---------|
| [场景1] | [处理逻辑] |
| [场景2] | [处理逻辑] |

### 4.3 错误处理
```typescript
try {
  // API 调用
} catch (error) {
  console.error('[FeatureName] Error:', error);
  message.error(t('[featureName].errorMessage'));
}
```

### 4.4 性能优化
- 使用 `useCallback` 缓存事件处理函数
- 使用 `useMemo` 缓存计算结果
- 使用 `React.memo` 优化子组件渲染
- 添加防抖/节流(如需要)

## 5. 测试策略

### 5.1 单元测试
- 组件测试: 使用 Vitest + React Testing Library
- Hook 测试: 测试自定义 Hook
- 工具函数测试: 测试纯函数

### 5.2 集成测试
- API 调用测试
- 状态管理测试
- 路由导航测试

### 5.3 E2E 测试
- 关键用户流程测试
- 跨浏览器测试

## 6. 代码规范

### 6.1 命名规范
- 组件: PascalCase (如 `ShoppingCart.tsx`)
- Hook: camelCase + `use` 前缀 (如 `useShoppingCart.ts`)
- 工具函数: camelCase (如 `formatPrice.ts`)
- 类型/接口: PascalCase (如 `CartData`, `ICartItem`)
- 常量: PascalCase 或 UPPER_SNAKE_CASE

### 6.2 代码风格
- 遵循 `.prettierrc` 配置
- 遵循 `eslint.config.js` 规则
- 导入顺序: 内置 → 外部 → 内部(@/) → 相对

### 6.3 注释规范
- 使用 JSDoc 注释复杂函数
- 注释解释"为什么"而非"是什么"
- 避免显而易见的注释

## 7. 部署与发布

### 7.1 环境配置
- 开发环境: [配置说明]
- 生产环境: [配置说明]

### 7.2 发布清单
- [ ] 代码审查通过
- [ ] 所有测试通过
- [ ] 文档更新完成
- [ ] 无安全漏洞
- [ ] 性能指标达标

## 8. 参考实现

### 8.1 项目内类似功能
- [功能1]: [代码路径]
- [功能2]: [代码路径]

### 8.2 最佳实践参考
- [参考1]: [链接]
- [参考2]: [链接]

## 9. 待确认事项

- [ ] [待确认事项1]
- [ ] [待确认事项2]

## 10. 设计评审

### 10.1 评审问题
1. [问题1]
2. [问题2]

### 10.2 评审结论
- [ ] 通过
- [ ] 需要修改
- [ ] 重新设计

### 10.3 评审意见
[评审者反馈]
```

---

### 阶段 3: 任务分解 (Task Breakdown)

#### 3.1 将设计分解为可执行任务
基于技术设计,将功能分解为具体的、可执行的任务。

#### 3.2 生成任务清单
创建 `tasks.md`,包含以下内容:

```markdown
# [功能名称] 实现任务清单

## 任务概览

**功能名称**: [功能名称]
**预估总工时**: [X]h
**优先级**: P0/P1/P2
**状态**: 📋 待开始

---

## Phase 1: 准备工作 (预计 [X]h)

### 1.1 环境准备
- [ ] 创建功能分支 `feat/[feature-name]`
- [ ] 安装必要的依赖(如需要)
- [ ] 配置开发环境

**验收标准**: 分支创建成功,环境配置完成

---

### 1.2 创建文件结构
- [ ] 创建目录结构 `src/features/[feature-name]/`
- [ ] 创建子目录: `components/`, `pages/`, `hooks/`, `api/`, `types/`, `utils/`
- [ ] 创建基础文件骨架

**验收标准**: 目录结构完整,符合项目规范

---

## Phase 2: 核心开发 (预计 [X]h)

### 2.1 类型定义
- [ ] 定义数据类型 `src/features/[feature-name]/types/index.ts`
- [ ] 定义 Props 接口
- [ ] 定义枚举类型(如需要)

**验收标准**: 类型定义完整,通过 TypeScript 类型检查

---

### 2.2 API 层
- [ ] 封装 API 调用函数 `src/features/[feature-name]/api/index.ts`
- [ ] 添加类型定义
- [ ] 添加错误处理

**验收标准**: API 函数可正常调用,类型安全

---

### 2.3 状态管理(如需要)
- [ ] 创建 Redux Slice `src/features/[feature-name]/[feature-name]Slice.ts`
- [ ] 定义 state 和 actions
- [ ] 注册到 store `src/store/index.ts`
- [ ] 创建类型安全的 hooks

**验收标准**: 状态管理正常工作,类型安全

---

### 2.4 UI 组件开发

#### 2.4.1 基础组件
- [ ] 组件1: `[ComponentName1]` `src/features/[feature-name]/components/[ComponentName1]/index.tsx`
  - [ ] 定义 Props 接口
  - [ ] 实现组件逻辑
  - [ ] 添加样式
  - [ ] 单元测试

**验收标准**: 组件功能正常,样式符合设计

- [ ] 组件2: `[ComponentName2]` (同上)

#### 2.4.2 页面组件
- [ ] 页面组件: `[PageName]` `src/features/[feature-name]/pages/[PageName]/index.tsx`
  - [ ] 集成子组件
  - [ ] 实现业务逻辑
  - [ ] 添加错误处理
  - [ ] 添加加载状态
  - [ ] 国际化支持

**验收标准**: 页面功能完整,用户体验良好

---

### 2.5 自定义 Hooks(如需要)
- [ ] Hook: `[HookName]` `src/features/[feature-name]/hooks/[HookName].ts`
  - [ ] 定义 Hook 接口
  - [ ] 实现逻辑
  - [ ] 添加清理函数
  - [ ] 单元测试

**验收标准**: Hook 功能正常,无内存泄漏

---

### 2.6 工具函数(如需要)
- [ ] 工具函数: `[UtilityName]` `src/features/[feature-name]/utils/[UtilityName].ts`
  - [ ] 实现逻辑
  - [ ] 添加类型定义
  - [ ] 单元测试

**验收标准**: 工具函数正确,性能良好

---

## Phase 3: 集成与配置 (预计 [X]h)

### 3.1 路由配置
- [ ] 添加路由配置 `src/app/routes/index.tsx`
- [ ] 测试路由导航

**验收标准**: 路由可正常访问,导航正常

---

### 3.2 国际化
- [ ] 添加翻译 keys 到 `src/features/i18n/locales/zh.json`
- [ ] 添加翻译 keys 到 `src/features/i18n/locales/en.json`
- [ ] 在组件中使用 `useTranslation` hook

**验收标准**: 中英文切换正常,所有文本已翻译

---

### 3.3 主题集成(如需要)
- [ ] 集成 Ant Design 主题
- [ ] 测试深色/浅色模式

**验收标准**: 主题切换正常,样式一致

---

## Phase 4: 测试与优化 (预计 [X]h)

### 4.1 单元测试
- [ ] 编写组件单元测试
- [ ] 编写 Hook 单元测试
- [ ] 编写工具函数单元测试

**验收标准**: 测试覆盖率 > 80%,所有测试通过

---

### 4.2 集成测试
- [ ] 测试组件集成
- [ ] 测试 API 调用
- [ ] 测试状态管理

**验收标准**: 集成测试通过,无错误

---

### 4.3 手动测试
- [ ] 测试功能流程
- [ ] 测试边界情况
- [ ] 测试错误处理
- [ ] 测试不同浏览器

**验收标准**: 所有测试用例通过

---

### 4.4 性能优化
- [ ] 使用 React DevTools 分析性能
- [ ] 优化不必要的渲染
- [ ] 添加防抖/节流(如需要)
- [ ] 优化大数据量渲染(如需要)

**验收标准**: 性能指标达标,用户体验流畅

---

## Phase 5: 代码质量 (预计 [X]h)

### 5.1 代码审查
- [ ] 自我代码审查
- [ ] 修复 ESLint 警告
- [ ] 修复 Prettier 格式问题
- [ ] 修复 TypeScript 类型错误

**验收标准**: 无 lint 错误,类型检查通过

---

### 5.2 代码格式化
- [ ] 运行 `bun run format`
- [ ] 运行 `bun run lint:fix`

**验收标准**: 代码格式符合规范

---

### 5.3 文档更新
- [ ] 更新组件注释
- [ ] 更新 README(如需要)
- [ ] 更新 API 文档(如需要)

**验收标准**: 文档完整准确

---

## Phase 6: 部署准备 (预计 [X]h)

### 6.1 构建测试
- [ ] 运行 `bun run build`
- [ ] 检查构建产物

**验收标准**: 构建成功,无错误

---

### 6.2 代码提交
- [ ] 使用 Conventional Commits 格式编写提交信息
- [ ] 提交代码
- [ ] 推送到远程仓库

**验收标准**: 提交信息规范,代码已推送

---

### 6.3 创建 Pull Request
- [ ] 创建 PR
- [ ] 填写 PR 描述
- [ ] 添加合适的 labels
- [ ] 关联相关 Issue

**验收标准**: PR 创建成功,信息完整

---

## 任务依赖关系

```
Phase 1 (准备工作)
    ↓
Phase 2 (核心开发)
    ├── 2.1 类型定义
    ├── 2.2 API 层
    ├── 2.3 状态管理 (可选)
    ├── 2.4 UI 组件开发
    │   ├── 2.4.1 基础组件
    │   └── 2.4.2 页面组件
    ├── 2.5 自定义 Hooks (可选)
    └── 2.6 工具函数 (可选)
    ↓
Phase 3 (集成与配置)
    ├── 3.1 路由配置
    ├── 3.2 国际化
    └── 3.3 主题集成 (可选)
    ↓
Phase 4 (测试与优化)
    ├── 4.1 单元测试
    ├── 4.2 集成测试
    ├── 4.3 手动测试
    └── 4.4 性能优化
    ↓
Phase 5 (代码质量)
    ├── 5.1 代码审查
    ├── 5.2 代码格式化
    └── 5.3 文档更新
    ↓
Phase 6 (部署准备)
    ├── 6.1 构建测试
    ├── 6.2 代码提交
    └── 6.3 创建 Pull Request
```

---

## 风险与缓解措施

| 风险 | 影响 | 缓解措施 |
|------|------|---------|
| [风险1] | 高/中/低 | [缓解措施] |
| [风险2] | 高/中/低 | [缓解措施] |

---

## 进度跟踪

**开始日期**: [YYYY-MM-DD]
**预计完成日期**: [YYYY-MM-DD]
**实际完成日期**: [YYYY-MM-DD]

**当前进度**: [X]%

| Phase | 状态 | 完成度 | 耗时 |
|-------|------|--------|------|
| Phase 1: 准备工作 | ⬜/🟡/🟢 | [X]% | [X]h |
| Phase 2: 核心开发 | ⬜/🟡/🟢 | [X]% | [X]h |
| Phase 3: 集成与配置 | ⬜/🟡/🟢 | [X]% | [X]h |
| Phase 4: 测试与优化 | ⬜/🟡/🟢 | [X]% | [X]h |
| Phase 5: 代码质量 | ⬜/🟡/🟢 | [X]% | [X]h |
| Phase 6: 部署准备 | ⬜/🟡/🟢 | [X]% | [X]h |

图例:
- ⬜ 未开始
- 🟡 进行中
- 🟢 已完成

---

## 备注

[其他需要说明的事项]
```

---

## 执行流程

### 触发条件
当用户提出以下类型的请求时,自动触发 Spec Mode:

1. **新功能开发**
   - "我想要实现一个[功能]"
   - "添加一个[组件/页面]"
   - "开发[某功能]"

2. **Bug 修复**
   - "修复[某问题]"
   - "[某功能]有问题"
   - "[某功能]不工作"

3. **功能优化**
   - "优化[某功能]"
   - "改进[某功能]的性能"

### 执行步骤

#### Step 1: 触发检测
识别用户请求类型,确定是否需要进入 Spec Mode。

#### Step 2: 需求分析
1. 询问用户明确需求(如果不够清晰)
2. 分析现有代码实现
3. 生成 `requirements.md`

#### Step 3: 技术设计
1. 基于需求设计技术方案
2. 生成 `design.md`

#### Step 4: 任务分解
1. 将设计分解为具体任务
2. 生成 `tasks.md`

#### Step 5: 用户确认
1. 向用户展示生成的规范文档
2. 等待用户确认或修改意见

#### Step 6: 记录用户反馈
1. 记录用户的纠正和修改意见
2. 更新规范文档
3. 将反馈用于优化 Skill

#### Step 7: 执行开发(可选)
如果用户确认后要求开始开发:
1. 按照 `tasks.md` 执行任务
2. 逐步完成每个 phase
3. 更新任务状态

---

## 学习与优化机制

### 反馈收集
在执行过程中,收集以下反馈:

1. **用户纠正**
   - 需求理解错误
   - 技术方案不合理
   - 任务分解不准确

2. **项目规范学习**
   - 代码风格习惯
   - 组件命名规范
   - API 设计模式

3. **最佳实践发现**
   - 项目特有的实现方式
   - 常用的工具函数
   - 常见的设计模式

### Skill 更新
根据收集的反馈,定期更新 Skill:

1. **更新需求模板**
   - 添加常见的字段
   - 优化问题引导
   - 增强验收标准

2. **更新设计模板**
   - 添加常见的设计模式
   - 优化架构描述
   - 补充最佳实践

3. **更新任务模板**
   - 添加常见的开发任务
   - 优化任务分解
   - 增强检查清单

4. **更新项目知识库**
   - 记录项目特有的规范
   - 记录常用的组件和工具
   - 记录常见问题和解决方案

### 知识沉淀
将学习到的知识沉淀到以下位置:

1. **SKILL.md** - Skill 本身
2. **references/** - 参考文档
3. **AGENTS.md** - 项目上下文(如需要)

---

## 项目规范学习

### 代码风格规范
每次执行前,阅读并应用以下规范:

1. **TypeScript 配置** (`tsconfig.json`)
2. **ESLint 配置** (`eslint.config.js`)
3. **Prettier 配置** (`.prettierrc`)
4. **导入顺序规范** (见 `AGENTS.md`)

### 组件开发规范
1. 组件命名: PascalCase
2. 文件结构: 单一职责
3. Props 定义: 必须使用 interface
4. 样式方案: Ant Design + CSS-in-JS

### API 调用规范
1. 使用 `safeInvoke` 封装
2. 统一错误处理
3. 类型安全调用

### 状态管理规范
1. 本地状态: `useState`
2. 全局状态: Redux Toolkit
3. 类型安全的 hooks

### 国际化规范
1. 所有用户可见文本使用 `t()`
2. 翻译 key 命名规范
3. 中英文双语支持

---

## 示例:购物车功能

假设用户要求开发一个购物车功能,执行流程如下:

### 1. 需求分析
生成 `.iflow/specs/shopping-cart/requirements.md`

### 2. 技术设计
生成 `.iflow/specs/shopping-cart/design.md`

### 3. 任务分解
生成 `.iflow/specs/shopping-cart/tasks.md`

### 4. 用户确认
向用户展示文档,等待确认

### 5. 执行开发(可选)
如果用户确认,按照任务清单执行

---

## 注意事项

1. **不要跳过步骤**: 必须按照工作流程执行,不能直接开始编码
2. **用户确认优先**: 在开始编码前,必须获得用户对规范文档的确认
3. **记录一切**: 记录所有用户反馈和纠正,用于优化 Skill
4. **保持更新**: 定期根据项目变化更新 Skill
5. **灵活调整**: 根据任务复杂度调整文档详细程度

---

## 快速参考

### 文件路径规范
- 需求文档: `.iflow/specs/[feature-name]/requirements.md`
- 设计文档: `.iflow/specs/[feature-name]/design.md`
- 任务清单: `.iflow/specs/[feature-name]/tasks.md`

### 命名规范
- feature-name: kebab-case (如 `shopping-cart`)
- ComponentName: PascalCase (如 `ShoppingCart`)
- functionName: camelCase (如 `calculateTotal`)

### 状态标记
- 📋 待开始
- 🟡 进行中
- 🟢 已完成
- ⚠️ 有问题

---

## 参考资源

- **项目文档**: `AGENTS.md`
- **代码规范**: `.prettierrc`, `eslint.config.js`
- **技术栈文档**: React, Ant Design, Redux Toolkit
- **设计规范**: Ant Design 设计语言