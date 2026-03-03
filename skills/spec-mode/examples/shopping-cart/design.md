# 购物车功能 技术设计

## 1. 总体设计

### 1.1 架构概览
购物车功能作为版本管理的一个增强功能,集成到现有的版本管理流程中。

```
┌─────────────────────────────────────────────────────────┐
│                      前端应用                            │
├─────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │ 版本列表页面  │──│ 购物车页面   │──│ 下载中心     │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
│         │                 │                  │        │
│         └─────────────────┴──────────────────┘        │
│                           │                            │
│                  ┌────────▼────────┐                   │
│                  │  Redux Store    │                   │
│                  │  (cartSlice)    │                   │
│                  └────────┬────────┘                   │
│                           │                            │
│                  ┌────────▼────────┐                   │
│                  │  Tauri Store    │                   │
│                  │  (持久化)       │                   │
│                  └─────────────────┘                   │
└─────────────────────────────────────────────────────────┘
```

### 1.2 技术选型
- 前端框架: React 19.1.0
- UI 组件库: Ant Design 6.3.0
- 状态管理: Redux Toolkit 2.11.2
- 持久化存储: Tauri Store Plugin 2.4.2
- 国际化: i18next 25.8.13
- 路由: React Router DOM 7.13.0

### 1.3 文件结构
```
src/
├── features/
│   └── shopping-cart/
│       ├── components/
│       │   ├── CartItem/
│       │   │   └── index.tsx
│       │   └── CartList/
│       │       └── index.tsx
│       ├── pages/
│       │   └── ShoppingCart/
│       │       └── index.tsx
│       ├── hooks/
│       │   └── useCart.ts
│       ├── api/
│       │   └── index.ts
│       ├── types/
│       │   └── index.ts
│       └── utils/
│           └── storage.ts
├── core/
│   ├── constants/
│   │   └── enum.ts (添加购物车相关枚举)
│   └── types/
│       └── common.ts (添加购物车相关类型)
└── store/
    └── index.ts (注册 cartSlice)
```

## 2. 详细设计

### 2.1 组件设计

#### 新增组件

| 组件名 | 文件路径 | 职责 | Props | 依赖 |
|--------|---------|------|-------|------|
| CartItem | shopping-cart/components/CartItem/index.tsx | 显示单个购物车项 | `{ item: CartItem; onRemove: () => void }` | Ant Design List.Item, Button |
| CartList | shopping-cart/components/CartList/index.tsx | 购物车列表容器 | `{ items: CartItem[]; onRemove: (item: CartItem) => void }` | CartItem, Ant Design List |
| ShoppingCart | shopping-cart/pages/ShoppingCart/index.tsx | 购物车主页面 | - | CartList, Ant Design Card |

#### 复用组件
- `Button` - 从 Ant Design 复用
- `List` - 从 Ant Design 复用
- `Empty` - 从 Ant Design 复用
- `Badge` - 从 Ant Design 复用(侧边栏数量徽标)

### 2.2 数据流设计

#### 2.2.1 状态管理

**Redux Slice**:
```typescript
// features/shopping-cart/shoppingCartSlice.ts
interface ShoppingCartState {
  items: CartItem[];
  total: number;
}

const shoppingCartSlice = createSlice({
  name: 'shoppingCart',
  initialState: {
    items: [],
    total: 0,
  } as ShoppingCartState,
  reducers: {
    addItem: (state, action: PayloadAction<CartItem>) => {
      state.items.push(action.payload);
      state.total = state.items.length;
    },
    removeItem: (state, action: PayloadAction<{ language: string; version: string }>) => {
      state.items = state.items.filter(
        item => !(item.language === action.payload.language && item.version === action.payload.version)
      );
      state.total = state.items.length;
    },
    clearCart: (state) => {
      state.items = [];
      state.total = 0;
    },
    setCart: (state, action: PayloadAction<CartItem[]>) => {
      state.items = action.payload;
      state.total = action.payload.length;
    },
  },
});

export const { addItem, removeItem, clearCart, setCart } = shoppingCartSlice.actions;
export default shoppingCartSlice.reducer;
```

**数据流向**:
```
用户操作 → 组件 dispatch action → Redux reducer 更新 state → 组件重新渲染
                                                    ↓
                                            Tauri Store 持久化
```

#### 2.2.2 持久化策略
```typescript
// 使用 Redux 中间件自动持久化到 Tauri Store
const cartPersistenceMiddleware: Middleware = (store) => (next) => (action) => {
  const result = next(action);

  if (action.type.startsWith('shoppingCart/')) {
    const state = store.getState();
    saveCartToTauriStore(state.shoppingCart.items);
  }

  return result;
};
```

### 2.3 API 设计

#### 2.3.1 Tauri Commands
由于购物车数据存储在前端,不需要额外的后端 API。使用现有的 Tauri Store Plugin 进行持久化。

#### 2.3.2 存储操作
```typescript
// features/shopping-cart/utils/storage.ts
import { Store } from '@tauri-apps/plugin-store';

const STORE_KEY = 'shopping_cart';

export const saveCart = async (items: CartItem[]): Promise<void> => {
  const store = await Store.load('settings.json');
  await store.set(STORE_KEY, items);
  await store.save();
};

export const loadCart = async (): Promise<CartItem[]> => {
  const store = await Store.load('settings.json');
  const items = await store.get<CartItem[]>(STORE_KEY);
  return items || [];
};
```

### 2.4 路由设计
```typescript
// app/routes/index.tsx
{
  path: 'cart',
  element: <ShoppingCart />,
}
```

### 2.5 国际化设计

