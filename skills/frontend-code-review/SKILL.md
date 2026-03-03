---
name: frontend-code-review
description: Frontend code review skill for LVM project - TypeScript/React code review using git diff analysis. Checks code quality, type safety, performance optimization, and best practices compliance. Use when: (1) Pre-commit self-review, (2) Pull Request code review, (3) Post-development quality check, (4) Post-refactoring validation
---

# Frontend Code Review

## Overview

This skill provides a systematic frontend code review workflow optimized for the LVM project's tech stack (Tauri + React + TypeScript + Ant Design). By analyzing git diff changes, it identifies potential issues, violations of standards, and improvement opportunities.

**Key Features:**
- Automated code quality checks (ESLint, Prettier, TypeScript)
- Manual review checklist covering all aspects of React/TypeScript development
- Auto-generates commit message following Conventional Commits
- Auto-generates PR description based on `.github/PULL_REQUEST_TEMPLATE.md`
- Compatible with GitHub workflow labels (bug, enhancement, rust, frontend, pr)

## Review Workflow

### Step 1: Get Code Changes

Use git commands to get code changes to review:

```bash
# Get all unstaged changes
git diff HEAD

# Get staged changes
git diff --staged

# Get changes for specific file
git diff HEAD -- src/features/version-manager/pages/PythonManagePage/index.tsx

# Get changes for last N commits
git diff HEAD~N HEAD

# Get branch comparison
git diff main...HEAD
```

**Best Practice:** Prioritize reviewing staged changes (`--staged`) as these are about to be committed.

### Step 2: Identify Change Types

Analyze git diff output to identify change types:

- **New files** (`new file mode`)
- **Deleted files** (`deleted file mode`)
- **Modified files** (existing file changes)
- **Renamed/moved** (`rename from/to`)

**Focus on:** Modified `.tsx`, `.ts`, `.jsx`, `.js` files.

### Step 3: Code Quality Checks

For each changed file, perform the following checks:

#### 3.1 Automated Checks

Run project's automated check tools:

```bash
# ESLint check
bun run lint

# Prettier format check
bun run format

# TypeScript type check
bun run build  # or tsc --noEmit
```

**If automated checks fail, prioritize fixing these issues.**

#### 3.2 Manual Review Checklist

For each change, check the following aspects:

**Code Style:**
- [ ] Complies with Prettier config (100 char width, 2 space indent, single quotes)
- [ ] Complies with ESLint rules (no unused imports, correct import order)
- [ ] File naming conventions (PascalCase components, camelCase utilities)

**TypeScript Types:**
- [ ] All components have Props interface definition
- [ ] Generic types explicitly specified (e.g., `safeInvoke<VersionResult>`)
- [ ] Avoid using `any` type
- [ ] Use `interface` for object structures, `type` for union types

**React Best Practices:**
- [ ] Hooks called at top level, not in conditions/loops
- [ ] Dependency arrays complete (all external dependencies included)
- [ ] Use `useCallback` to cache functions passed to child components
- [ ] Use `useMemo` to cache expensive computations
- [ ] Event handlers wrapped with `useCallback`

**Tauri API:**
- [ ] Use `safeInvoke` instead of direct `invoke`
- [ ] Event listeners properly cleaned up (return cleanup function)
- [ ] Error handling complete (try-catch + user feedback)

**Performance Optimization:**
- [ ] Avoid creating new objects/functions in render
- [ ] Large lists use virtual scrolling
- [ ] Debounce/throttle high-frequency events
- [ ] Use `React.memo` to avoid unnecessary re-renders

**State Management:**
- [ ] Local state uses `useState`
- [ ] Cross-component state uses Redux
- [ ] Correct use of typed hooks (`useAppDispatch`, `useAppSelector`)

**Internationalization:**
- [ ] User-visible text uses `t()` function
- [ ] Translation keys defined in language files

### Step 4: Generate Review Report

Generate a structured report based on check results:

```markdown
## Code Review Report

### Overall Assessment
- **Quality Level**: Excellent / Good / Needs Improvement
- **Key Strengths**: [List 2-3 strengths]
- **Key Issues**: [List 2-3 main issues]

### Issue List

#### Critical Issues (Must Fix)
1. **[Issue Title]**
   - Location: `file_path:line_number`
   - Description: [Issue description]
   - Impact: [Impact scope]
   - Fix Suggestion: [Code example]

#### Warnings (Recommended Fix)
1. **[Issue Title]**
   - Location: `file_path:line_number`
   - Description: [Issue description]
   - Fix Suggestion: [Code example]

#### Suggestions (Optional Optimization)
1. **[Optimization Suggestion]**
   - Location: `file_path:line_number`
   - Description: [Optimization explanation]
   - Expected Benefit: [Performance/maintainability improvement]

### Best Practice Recommendations
- [Recommendation 1]
- [Recommendation 2]

### Summary
- Fix Priority: [List priority order]
- Estimated Fix Time: [Estimate]
```

