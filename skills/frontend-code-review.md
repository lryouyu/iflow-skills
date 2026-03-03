---
name: frontend-code-review
version: 1.0.0
description: LVM 项目前端代码审查技能,基于项目特定的代码规范和架构模式进行全面的代码审查
---

# LVM 前端代码审查 Skill

## 项目上下文

LVM 是一个基于 Tauri + React + TypeScript 的跨平台语言版本管理器,使用 Ant Design 作为 UI 组件库,Redux Toolkit 进行状态管理。

## 审查范围

此技能专门审查 LVM 项目的前端代码,包括但不限于:
- React 组件 (`.tsx`, `.jsx`)
- TypeScript/JavaScript 文件 (`.ts`, `.js`)
- 自定义 Hooks (`src/hooks/`)
- Redux Slices (`features/**/*Slice.ts`)
- 路由配置 (`src/app/routes/`)
- API 调用层 (`src/api/`)

## 审查标准

### 1. 代码风格与格式

#### Prettier 规则
- 打印宽度: 100 字符
- 缩进: 2 空格
- 引号: 单引号 (JSX 属性使用双引号)
- 分号: 必须使用
- 尾随逗号: ES5 风格 (始终添加)
- 箭头函数参数: 单参数时省略括号
- 行尾: CRLF (Windows)

#### ESLint 规则
- ❌ 禁止未使用的导入 (`unused-imports/no-unused-imports: error`)
- ⚠️ 禁止未使用的变量 (警告级别,以下划线开头的变量除外)
- ✅ 强制导入顺序: builtin → external → internal → parent → sibling → index
- ✅ React JSX 布尔值: 省略值 (如 `<Button disabled />` 而非 `<Button disabled={true} />`)
- ✅ 自闭合标签: 尽可能使用自闭合 (如 `<Component />`)

### 2. TypeScript 类型安全

#### 必须要求
- ✅ 所有组件必须使用接口或类型定义 Props
- ✅ 使用 `interface` 定义对象结构
- ✅ 使用 `type` 定义联合类型、交叉类型和工具类型
- ✅ 启用严格模式 (`strict: true`)
- ✅ 泛型类型必须显式指定 (如 `safeInvoke<VersionResult>`)
- ✅ 避免使用 `any` 类型,必要时使用 `unknown` 或具体类型

#### 类型定义示例
```typescript
// ✅ 正确: 使用 interface 定义对象结构
interface VersionTableProps {
  data: VersionResult;
  loading?: boolean;
  onSearch?: (value: string) => void;
}

// ✅ 正确: 使用 type 定义联合类型
type DownloadStatus = 'downloading' | 'success' | 'error';

// ❌ 错误: 使用 any
const result: any = await invoke('list_versions');

// ✅ 正确: 使用具体类型
const result = await safeInvoke<VersionResult>('list_versions', payload);
```

### 3. 导入顺序与组织

#### 标准导入顺序
```typescript
// 1. Node.js 内置模块
import { useState, useEffect } from 'react';

// 2. 外部依赖 (第三方库)
import { Button, Table } from 'antd';
import { invoke } from '@tauri-apps/api/core';

// 3. 内部模块 (使用 @/ 别名)
import { LangEnum } from '@/core/constants/enum';
import { store } from '@/store';

// 4. 相对导入
import { SomeComponent } from './components';
import { someUtil } from '../utils';
```

#### 导入规范
- ✅ 使用 `@/` 别名导入 `src/` 下的模块
- ✅ 导入语句之间保持空行分隔不同组别
- ✅ 删除未使用的导入

### 4. React 组件最佳实践

#### 组件结构
```typescript
// ✅ 推荐的结构
import { useState, useEffect } from 'react';
import { Button } from 'antd';
import { safeInvoke } from '@/api/tauri';

interface MyComponentProps {
  title: string;
  onAction?: () => void;
}

export const MyComponent: React.FC<MyComponentProps> = ({ title, onAction }) => {
  // 1. Hooks
  const [state, setState] = useState('');

  // 2. 副作用
  useEffect(() => {
    // ...
  }, []);

  // 3. 事件处理器
  const handleClick = () => {
    // ...
  };

  // 4. 渲染
  return <div>{title}</div>;
};
```

#### Hooks 使用规范
- ✅ Hooks 必须在组件顶层调用,不放在条件语句、循环或嵌套函数中
- ✅ 使用自定义 Hook 抽取可复用逻辑 (如 `useDownload`)
- ✅ 依赖数组必须包含所有外部依赖
- ✅ 使用 `useCallback` 缓存回调函数 (特别是传递给子组件时)
- ✅ 使用 `useMemo` 缓存昂贵的计算

