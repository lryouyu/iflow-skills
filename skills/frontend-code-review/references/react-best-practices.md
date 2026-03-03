# React Best Practices

This document provides React development best practices to be used as reference during code reviews.

## Hooks Best Practices

### Hooks Usage Rules

#### Rule 1: Only Call Hooks at Top Level

```typescript
// Correct
function MyComponent() {
  const [state, setState] = useState('');
  useEffect(() => {}, []);
}

// Incorrect: Hooks in conditional statements
function MyComponent() {
  if (condition) {
    const [state, setState] = useState('');
  }
}

// Incorrect: Hooks in loops
function MyComponent() {
  const items = [1, 2, 3];
  items.forEach(item => {
    const [state, setState] = useState(item);
  });
}

// Incorrect: Hooks in nested functions
function MyComponent() {
  const handleClick = () => {
    const [state, setState] = useState('');
  };
}
```

#### Rule 2: Only Call Hooks from React Functions

```typescript
// Correct
function MyComponent() {
  const [state, setState] = useState('');
}

// Incorrect: Hooks in regular functions
function myHelper() {
  const [state, setState] = useState('');
}
```

### Common Hooks Usage Patterns

#### useState

```typescript
// Simple state
const [count, setCount] = useState(0);

// Object state
const [user, setUser] = useState<User | null>(null);

// Lazy initialization
const [data, setData] = useState(() => {
  const initial = heavyComputation();
  return initial;
});

// Functional update
const [count, setCount] = useState(0);
setCount(prev => prev + 1);

// Incorrect: Direct state mutation
const [items, setItems] = useState<Item[]>([]);
items.push(newItem);  // Wrong
setItems(items);

// Correct: Create new array
const [items, setItems] = useState<Item[]>([]);
setItems(prev => [...prev, newItem]);
```

#### useEffect

```typescript
// Correct: Dependency array complete
useEffect(() => {
  fetchData(userId);
}, [userId]);

// Correct: Cleanup side effects
useEffect(() => {
  const subscription = subscribe();
  return () => subscription.unsubscribe();
}, []);

// Correct: Run once on mount
useEffect(() => {
  console.log('Component mounted');
}, []);

// Incorrect: Dependency array incomplete
useEffect(() => {
  fetchData(userId);
}, []);  // Missing userId

// Incorrect: Missing dependency array
useEffect(() => {
  fetchData(userId);
});
```

#### useCallback

```typescript
// Cache callback function
const handleClick = useCallback(() => {
  doSomething(data);
}, [data]);

// Pass to child component
const Parent = () => {
  const handleClick = useCallback((id: string) => {
    console.log(id);
  }, []);

  return <Child onClick={handleClick} />;
};

// Incorrect: Not using useCallback causes unnecessary child re-renders
const Parent = () => {
  const handleClick = (id: string) => {
    console.log(id);
  };

  return <Child onClick={handleClick} />;
};
```

#### useMemo

```typescript
// Cache expensive computation
const sortedList = useMemo(() => {
  return list.sort((a, b) => a.value - b.value);
}, [list]);

// Cache object reference
const memoizedObject = useMemo(() => ({
  id,
  name,
  data,
}), [id, name, data]);

// Incorrect: Unnecessary useMemo
const value = useMemo(() => 1 + 1, []);  // Simple calculation doesn't need memoization
```

#### useRef

```typescript
// Access DOM
const inputRef = useRef<HTMLInputElement>(null);

useEffect(() => {
  inputRef.current?.focus();
}, []);

// Save mutable value (no re-render)
const timerRef = useRef<NodeJS.Timeout>();

useEffect(() => {
  timerRef.current = setInterval(() => {
    console.log('tick');
  }, 1000);

  return () => {
    clearInterval(timerRef.current);
  };
}, []);

// Save previous value
const prevCountRef = useRef<number>();

useEffect(() => {
  prevCountRef.current = count;
}, [count]);

const prevCount = prevCountRef.current;
```

#### useContext

```typescript
// Create Context
const ThemeContext = createContext<ThemeContextType>({
  isDark: false,
  toggleTheme: () => {},
});

// Provider
const ThemeProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [isDark, setIsDark] = useState(false);

  const toggleTheme = () => setIsDark(prev => !prev);

  return (
    <ThemeContext.Provider value={{ isDark, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Consumer
const MyComponent = () => {
  const { isDark, toggleTheme } = useContext(ThemeContext);
  return <button onClick={toggleTheme}>{isDark ? 'Dark' : 'Light'}</button>;
};
```

## Component Design Best Practices

### Single Responsibility

```typescript
// Incorrect: Component has too many responsibilities
function UserManagement() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);
  const [searchTerm, setSearchTerm] = useState('');

  const fetchUsers = async () => { /* ... */ };
  const handleSearch = () => { /* ... */ };
  const handleDelete = () => { /* ... */ };
  const handleEdit = () => { /* ... */ };

  return (
    <div>
      <input onChange={e => setSearchTerm(e.target.value)} />
      <button onClick={handleSearch}>Search</button>
      <Table dataSource={users} />
    </div>
  );
}

// Correct: Split into multiple components
function UserManagement() {
  const [users, setUsers] = useState<User[]>([]);
  const [searchTerm, setSearchTerm] = useState('');

  return (
    <div>
      <UserSearch onSearch={setSearchTerm} />
      <UserList users={users} searchTerm={searchTerm} />
    </div>
  );
}

function UserSearch({ onSearch }: { onSearch: (term: string) => void }) {
  const [term, setTerm] = useState('');

  return (
    <input
      value={term}
      onChange={e => setTerm(e.target.value)}
      onBlur={() => onSearch(term)}
    />
  );
}

function UserList({ users, searchTerm }: { users: User[]; searchTerm: string }) {
  const filteredUsers = users.filter(u => u.name.includes(searchTerm));
  return <Table dataSource={filteredUsers} />;
}
```

