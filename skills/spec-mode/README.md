# Spec Mode Skill 使用指南

## 概述

Spec Mode 是一个规范优先的开发工作流 skill,用于在新功能开发或 Bug 修复前进行系统化的规划和设计。

## 触发条件

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

## 工作流程

```
用户提出需求
    ↓
触发 Spec Mode
    ↓
阶段1: 需求分析
    ├── 理解用户需求
    ├── 分析现有实现
    └── 生成 requirements.md
    ↓
阶段2: 技术设计
    ├── 架构设计
    ├── 组件设计
    └── 生成 design.md
    ↓
阶段3: 任务分解
    ├── 分解开发任务
    ├── 制定检查清单
    └── 生成 tasks.md
    ↓
用户确认
    ├── 展示规范文档
    ├── 收集反馈
    └── 记录纠正意见
    ↓
执行开发(可选)
    ├── 按任务清单执行
    ├── 更新进度
    └── 完成功能
```

## 生成的文件结构

执行 Spec Mode 后,会在 `.iflow/specs/<feature-name>/` 目录下生成以下文件:

```
.iflow/specs/
└── shopping-cart/        # 功能名称(kebab-case)
    ├── requirements.md   # 需求定义文档
    ├── design.md         # 技术设计文档
    └── tasks.md          # 实现任务清单
```

### 文件说明

#### requirements.md
包含:
- 功能概述(背景、目标、范围)
- 用户故事和验收标准
- 功能需求列表
- UI/UX 需求
- 数据需求和 API 接口
- 技术约束
- 依赖与排期
- 风险评估

#### design.md
包含:
- 总体设计(架构、技术选型、文件结构)
- 详细设计(组件、数据流、API、路由)
- 类型定义
- 实现细节
- 测试策略
- 代码规范
- 参考实现

#### tasks.md
包含:
- 分阶段任务清单
- 每个任务的验收标准
- 任务依赖关系
- 进度跟踪
- 风险与缓解措施

## 使用示例

### 示例 1: 开发新功能

**用户请求**:
```
我想要实现一个购物车功能,用户可以将版本添加到购物车,然后批量安装。
```

**Spec Mode 执行**:
1. 识别为新功能开发请求
2. 生成 `.iflow/specs/shopping-cart/requirements.md`
3. 生成 `.iflow/specs/shopping-cart/design.md`
4. 生成 `.iflow/specs/shopping-cart/tasks.md`
5. 向用户展示文档,等待确认

### 示例 2: 修复 Bug

**用户请求**:
```
修复下载进度不更新的问题
```

**Spec Mode 执行**:
1. 识别为 Bug 修复请求
2. 分析现有代码,找出问题原因
3. 生成 `.iflow/specs/fix-download-progress/requirements.md`
4. 生成 `.iflow/specs/fix-download-progress/design.md`
5. 生成 `.iflow/specs/fix-download-progress/tasks.md`
6. 向用户展示修复方案,等待确认

## 学习与优化

### 反馈收集

在执行过程中,Skill 会自动收集以下反馈:

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

1. 更新需求模板
2. 更新设计模板
3. 更新任务模板
4. 更新项目知识库

### 知识沉淀

将学习到的知识沉淀到:
- **SKILL.md** - Skill 本身
- **examples/** - 示例文档
- **AGENTS.md** - 项目上下文

## 项目规范学习

每次执行前,Skill 会自动学习以下规范:

### 代码风格规范
- TypeScript 配置 (`tsconfig.json`)
- ESLint 配置 (`eslint.config.js`)
- Prettier 配置 (`.prettierrc`)
- 导入顺序规范

### 组件开发规范
- 组件命名: PascalCase
- 文件结构: 单一职责
- Props 定义: 必须使用 interface
- 样式方案: Ant Design

### API 调用规范
- 使用 `safeInvoke` 封装
- 统一错误处理
- 类型安全调用

### 状态管理规范
- 本地状态: `useState`
- 全局状态: Redux Toolkit
- 类型安全的 hooks

### 国际化规范
- 所有用户可见文本使用 `t()`
- 翻译 key 命名规范
- 中英文双语支持

## 注意事项

1. **不要跳过步骤**: 必须按照工作流程执行,不能直接开始编码
2. **用户确认优先**: 在开始编码前,必须获得用户对规范文档的确认
3. **记录一切**: 记录所有用户反馈和纠正,用于优化 Skill
4. **保持更新**: 定期根据项目变化更新 Skill
5. **灵活调整**: 根据任务复杂度调整文档详细程度

## 示例文档

查看 `.iflow/specs/shopping-cart/` 目录下的示例文档,了解规范文档的实际格式和内容。

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

## 常见问题

### Q: 是否所有任务都需要生成规范文档?
A: 不是。对于简单的、快速的任务(如修复一个明显的 typo),可以跳过规范文档直接修复。Spec Mode 主要用于:
- 新功能开发
- 复杂的 Bug 修复
- 重大功能重构
- 涉及多个模块的修改

### Q: 规范文档生成后可以修改吗?
A: 可以。在用户确认阶段,可以根据用户反馈修改规范文档。用户确认后,文档作为开发的依据。

### Q: 如果用户确认后需求变了怎么办?
A: 需要重新生成或更新规范文档,再次获得用户确认后再继续开发。

### Q: Spec Mode 会影响开发效率吗?
A: 短期来看,会增加规划时间。但从长期来看,可以:
- 减少返工
- 提高代码质量
- 便于团队协作
- 便于后续维护

## 技术支持

如有问题或建议,请:
1. 查看示例文档
2. 参考 `AGENTS.md` 项目文档
3. 更新 Skill 配置

## 版本历史

- **v1.0.0** (2026-03-02)
  - 初始版本
  - 实现基础工作流程
  - 添加示例文档