#### 性能优化
- ✅ 大列表使用虚拟滚动 (Ant Design Table 的 `scroll` 属性)
- ✅ 避免不必要的重渲染 (使用 `React.memo`, `useCallback`, `useMemo`)
- ✅ 异步操作使用 `try-catch` 处理错误
- ✅ 防抖/节流高频事件 (如搜索输入)

### 5. Tauri API 调用规范

#### 使用 safeInvoke
```typescript
// ✅ 正确: 使用 safeInvoke 并指定返回类型
import { safeInvoke } from '@/api/tauri';

const result = await safeInvoke<VersionResult>('list_versions', {
  language: 'python',
  page: 0,
  pageSize: 10,
  keyWord: ''
});

// ❌ 错误: 直接使用 invoke
import { invoke } from '@tauri-apps/api/core';
const result = await invoke('list_versions', { ... });
```

#### 事件监听
```typescript
// ✅ 正确: 监听 Tauri 事件并清理
useEffect(() => {
  const unlisten = listen<ProgressPayload>('download-progress', (event) => {
    const { version, percentage } = event.payload;
    // 更新状态
  });

  return () => {
    unlisten.then(f => f());
  };
}, []);
```

#### 错误处理
```typescript
// ✅ 正确: 处理 Tauri 调用错误
try {
  await safeInvoke('install', { language: 'python', version: '3.9.7' });
  message.success('安装成功');
} catch (error) {
  message.error(`安装失败: ${error}`);
}
```

### 6. 状态管理规范

#### Redux Toolkit
```typescript
// ✅ 正确: 创建 Slice
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface ThemeState {
  isDark: boolean;
}

const initialState: ThemeState = {
  isDark: false,
};

const themeSlice = createSlice({
  name: 'theme',
  initialState,
  reducers: {
    toggleTheme: state => {
      state.isDark = !state.isDark;
    },
  },
});

export const { toggleTheme } = themeSlice.actions;
export default themeSlice.reducer;
```

#### 状态选择
```typescript
// ✅ 正确: 使用类型安全的 hooks
import { useAppDispatch, useAppSelector } from '@/store';

const dispatch = useAppDispatch();
const isDark = useAppSelector(state => state.theme.isDark);
```

#### 本地状态 vs 全局状态
- ✅ 组件内部状态使用 `useState`
- ✅ 跨组件共享状态使用 Redux
- ✅ 服务端数据使用 React Query 或自定义 Hook (本项目使用自定义 Hook)

### 7. 国际化 (i18n)

#### 使用规范
```typescript
// ✅ 正确: 使用 useTranslation hook
import { useTranslation } from 'react-i18next';

function MyComponent() {
  const { t } = useTranslation();
  return <h1>{t('welcome')}</h1>;
}

// ✅ 正确: 在语言包中定义翻译
// locales/en.json
{
  "welcome": "Welcome to LVM"
}

// locales/zh.json
{
  "welcome": "欢迎使用 LVM"
}
```

### 8. 文件命名约定

- ✅ 组件: PascalCase (如 `VersionTable.tsx`, `ThemeProvider.tsx`)
- ✅ 工具函数: camelCase (如 `store.ts`, `tauriStore.ts`)
- ✅ 常量/枚举: PascalCase (如 `LangEnum`)
- ✅ 自定义 Hook: camelCase 且以 `use` 开头 (如 `useDownload.ts`)
- ✅ 类型定义: camelCase (如 `common.ts`)

### 9. 组件设计原则

#### Props 设计
- ✅ Props 接口命名: `组件名Props` (如 `VersionTableProps`)
- ✅ 可选属性使用 `?` 标记
- ✅ 回调函数命名: `on` + 动作 (如 `onSearch`, `onInstallToggle`)
- ✅ 提供合理的默认值

#### 组件复用
- ✅ 提取可复用逻辑到自定义 Hook
- ✅ 提取可复用 UI 到共享组件 (`src/shared/components/`)
- ✅ 使用组合而非继承

#### 组件文档
- ✅ 复杂组件添加 JSDoc 注释
- ✅ 说明 Props 用途和类型

### 10. 路由与导航

#### 路由配置
```typescript
// ✅ 正确: 使用 createBrowserRouter
import { createBrowserRouter, Navigate } from 'react-router-dom';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <BasicLayout />,
    children: [
      {
        index: true,
        element: <Navigate to="/python" />,
      },
      {
        path: 'python',
        element: <PythonManagePage />,
      },
    ],
  },
]);
```