### Step 5: Determine Approval Status

Based on the review results, determine if code can be committed:

**Approval Criteria:**
- ✅ No Critical Issues
- ✅ Automated checks pass (ESLint, Prettier, TypeScript)
- ✅ Code follows project standards
- ✅ No security vulnerabilities

**If Approved:**
- Proceed to Step 6 to generate commit message and PR description

**If Not Approved:**
- List required fixes
- Re-review after fixes are applied

### Step 6: Generate Commit Message

If code is approved, generate a commit message following Conventional Commits format:

```bash
# Format
<type>(<scope>): <subject>

<body>

<footer>
```

**Type Options:**
- `feat`: 新功能
- `fix`: Bug 修复
- `refactor`: 重构(不改变功能)
- `perf`: 性能优化
- `style`: 代码风格改进
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建/CI 相关
- `build`: 构建系统或外部依赖变更

**Scope Options:**
- `frontend`: 前端代码
- `rust`: 后端 Rust 代码
- `i18n`: 国际化
- `types`: 类型定义
- `api`: API 层
- `ui`: UI 组件

**Example:**
```bash
feat(frontend): implement i18n support for settings page

- Add translation keys for settings page in en.json and zh.json
- Replace hardcoded strings with t() function calls
- Update Settings component to use useTranslation hook
- Format language pack files for better readability

Closes #123
```

### Step 7: Generate PR Description

