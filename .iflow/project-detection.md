# 项目规范检测逻辑

## 概述

本文档描述了如何检测项目环境并动态加载项目特定的代码规范。这是实现通用技能库的关键机制，确保技能能够在不同项目中正确工作。

## 检测流程

### 1. 环境检测

#### 检测是否为项目目录

**检测方法**：
```bash
# 检查是否存在常见的项目配置文件
[ -f "package.json" ] && echo "Node.js 项目"
[ -f "requirements.txt" ] && echo "Python 项目"
[ -f "Cargo.toml" ] && echo "Rust 项目"
[ -f "go.mod" ] && echo "Go 项目"
[ -f "pom.xml" ] && echo "Java/Maven 项目"
[ -f "build.gradle" ] && echo "Java/Gradle 项目"
```

**判断逻辑**：
- 如果存在任何项目配置文件 → 项目环境
- 如果不存在任何项目配置文件 → 全局环境

### 2. 配置文件读取

#### 前端项目配置文件

**package.json**
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "scripts": {
    "lint": "eslint .",
    "format": "prettier --write .",
    "build": "tsc"
  },
  "dependencies": {
    "react": "^18.0.0",
    "typescript": "^5.0.0"
  },
  "devDependencies": {
    "eslint": "^8.0.0",
    "prettier": "^3.0.0"
  }
}
```

**tsconfig.json**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "strict": true,
    "jsx": "react-jsx"
  }
}
```

**eslint.config.js**
```javascript
export default [
  {
    rules: {
      "no-unused-imports": "error",
      "import/order": "warn"
    }
  }
];
```

**.prettierrc**
```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "singleQuote": true,
  "semi": true
}
```

### 3. 规范文档查找

#### 规范文档优先级

1. **项目根目录**
   - `AGENTS.md` - 项目上下文文档
   - `CONTRIBUTING.md` - 贡献指南
   - `CODE_OF_CONDUCT.md` - 行为准则

2. **docs/ 目录**
   - `docs/standards.md`
   - `docs/conventions.md`
   - `docs/coding-style.md`

3. **.github/ 目录**
   - `.github/CONTRIBUTING.md`
   - `.github/PULL_REQUEST_TEMPLATE.md`
   - `.github/ISSUE_TEMPLATE/`

4. **技能库参考**
   - `skills/*/references/*.md`

### 4. 代码风格分析

#### 分析现有代码

**导入顺序分析**
```typescript
// 检查现有文件的导入顺序
// 识别项目使用的导入模式
import { useState } from 'react';
import { Button } from 'antd';
import { store } from '@/store';
import { util } from './utils';
```

**组件命名分析**
```typescript
// 检查组件文件命名
UserList.tsx      // PascalCase
formatDate.ts     // camelCase
useAuth.ts        // camelCase + use 前缀
API_BASE_URL.ts   // UPPER_SNAKE_CASE
```

**状态管理方案**
```typescript
// 检测使用的状态管理方案
Redux Toolkit / Zustand / Context API / Jotai / Recoil
```

### 5. 规范合并策略

#### 优先级规则

1. **项目特定规范**（最高优先级）
   - 项目配置文件中的设置
   - 项目规范文档中的规定

2. **技能库参考**（中等优先级）
   - 技能自带的参考文档
   - 通用的最佳实践

3. **通用标准**（最低优先级）
   - 语言/框架官方推荐
   - 行业通用标准

#### 冲突解决

当不同来源的规范存在冲突时：
- 项目规范 > 技能库规范 > 通用标准
- 明确标注冲突点
- 提供建议的解决方案

## 实现示例

### 检测逻辑伪代码

```typescript
async function detectProjectStandards() {
  // 1. 检测环境
  const isProject = await checkIsProjectDirectory();

  if (!isProject) {
    return getGlobalStandards();
  }

  // 2. 读取配置文件
  const configs = await readConfigFiles([
    'package.json',
    'tsconfig.json',
    'eslint.config.js',
    '.prettierrc'
  ]);

  // 3. 查找规范文档
  const specDocs = await findSpecDocuments([
    'AGENTS.md',
    'CONTRIBUTING.md',
    'docs/standards.md'
  ]);

  // 4. 分析代码风格
  const codeStyle = await analyzeCodeStyle();

  // 5. 合并规范
  return {
    project: true,
    configs,
    specDocs,
    codeStyle,
    standards: mergeStandards(configs, specDocs, codeStyle)
  };
}

function mergeStandards(configs, specDocs, codeStyle) {
  return {
    codeStyle: {
      ...getGlobalCodeStyle(),
      ...configs.prettier,
      ...codeStyle.detected
    },
    linting: {
      ...getGlobalLinting(),
      ...configs.eslint
    },
    types: {
      ...getGlobalTypes(),
      ...configs.typescript
    },
    conventions: {
      ...getGlobalConventions(),
      ...specDocs.conventions
    }
  };
}
```