#### 导航
```typescript
// ✅ 正确: 使用 useNavigate
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/settings');
  };
}
```

### 11. Ant Design 使用规范

#### 组件使用
- ✅ 优先使用 Ant Design 组件而非自定义实现
- ✅ 遵循 Ant Design 设计规范
- ✅ 合理使用组件的 API (如 Table 的 `pagination`, `loading` 属性)
- ✅ 使用 `theme` token 保持设计一致性

#### 表单处理
```typescript
// ✅ 正确: 使用 Form 组件
import { Form, Input, Button } from 'antd';

function MyForm() {
  const [form] = Form.useForm();

  const handleSubmit = async (values) => {
    console.log(values);
  };

  return (
    <Form form={form} onFinish={handleSubmit}>
      <Form.Item name="username" rules={[{ required: true }]}>
        <Input />
      </Form.Item>
      <Button type="primary" htmlType="submit">提交</Button>
    </Form>
  );
}
```

### 12. 错误处理与边界

#### 错误边界
- ✅ 使用 React 错误边界捕获组件错误
- ✅ 提供友好的错误提示页面

#### 异步错误
```typescript
// ✅ 正确: 处理异步错误
const fetchData = async () => {
  try {
    const result = await safeInvoke('list_versions', payload);
    setData(result);
  } catch (error) {
    message.error('加载数据失败');
    console.error(error);
  }
};
```

### 13. 性能优化检查

#### 常见性能问题
- ❌ 在渲染函数中创建新对象/函数
- ❌ 不必要的重渲染
- ❌ 大列表未使用虚拟滚动
- ❌ 未优化的事件监听器
- ❌ 未清理的副作用 (定时器、订阅)

#### 优化建议
```typescript
// ✅ 正确: 使用 useCallback 缓存回调
const handleClick = useCallback(() => {
  // ...
}, [/* 依赖 */]);

// ✅ 正确: 使用 useMemo 缓存计算结果
const filteredList = useMemo(() => {
  return list.filter(item => item.active);
}, [list]);

// ✅ 正确: 使用 React.memo 避免不必要的重渲染
export const MyComponent = React.memo(({ data }) => {
  // ...
});
```

### 14. 安全性检查

#### 安全最佳实践
- ✅ 验证用户输入
- ✅ 防止 XSS 攻击 (React 默认转义,但仍需注意)
- ✅ 不在前端存储敏感信息
- ✅ 使用 HTTPS 调用 API

#### Tauri 安全
- ✅ 不暴露敏感的 Tauri 命令
- ✅ 验证 Tauri 命令参数

### 15. 可访问性 (Accessibility)

#### ARIA 属性
- ✅ 为交互元素添加适当的 ARIA 属性
- ✅ 为图标添加 `aria-label`
- ✅ 确保键盘导航可用

#### 语义化 HTML
- ✅ 使用语义化标签 (`<header>`, `<nav>`, `<main>`, `<footer>`)
- ✅ 为表单元素添加 `label`

### 16. 测试建议

#### 当前状态
- ⚠️ 项目尚未配置测试框架

#### 建议
- ✅ 为关键组件编写单元测试 (Vitest + React Testing Library)
- ✅ 为自定义 Hook 编写测试
- ✅ 为 API 调用编写集成测试
- ✅ 为关键用户流程编写端到端测试 (Playwright)

## 审查流程

### 1. 自动检查
```bash
# 运行 lint 检查
bun run lint

# 运行格式化检查
bun run format

# 运行类型检查
tsc --noEmit
```

### 2. 手动审查清单
- [ ] 代码符合 Prettier 和 ESLint 规则
- [ ] TypeScript 类型定义完整且正确
- [ ] 导入顺序正确
- [ ] 组件结构清晰,职责单一
- [ ] Hooks 使用规范
- [ ] 错误处理完善
- [ ] 性能优化到位
- [ ] 无安全漏洞
- [ ] 可访问性考虑
- [ ] 代码注释充分 (复杂逻辑)

### 3. 常见问题修复

#### 未使用的导入
```typescript
// ❌ 错误
import { useState, useEffect } from 'react';
import { Button } from 'antd';

function MyComponent() {
  const [state, setState] = useState('');
  // useEffect 未使用
  return <Button>Click</Button>;
}

// ✅ 正确
import { useState } from 'react';
import { Button } from 'antd';

function MyComponent() {
  const [state, setState] = useState('');
  return <Button>Click</Button>;
}
```