#### 新增翻译 keys
```json
// features/i18n/locales/zh.json
{
  "cart": {
    "title": "购物车",
    "addToCart": "添加到购物车",
    "removeFromCart": "移除",
    "batchInstall": "批量安装",
    "batchSwitch": "批量切换",
    "clearCart": "清空购物车",
    "empty": "购物车为空",
    "itemCount": "{count} 个项目",
    "confirmClear": "确定要清空购物车吗?",
    "installing": "正在安装...",
    "switching": "正在切换...",
    "success": "操作成功"
  }
}

// features/i18n/locales/en.json
{
  "cart": {
    "title": "Shopping Cart",
    "addToCart": "Add to Cart",
    "removeFromCart": "Remove",
    "batchInstall": "Batch Install",
    "batchSwitch": "Batch Switch",
    "clearCart": "Clear Cart",
    "empty": "Cart is empty",
    "itemCount": "{count} items",
    "confirmClear": "Are you sure you want to clear the cart?",
    "installing": "Installing...",
    "switching": "Switching...",
    "success": "Operation successful"
  }
}
```

#### 使用方式
```typescript
const { t } = useTranslation();
<Button>{t('cart.addToCart')}</Button>
```

## 3. 类型定义

### 3.1 数据类型
```typescript
// features/shopping-cart/types/index.ts
export interface CartItem {
  language: string;
  version: string;
  addedAt: number;
}

export interface ShoppingCartState {
  items: CartItem[];
  total: number;
}
```

### 3.2 Props 类型
```typescript
// features/shopping-cart/components/CartItem/index.tsx
export interface CartItemProps {
  item: CartItem;
  onRemove: () => void;
}

// features/shopping-cart/components/CartList/index.tsx
export interface CartListProps {
  items: CartItem[];
  onRemove: (item: CartItem) => void;
  onBatchInstall: () => void;
  onBatchSwitch: () => void;
  onClearCart: () => void;
}
```

## 4. 实现细节

### 4.1 核心逻辑

**添加到购物车**:
```typescript
const handleAddToCart = (version: VersionItem) => {
  const cartItem: CartItem = {
    language: LanguageEnum.PYTHON,
    version: version.version,
    addedAt: Date.now(),
  };
  dispatch(addItem(cartItem));
  message.success(t('cart.addToCart') + ' ' + t('cart.success'));
};
```

**批量安装**:
```typescript
const handleBatchInstall = async () => {
  for (const item of items) {
    await safeInvoke(CommandEnum.INSTALL, {
      language: item.language,
      version: item.version,
    });
  }
  dispatch(clearCart());
  message.success(t('cart.success'));
};
```

### 4.2 边界情况处理
| 场景 | 处理方式 |
|------|---------|
| 添加重复版本 | 检查是否已存在,已存在则提示 |
| 购物车为空 | 显示 Empty 组件 |
| 批量操作失败 | 显示错误信息,保留购物车内容 |
| 页面刷新 | 从 Tauri Store 恢复购物车数据 |

### 4.3 错误处理
```typescript
try {
  await handleBatchInstall();
} catch (error) {
  console.error('[ShoppingCart] Batch install error:', error);
  message.error(t('cart.error.installFailed'));
}
```

### 4.4 性能优化
- 使用 `useCallback` 缓存事件处理函数
- 购物车数量计算使用 memo
- 使用 `React.memo` 优化 CartItem 组件

## 5. 测试策略

### 5.1 单元测试
- Redux slice reducer 测试
- storage 工具函数测试
- 自定义 Hook 测试

### 5.2 集成测试
- 购物车组件集成测试
- 持久化集成测试

### 5.3 E2E 测试
- 添加到购物车流程
- 批量安装流程
- 清空购物车流程

## 6. 代码规范

### 6.1 命名规范
- 组件: PascalCase (`ShoppingCart.tsx`)
- Hook: camelCase + `use` 前缀 (`useCart.ts`)
- 工具函数: camelCase (`storage.ts`)
- 类型/接口: PascalCase (`CartItem`, `ShoppingCartState`)
- Redux action: camelCase (`addItem`, `removeItem`)

### 6.2 代码风格
- 遵循 `.prettierrc` 配置
- 遵循 `eslint.config.js` 规则
- 导入顺序: 内置 → 外部 → 内部(@/) → 相对

### 6.3 注释规范
- 复杂逻辑添加 JSDoc 注释
- 注释解释"为什么"而非"是什么"

## 7. 部署与发布

### 7.1 环境配置
- 开发环境: 使用 Tauri 开发模式
- 生产环境: 使用 Tauri 构建模式

### 7.2 发布清单
- [ ] 代码审查通过
- [ ] 所有测试通过
- [ ] 文档更新完成
- [ ] 无安全漏洞
- [ ] 性能指标达标

## 8. 参考实现

### 8.1 项目内类似功能
- 下载中心: `src/features/version-manager/pages/DownloadCenter/`
- 版本列表: `src/features/version-manager/pages/PythonManagePage/`

### 8.2 最佳实践参考
- Redux Toolkit 官方文档
- Ant Design 列表组件文档
- Tauri Store Plugin 文档

## 9. 待确认事项

- [ ] 是否需要购物车排序功能
- [ ] 是否需要购物车导出/导入功能
- [ ] 批量操作是否需要进度显示

## 10. 设计评审

### 10.1 评审问题
1. 购物车数据是否需要同步到云端?
2. 批量操作是否需要支持取消?
3. 购物车数量是否需要限制?

### 10.2 评审结论
- [ ] 通过
- [ ] 需要修改
- [ ] 重新设计

### 10.3 评审意见
待评审