If creating a Pull Request, generate description based on `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## 📌 变更类型

- [ ] 🐞 Bug 修复
- [ ] ✨ 新功能
- [ ] ♻️ 重构
- [ ] 📚 文档更新
- [ ] 🎨 代码风格改进
- [ ] ⚡ 性能优化
- [ ] 🛡️ 安全改进
- [ ] 🔧 构建/CI 相关
- [ ] 📦 依赖更新
- [ ] 🔧 其他

---

## 🧾 变更说明

[简要说明本次 PR 做了什么]

### 主要变更

- [变更点 1]
- [变更点 2]
- [变更点 3]

### 技术细节

[可选: 详细说明技术实现]

---

## 🔍 相关 Issue

Closes #[issue_number]

---

## 🧪 测试情况

- [x] 本地运行通过
- [x] cargo check 通过
- [x] cargo fmt 已执行
- [x] 前端 lint 通过

### 测试步骤

1. [测试步骤 1]
2. [测试步骤 2]

---

## 📷 截图（如有）

[如果有 UI 变更，添加截图]

---

## ⚠️ 注意事项

- [ ] 是否有破坏性更改？
- [ ] 是否需要迁移指南？
- [ ] 是否影响现有功能？

---

## 🏷️ Labels

根据 `.github/workflows/auto-label.yml`，PR 会自动添加以下标签：
- `pr` - 所有 PR 默认添加
- `frontend` - 如果修改了 `src/**` 目录
- `rust` - 如果修改了 `src-tauri/**` 目录
- `bug` - 如果标题包含 `[Bug]`
- `enhancement` - 如果标题包含 `[Feature]`
```

## Auto-Generation Guidelines

### Commit Message Generation Rules

1. **Analyze changes to determine type:**
   - Adding new feature → `feat`
   - Fixing bug → `fix`
   - Refactoring code → `refactor`
   - Performance improvement → `perf`
   - Style/format changes → `style`
   - Documentation → `docs`
   - Testing → `test`
   - Build/CI changes → `build` or `chore`

2. **Determine scope:**
   - Changes in `src/**` → `frontend`
   - Changes in `src-tauri/**` → `rust`
   - Both → `all` or omit scope
   - Specific module (e.g., `i18n`, `types`, `api`)

3. **Write subject:**
   - Use imperative mood ("add" not "added")
   - Don't capitalize first letter
   - No period (.) at the end
   - Limit to 50 characters

4. **Write body (optional but recommended):**
   - Explain "what" and "why"
   - Use bullet points for multiple changes
   - Wrap at 72 characters

5. **Add footer (optional):**
   - Reference issues: `Closes #123`
   - Breaking changes: `BREAKING CHANGE:`

### PR Description Generation Rules

1. **Select change type:**
   - Based on commit message type
   - Check corresponding checkbox in template

2. **Write change description:**
   - Brief summary of changes
   - List main changes
   - Add technical details if complex

3. **Fill testing section:**
   - Mark completed checks
   - Add test steps if applicable
   - Include screenshots for UI changes

4. **Add notes:**
   - Highlight breaking changes
   - Note migration requirements
   - List any known limitations

## Common Issue Patterns

### Unused Imports

**Detection:** ESLint automatic detection

**Fix Example:**
```typescript
// Bad
import { useState, useEffect } from 'react';
import { Button } from 'antd';

function MyComponent() {
  const [state, setState] = useState('');
  return <Button>Click</Button>;
}

// Good
import { useState } from 'react';
import { Button } from 'antd';

function MyComponent() {
  const [state, setState] = useState('');
  return <Button>Click</Button>;
}
```

### Missing Type Definitions

**Detection:** TypeScript compilation check

**Fix Example:**
```typescript
// Bad
function MyComponent({ title, onAction }) {
  return <div>{title}</div>;
}

// Good
interface MyComponentProps {
  title: string;
  onAction?: () => void;
}

function MyComponent({ title, onAction }: MyComponentProps) {
  return <div>{title}</div>;
}
```

### Incomplete Dependency Array

**Detection:** ESLint react-hooks rules

**Fix Example:**
```typescript
// Bad
useEffect(() => {
  fetchData(userId);
}, []);  // Missing userId dependency

// Good
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

### Uncleaned Event Listeners

**Detection:** Code review

**Fix Example:**
```typescript
// Bad
useEffect(() => {
  const unlisten = listen('event', handler);
  // Missing cleanup
}, []);

// Good
useEffect(() => {
  const unlisten = listen('event', handler);
  return () => {
    unlisten.then(f => f());
  };
}, []);
```

### Creating New Functions in Render

**Detection:** Performance analysis

**Fix Example:**
```typescript
// Bad
<List
  dataSource={data}
  renderItem={item => (
    <Item onClick={() => handleAction(item.id)}>{item.name}</Item>
  )}
/>

// Good
const renderItem = useCallback((item: DataType) => (
  <Item onClick={() => handleAction(item.id)}>{item.name}</Item>
), [handleAction]);

<List dataSource={data} renderItem={renderItem} />
```

## Project-Specific Standards

### Import Order

Strictly follow this import order:

```typescript
// 1. Node.js built-in modules
import { useState, useEffect } from 'react';

// 2. External dependencies
import { Button, Table } from 'antd';
import { invoke } from '@tauri-apps/api/core';

// 3. Internal modules (using @/ alias)
import { LangEnum } from '@/core/constants/enum';
import { store } from '@/store';

// 4. Relative imports
import { SomeComponent } from './components';
```

### Tauri API Calls

Always use `safeInvoke` wrapper:

```typescript
// Good
import { safeInvoke } from '@/api/tauri';
import { CommandEnum } from '@/core/constants/enum';

const result = await safeInvoke<VersionResult>(CommandEnum.LIST_VERSIONS, {
  language: LanguageEnum.PYTHON,
  page: 0,
  pageSize: 10,
  keyWord: ''
});
```

### Component Naming

- Component files: PascalCase (`VersionTable.tsx`)
- Utility files: camelCase (`store.ts`)
- Hook files: camelCase with `use` prefix (`useDownload.ts`)

## References

For detailed code standards and best practices, refer to:

- **[LVM Project Standards](references/lvm-standards.md)** - Complete project code standards
- **[React Best Practices](references/react-best-practices.md)** - React development best practices
- **[AGENTS.md](../../../AGENTS.md)** - Project context documentation
- **[.github/PULL_REQUEST_TEMPLATE.md](../../../.github/PULL_REQUEST_TEMPLATE.md)** - PR template

## Quick Reference

### Key File Locations

- **ESLint Config**: `eslint.config.js`
- **Prettier Config**: `.prettierrc`
- **TypeScript Config**: `tsconfig.json`
- **Project Standards**: `AGENTS.md`
- **PR Template**: `.github/PULL_REQUEST_TEMPLATE.md`
- **Issue Templates**: `.github/ISSUE_TEMPLATE/`
- **Workflow Config**: `.github/workflows/auto-label.yml`

### Common Commands

```bash
# Run all checks
bun run lint && bun run format && bun run build

# Check only, no fix
bun run lint

# Auto fix
bun run lint:fix

# Format code
bun run format
```

### Review Priority

1. **Critical Issues** - Block merge, must fix immediately
2. **Warnings** - Recommended fix, improve code quality
3. **Suggestions** - Optional optimization, long-term improvement

## Notes

- Review should consider the context of changes and business requirements
- Prioritize functional issues and potential bugs
- For style issues, automated tools can be used for batch fixes
- Provide constructive feedback and specific fix suggestions
- Respect developer's design decisions unless there are clear issues
- When approved, always generate commit message and PR description following project standards
- Ensure generated content matches GitHub workflow expectations for automatic labeling