#### 缺少类型定义
```typescript
// ❌ 错误
function MyComponent({ title, onAction }) {
  return <div>{title}</div>;
}

// ✅ 正确
interface MyComponentProps {
  title: string;
  onAction?: () => void;
}

function MyComponent({ title, onAction }: MyComponentProps) {
  return <div>{title}</div>;
}
```

#### 导入顺序错误
```typescript
// ❌ 错误
import { store } from '@/store';
import { useState } from 'react';
import { Button } from 'antd';

// ✅ 正确
import { useState } from 'react';

import { Button } from 'antd';

import { store } from '@/store';
```

## 项目特定模式

### 1. 版本表格模式
```typescript
// ✅ 标准版本表格组件
import { VersionTable } from '@/shared/components/VersionTable';

function MyPage() {
  const [data, setData] = useState<VersionResult>({
    total: 0,
    list: [],
  });

  const handleSearch = async (keyWord: string) => {
    const result = await safeInvoke<VersionResult>('list_versions', {
      language: 'python',
      page: 0,
      pageSize: 10,
      keyWord,
    });
    setData(result);
  };

  const handleInstallToggle = async (record: VersionItem) => {
    await safeInvoke('install', {
      language: 'python',
      version: record.version,
    });
    handleSearch('');
  };

  return (
    <VersionTable
      data={data}
      onSearch={handleSearch}
      onInstallToggle={handleInstallToggle}
    />
  );
}
```

### 2. 下载管理模式
```typescript
// ✅ 标准下载管理 Hook
import { useDownload } from '@/hooks/useDownload';

function DownloadCenter() {
  const { tasks, startDownload } = useDownload();

  return (
    <List
      dataSource={tasks}
      renderItem={task => (
        <List.Item>
          <Progress percent={task.percentage} />
        </List.Item>
      )}
    />
  );
}
```

### 3. 配置管理模式
```typescript
// ✅ 标准配置管理
import { LazyStore } from '@tauri-apps/plugin-store';

const store = new LazyStore('.settings.json');

function Settings() {
  const [basePath, setBasePath] = useState('');

  useEffect(() => {
    store.get<string>('base_path').then(val => {
      setBasePath(val || 'd:\\lvm');
    });
  }, []);

  const handleSave = async () => {
    await store.set('base_path', basePath);
    await store.save();
    message.success('配置已保存');
  };

  return (
    <Input value={basePath} onChange={e => setBasePath(e.target.value)} />
  );
}
```

## 输出格式

**重要**: 审查报告的所有输出内容必须使用中文，除了 git commit 信息必须保持英文（遵循 Conventional Commits 规范）。

审查报告应包含:

### 1. 总体评价
- 代码质量等级 (优秀 / 良好 / 需要改进)
- 主要优点
- 主要问题

### 2. 详细问题列表
按优先级排序:
- 🔴 严重问题 (必须修复)
- 🟡 警告 (建议修复)
- 🟢 建议 (可选优化)

每个问题应包含:
- 问题描述
- 问题位置 (文件路径:行号)
- 影响范围
- 修复建议 (代码示例)

### 3. 最佳实践建议
- 性能优化建议
- 可维护性建议
- 可测试性建议

### 4. 总结
- 修复优先级
- 预计修复时间
- 其他注意事项

### 5. Git Commit 信息生成规则

**格式**: 所有 commit 信息必须使用英文，遵循 Conventional Commits 规范，保持简洁。

```
<type>(<scope>): <subject>
```

**类型选项**:
- `feat`: 新功能
- `fix`: Bug 修复
- `refactor`: 重构
- `perf`: 性能优化
- `style`: 代码风格改进
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建/CI 相关

**作用域选项**:
- `frontend`: 前端代码
- `rust`: 后端 Rust 代码
- `i18n`: 国际化
- `types`: 类型定义
- `api`: API 层
- `ui`: UI 组件

**Subject 规则**:
- 使用祈使语气 ("add" 而非 "added")
- 首字母小写
- 结尾不加句号
- 限制在 50 字符以内

**Commit 信息示例**:
```
feat(frontend): add sidebar collapse feature

refactor(ui): improve version table structure

fix(types): correct prop interface definition
```

## 参考资料

- [React 官方文档](https://react.dev/)
- [TypeScript 官方文档](https://www.typescriptlang.org/)
- [Ant Design 文档](https://ant.design/)
- [Redux Toolkit 文档](https://redux-toolkit.js.org/)
- [Tauri 官方文档](https://tauri.app/)
- [i18next 文档](https://www.i18next.com/)
- [项目 AGENTS.md](/AGENTS.md)