# LVM Project Code Standards

This document provides detailed code standards for the LVM project to be used as reference during code reviews.

## Table of Contents

1. [Code Style](#code-style)
2. [TypeScript Standards](#typescript-standards)
3. [React Standards](#react-standards)
4. [Tauri API Standards](#tauri-api-standards)
5. [State Management Standards](#state-management-standards)
6. [Performance Optimization Standards](#performance-optimization-standards)
7. [Security Standards](#security-standards)

## Code Style

### Prettier Configuration

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "trailingComma": "all",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "crlf"
}
```

### ESLint Key Rules

```javascript
{
  // No unused imports
  'unused-imports/no-unused-imports': 'error',

  // No unused variables (warning level)
  'unused-imports/no-unused-vars': 'warn',

  // Enforce import order
  'import/order': [
    'warn',
    {
      groups: ['builtin', 'external', 'internal', 'parent', 'sibling', 'index'],
      'newlines-between': 'always',
    },
  ],

  // React JSX boolean value omission
  'react/jsx-boolean-value': ['warn', 'never'],

  // Self-closing tags
  'react/self-closing-comp': 'warn',
}
```

### File Naming Conventions

| Type | Convention | Examples |
|------|-----------|----------|
| Components | PascalCase | `VersionTable.tsx`, `ThemeProvider.tsx` |
| Utility Functions | camelCase | `store.ts`, `tauriStore.ts` |
| Constants/Enums | PascalCase | `LangEnum` |
| Hooks | camelCase + `use` prefix | `useDownload.ts`, `useFetch.ts` |
| Type Definitions | camelCase | `common.ts`, `api.ts` |

## TypeScript Standards

### Type Definition Standards

```typescript
// Use interface for object structures
interface VersionTableProps {
  data: VersionResult;
  loading?: boolean;
  onSearch?: (value: string) => void;
}

// Use type for union types
type DownloadStatus = 'downloading' | 'success' | 'error';

// Use type for utility types
type Nullable<T> = T | null;
type Partial<T> = { [P in keyof T]?: T[P] };

// Explicitly specify generics
const result = await safeInvoke<VersionResult>('list_versions', payload);

// Avoid using any
const data: any = await invoke('command');

// Use unknown instead of any
const data: unknown = await invoke('command');
```

### Props Interface Naming

```typescript
// Correct: ComponentName + Props
interface VersionTableProps { }
interface MyComponentProps { }

// Incorrect
interface Props { }
interface IVersionTable { }
```

### Type Exports

```typescript
// Explicitly export types
export interface MyProps { }
export type MyType = string | number;

// Export type aliases
export type { MyType };
```

## React Standards

### Component Structure

```typescript
// Recommended component structure
import { useState, useEffect, useCallback } from 'react';
import { Button } from 'antd';
import { safeInvoke } from '@/api/tauri';

interface MyComponentProps {
  title: string;
  onAction?: () => void;
}

export const MyComponent: React.FC<MyComponentProps> = ({ title, onAction }) => {
  // 1. Hooks
  const [state, setState] = useState('');

  // 2. Side effects
  useEffect(() => {
    // effect logic
  }, []);

  // 3. Event handlers (using useCallback)
  const handleClick = useCallback(() => {
    // handler logic
  }, [/* dependencies */]);

  // 4. Render
  return (
    <div>
      <Button onClick={handleClick}>{title}</Button>
    </div>
  );
};
```

### Hooks Usage Standards

```typescript
// Hooks called at top level
function MyComponent() {
  const [state, setState] = useState('');
  useEffect(() => {}, []);
  // Correct
}

// Incorrect: Hooks in conditional statements
function MyComponent() {
  if (condition) {
    const [state, setState] = useState('');
  }
}

// Correct: Dependency array complete
useEffect(() => {
  fetchData(userId);
}, [userId]);

// Incorrect: Incomplete dependency array
useEffect(() => {
  fetchData(userId);
}, []);  // Missing userId

// Use useCallback to cache callbacks
const handleClick = useCallback(() => {
  doSomething(data);
}, [data]);

// Use useMemo to cache computations
const filteredList = useMemo(() => {
  return list.filter(item => item.active);
}, [list]);
```

### Event Handling

```typescript
// Correct: Use useCallback
const handleSubmit = useCallback(async (values: FormValues) => {
  try {
    await safeInvoke('submit', { data: values });
    message.success('Submit successful');
  } catch (error) {
    message.error('Submit failed');
  }
}, []);

// Incorrect: Inline function causing unnecessary re-renders
<Button onClick={() => handleAction(item.id)}>
  Click
</Button>

// Correct: Use cached function
const handleItemClick = useCallback((id: string) => {
  console.log(id);
}, [handleAction]);

<Button onClick={() => handleItemClick(item.id)}>
  Click
</Button>
```

## Tauri API Standards

### Using safeInvoke

```typescript
// Correct: Use safeInvoke with return type
import { safeInvoke } from '@/api/tauri';

const result = await safeInvoke<VersionResult>('list_versions', {
  language: 'python',
  page: 0,
  pageSize: 10,
  keyWord: ''
});

// Incorrect: Direct invoke
import { invoke } from '@tauri-apps/api/core';
const result = await invoke('list_versions', { ... });
```

### Event Listening

```typescript
// Correct: Listen and cleanup
useEffect(() => {
  const unlisten = listen<ProgressPayload>('download-progress', (event) => {
    const { version, percentage } = event.payload;
    setProgress(prev => ({ ...prev, [version]: percentage }));
  });

  return () => {
    unlisten.then(f => f());
  };
}, []);

// Incorrect: No cleanup
useEffect(() => {
  listen('event', handler);
  // Missing cleanup
}, []);
```

### Error Handling

```typescript
// Correct: Complete error handling
const handleInstall = async (version: string) => {
  try {
    await safeInvoke('install', {
      language: 'python',
      version,
    });
    message.success('Install successful');
    await refreshList();
  } catch (error) {
    message.error(`Install failed: ${error}`);
    console.error('Install error:', error);
  }
};

// Incorrect: No error handling
const handleInstall = async (version: string) => {
  await safeInvoke('install', { version });
  refreshList();
};
```

## State Management Standards

### Redux Toolkit

```typescript
// Slice definition
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface ThemeState {
  isDark: boolean;
  primaryColor: string;
}

const initialState: ThemeState = {
  isDark: false,
  primaryColor: '#1890ff',
};

const themeSlice = createSlice({
  name: 'theme',
  initialState,
  reducers: {
    toggleTheme: state => {
      state.isDark = !state.isDark;
    },
    setPrimaryColor: (state, action: PayloadAction<string>) => {
      state.primaryColor = action.payload;
    },
  },
});

export const { toggleTheme, setPrimaryColor } = themeSlice.actions;
export default themeSlice.reducer;
```

### Using Typed Hooks

```typescript
// Correct: Use typed hooks
import { useAppDispatch, useAppSelector } from '@/store';

function MyComponent() {
  const dispatch = useAppDispatch();
  const isDark = useAppSelector(state => state.theme.isDark);

  const handleToggle = () => {
    dispatch(toggleTheme());
  };
}

// Incorrect: Use untyped hooks
function MyComponent() {
  const dispatch = useDispatch();
  const isDark = useSelector(state => state.theme.isDark);
}
```

### Local vs Global State

```typescript
// Local state with useState
function MyComponent() {
  const [localState, setLocalState] = useState('');
}

// Cross-component state with Redux
function Parent() {
  const globalState = useAppSelector(state => state.global.value);
}

// Server data with custom Hook
function useVersionList() {
  const [data, setData] = useState<VersionResult>({ total: 0, list: [] });

  useEffect(() => {
    safeInvoke<VersionResult>('list_versions', {}).then(setData);
  }, []);

  return data;
}
```

## Performance Optimization Standards

### Avoid Unnecessary Re-renders

```typescript
// Use React.memo
export const ExpensiveComponent = React.memo<ExpensiveComponentProps>(
  ({ data }) => {
    return <div>{/* Expensive rendering */}</div>;
  }
);

// Use useCallback
const handleClick = useCallback(() => {
  setCount(c => c + 1);
}, []);

// Use useMemo
const expensiveValue = useMemo(() => {
  return heavyCalculation(data);
}, [data]);
```

### List Optimization

```typescript
// Large lists use virtual scrolling
import { List } from 'antd';

const LargeList = ({ items }: { items: Item[] }) => {
  return (
    <List
      dataSource={items}
      renderItem={item => <List.Item>{item.name}</List.Item>}
      pagination={{
        pageSize: 20,
      }}
      scroll={{ y: 500 }}
    />
  );
};

// Use stable keys
{list.map(item => (
  <Item key={item.id}>{item.name}</Item>
))}

// Incorrect: Use index as key
{list.map((item, index) => (
  <Item key={index}>{item.name}</Item>
))}
```

### Debounce and Throttle

```typescript
// Debounce search input
import { debounce } from 'lodash-es';

const debouncedSearch = useCallback(
  debounce((value: string) => {
    handleSearch(value);
  }, 300),
  []
);

// Throttle scroll events
const handleScroll = useCallback(
  throttle(() => {
    // scroll logic
  }, 100),
  []
);
```

## Security Standards

### Input Validation

```typescript
// Validate user input
const handleSubmit = async (values: FormValues) => {
  if (!values.username || values.username.length < 3) {
    message.error('Username must be at least 3 characters');
    return;
  }

  await safeInvoke('submit', { data: values });
};
```

### Prevent XSS

```typescript
// React auto-escapes, but be careful
function MyComponent({ htmlContent }) {
  // Dangerous: Direct HTML rendering
  // return <div dangerouslySetInnerHTML={{ __html: htmlContent }} />;

  // Safe: Use text
  return <div>{htmlContent}</div>;
}
```

### Sensitive Information Handling

```typescript
// Incorrect: Store sensitive info in localStorage
localStorage.setItem('password', password);

// Correct: Use Tauri Store or backend storage
const store = new LazyStore('.settings.json');
await store.set('apiKey', apiKey);  // Store in local app config
```

## Common Anti-Patterns

### Anti-Pattern 1: Direct State Mutation

```typescript
// Incorrect
const [items, setItems] = useState<Item[]>([]);
items.push(newItem);
setItems(items);

// Correct
const [items, setItems] = useState<Item[]>([]);
setItems(prev => [...prev, newItem]);
```

### Anti-Pattern 2: Calling Hooks in Render

```typescript
// Incorrect
function MyComponent() {
  if (condition) {
    const [state, setState] = useState('');
  }
}

// Correct
function MyComponent() {
  const [state, setState] = useState('');
  if (condition) {
    // Use state
  }
}
```

### Anti-Pattern 3: Overusing useEffect

```typescript
// Incorrect: Can be calculated directly
const [count, setCount] = useState(0);
const [doubled, setDoubled] = useState(0);

useEffect(() => {
  setDoubled(count * 2);
}, [count]);

// Correct: Calculate directly
const [count, setCount] = useState(0);
const doubled = count * 2;
```

### Anti-Pattern 4: Abusing Dependency Arrays

```typescript
// Incorrect: Use empty array to bypass dependency check
useEffect(() => {
  fetchData(userId);
}, []);  // eslint-disable-line react-hooks/exhaustive-deps

// Correct: Include all dependencies
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

## Checklist

Before committing code, ensure:

- [ ] All files pass ESLint check
- [ ] All files comply with Prettier format
- [ ] TypeScript compilation passes
- [ ] All components have Props interface
- [ ] All Hooks have complete dependency arrays
- [ ] Use useCallback for functions passed to children
- [ ] Use useMemo for expensive computations
- [ ] Side effects properly cleaned up
- [ ] Props interface naming convention (ComponentName + Props)
- [ ] Component has single responsibility
- [ ] Error handling is complete
- [ ] Large lists use virtual scrolling
- [ ] Debounce/throttle high-frequency events
- [ ] Don't directly mutate state
- [ ] Don't call Hooks in render