## 常见项目类型

### React 项目

**特征文件**：
- `package.json`（包含 react、react-dom）
- `tsconfig.json` 或 `jsconfig.json`
- `.eslintrc` 或 `eslint.config.js`
- `.prettierrc`

**规范要点**：
- 组件命名：PascalCase
- Hooks 命名：camelCase + use 前缀
- 文件结构：components/, pages/, hooks/, utils/

### Vue 项目

**特征文件**：
- `package.json`（包含 vue）
- `vite.config.ts` 或 `vue.config.js`
- `.eslintrc`
- `.prettierrc`

**规范要点**：
- 组件命名：PascalCase 或 kebab-case
- Composition API 优先
- 文件结构：components/, views/, composables/

### Node.js 后端项目

**特征文件**：
- `package.json`（不包含前端框架）
- `tsconfig.json`
- `src/` 目录
- 可能包含 `nest-cli.json`、`ts-node.json` 等

**规范要点**：
- 模块命名：camelCase
- 类命名：PascalCase
- 常量命名：UPPER_SNAKE_CASE

### Python 项目

**特征文件**：
- `requirements.txt` 或 `pyproject.toml`
- `setup.py`
- `.flake8`, `.pylint`, `black` 配置

**规范要点**：
- 模块命名：lowercase_with_underscores
- 类命名：PascalCase
- 常量命名：UPPER_CASE_WITH_UNDERSCORES

## 缓存机制

为了提高性能，建议实现规范检测结果的缓存：

```typescript
const specCache = new Map<string, ProjectSpecs>();

async function getProjectSpecs(projectPath: string) {
  // 检查缓存
  if (specCache.has(projectPath)) {
    return specCache.get(projectPath);
  }

  // 执行检测
  const specs = await detectProjectStandards(projectPath);

  // 存入缓存
  specCache.set(projectPath, specs);

  return specs;
}

// 清除缓存
function clearSpecCache(projectPath?: string) {
  if (projectPath) {
    specCache.delete(projectPath);
  } else {
    specCache.clear();
  }
}
```

## 错误处理

### 配置文件不存在

```typescript
async function readConfigFile(filename: string) {
  try {
    return await readFile(filename);
  } catch (error) {
    if (error.code === 'ENOENT') {
      // 文件不存在，使用默认值
      return getDefaultConfig(filename);
    }
    throw error;
  }
}
```

### 配置文件解析错误

```typescript
async function parseConfigFile(filename: string) {
  try {
    const content = await readFile(filename);
    return JSON.parse(content);
  } catch (error) {
    if (error instanceof SyntaxError) {
      // 解析错误，使用默认值并记录警告
      console.warn(`配置文件 ${filename} 解析失败，使用默认值`);
      return getDefaultConfig(filename);
    }
    throw error;
  }
}
```

## 最佳实践

1. **渐进式检测**：先检测最基本的配置，逐步扩展到更详细的规范
2. **容错机制**：配置文件不存在或解析失败时，提供合理的默认值
3. **性能优化**：使用缓存机制避免重复检测
4. **可扩展性**：支持添加新的项目类型和配置文件类型
5. **明确反馈**：向用户明确说明使用了哪些规范配置

## 总结

项目规范检测是通用技能库的核心能力，通过动态检测和加载项目规范，确保技能能够在不同项目中正确工作。关键点包括：

- 环境检测：判断是项目环境还是全局环境
- 配置读取：读取项目的配置文件和规范文档
- 代码分析：分析现有代码的风格和模式
- 规范合并：按照优先级合并不同来源的规范
- 缓存优化：提高检测性能
- 错误处理：确保健壮性和容错性