### Props Design

```typescript
// Props interface naming convention
interface UserCardProps {
  user: User;
  onEdit?: (id: string) => void;
  onDelete?: (id: string) => void;
}

// Provide default values
const UserCard: React.FC<UserCardProps> = ({
  user,
  onEdit,
  onDelete,
}) => {
  return (
    <Card>
      <h3>{user.name}</h3>
      {onEdit && <Button onClick={() => onEdit(user.id)}>Edit</Button>}
      {onDelete && <Button onClick={() => onDelete(user.id)}>Delete</Button>}
    </Card>
  );
};

// Use children prop
interface ContainerProps {
  title: string;
  children: React.ReactNode;
}

const Container: React.FC<ContainerProps> = ({ title, children }) => {
  return (
    <div>
      <h2>{title}</h2>
      {children}
    </div>
  );
};
```

### Component Composition

```typescript
// Use composition over inheritance
interface CardProps {
  header?: React.ReactNode;
  footer?: React.ReactNode;
  children: React.ReactNode;
}

const Card: React.FC<CardProps> = ({ header, footer, children }) => {
  return (
    <div className="card">
      {header && <div className="card-header">{header}</div>}
      <div className="card-body">{children}</div>
      {footer && <div className="card-footer">{footer}</div>}
    </div>
  );
};

// Usage
<Card
  header={<h3>Title</h3>}
  footer={<Button>Action</Button>}
>
  <p>Content</p>
</Card>
```

## Performance Optimization

### React.memo

```typescript
// Use React.memo to avoid unnecessary re-renders
export const ExpensiveComponent = React.memo<ExpensiveComponentProps>(
  ({ data }) => {
    return <div>{/* Expensive rendering */}</div>;
  }
);

// Custom comparison function
export const MyComponent = React.memo<MyComponentProps>(
  ({ data, onUpdate }) => {
    return <div onClick={onUpdate}>{data}</div>;
  },
  (prevProps, nextProps) => {
    return prevProps.data === nextProps.data;
  }
);
```

### useCallback and useMemo

```typescript
// Prevent unnecessary child re-renders
const Parent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child onClick={handleClick} />;
};

// Cache computation results
const filteredList = useMemo(() => {
  return list.filter(item => item.active);
}, [list]);
```

### Virtual Scrolling

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
```

## Error Handling

### Error Boundaries

```typescript
// Create error boundary
class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean }
> {
  constructor(props: { children: React.ReactNode }) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <div>Something went wrong.</div>;
    }

    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

### Async Error Handling

```typescript
// Complete error handling
const loadData = async () => {
  setLoading(true);
  try {
    const result = await safeInvoke('load');
    setData(result);
  } catch (error) {
    message.error('Load failed');
    console.error(error);
  } finally {
    setLoading(false);
  }
};
```

## Form Handling

```typescript
// Use Ant Design Form
import { Form, Input, Button } from 'antd';

interface FormData {
  username: string;
  password: string;
}

const MyForm = () => {
  const [form] = Form.useForm<FormData>();

  const handleSubmit = async (values: FormData) => {
    try {
      await safeInvoke('login', values);
      message.success('Login successful');
    } catch (error) {
      message.error('Login failed');
    }
  };

  return (
    <Form form={form} onFinish={handleSubmit}>
      <Form.Item
        name="username"
        rules={[{ required: true, message: 'Please enter username' }]}
      >
        <Input placeholder="Username" />
      </Form.Item>

      <Form.Item
        name="password"
        rules={[{ required: true, message: 'Please enter password' }]}
      >
        <Input.Password placeholder="Password" />
      </Form.Item>

      <Button type="primary" htmlType="submit">
        Submit
      </Button>
    </Form>
  );
};
```

## Test-Friendly Design

```typescript
// Components accept props for easy testing
interface MyComponentProps {
  data: DataType[];
  onItemClick: (id: string) => void;
  loading?: boolean;
}

export const MyComponent: React.FC<MyComponentProps> = ({
  data,
  onItemClick,
  loading = false,
}) => {
  if (loading) return <Spin />;
  return <List dataSource={data} renderItem={item => (
    <Item onClick={() => onItemClick(item.id)}>{item.name}</Item>
  )} />;

// Test
test('renders list items', () => {
  const mockData = [{ id: '1', name: 'Item 1' }];
  const mockOnClick = jest.fn();

  render(<MyComponent data={mockData} onItemClick={mockOnClick} />);

  expect(screen.getByText('Item 1')).toBeInTheDocument();
});
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

### Anti-Pattern 4: Dependency Array Abuse

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

When developing components, ensure:

- [ ] Hooks called at top level
- [ ] Hooks only called in React functions
- [ ] useEffect dependency arrays complete
- [ ] Use useCallback to cache functions passed to children
- [ ] Use useMemo to cache expensive computations
- [ ] Side effects properly cleaned up
- [ ] Props interface naming convention (ComponentName + Props)
- [ ] Component has single responsibility
- [ ] Error handling is complete
- [ ] Large lists use virtual scrolling
- [ ] Don't directly mutate state
- [ ] Don't call Hooks in render