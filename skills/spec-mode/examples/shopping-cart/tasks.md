# 购物车功能 实现任务清单

## 任务概览

**功能名称**: 购物车功能
**预估总工时**: 9.5h
**优先级**: P0
**状态**: 📋 待开始

---

## Phase 1: 准备工作 (预计 0.5h)

### 1.1 环境准备
- [ ] 创建功能分支 `feat/shopping-cart`
- [ ] 确认必要的依赖已安装(Redux Toolkit, Tauri Store Plugin)

**验收标准**: 分支创建成功,依赖已安装

---

### 1.2 创建文件结构
- [ ] 创建目录结构 `src/features/shopping-cart/`
- [ ] 创建子目录: `components/`, `pages/`, `hooks/`, `api/`, `types/`, `utils/`
- [ ] 创建基础文件骨架

**验收标准**: 目录结构完整,符合项目规范

---

## Phase 2: 核心开发 (预计 4h)

### 2.1 类型定义
- [ ] 定义数据类型 `src/features/shopping-cart/types/index.ts`
  - [ ] 定义 `CartItem` 接口
  - [ ] 定义 `ShoppingCartState` 接口
- [ ] 定义 Props 接口
  - [ ] 定义 `CartItemProps` 接口
  - [ ] 定义 `CartListProps` 接口

**验收标准**: 类型定义完整,通过 TypeScript 类型检查

---

### 2.2 状态管理
- [ ] 创建 Redux Slice `src/features/shopping-cart/shoppingCartSlice.ts`
  - [ ] 定义 state 初始值
  - [ ] 实现 `addItem` action
  - [ ] 实现 `removeItem` action
  - [ ] 实现 `clearCart` action
  - [ ] 实现 `setCart` action
- [ ] 注册到 store `src/store/index.ts`
  - [ ] 导入 cartReducer
  - [ ] 添加到 store 配置
- [ ] 创建持久化中间件
  - [ ] 实现 `cartPersistenceMiddleware`
  - [ ] 集成到 store
- [ ] 创建类型安全的 hooks
  - [ ] 创建 `useAppDispatch`
  - [ ] 创建 `useAppSelector`

**验收标准**: 状态管理正常工作,类型安全

---

### 2.3 UI 组件开发

#### 2.3.1 基础组件
- [ ] 组件1: `CartItem` `src/features/shopping-cart/components/CartItem/index.tsx`
  - [ ] 定义 Props 接口
  - [ ] 实现组件布局
  - [ ] 添加删除按钮
  - [ ] 添加样式
  - [ ] 单元测试

**验收标准**: 组件功能正常,样式符合设计

#### 2.3.2 容器组件
- [ ] 组件2: `CartList` `src/features/shopping-cart/components/CartList/index.tsx`
  - [ ] 定义 Props 接口
  - [ ] 实现列表布局
  - [ ] 集成 CartItem 组件
  - [ ] 添加批量操作按钮
  - [ ] 添加空状态处理
  - [ ] 单元测试

**验收标准**: 组件功能正常,样式符合设计

#### 2.3.3 页面组件
- [ ] 页面组件: `ShoppingCart` `src/features/shopping-cart/pages/ShoppingCart/index.tsx`
  - [ ] 使用 Redux hooks 获取购物车数据
  - [ ] 实现添加到购物车逻辑
  - [ ] 实现移除逻辑
  - [ ] 实现批量安装逻辑
  - [ ] 实现批量切换逻辑
  - [ ] 实现清空购物车逻辑
  - [ ] 添加错误处理
  - [ ] 添加加载状态
  - [ ] 国际化支持

**验收标准**: 页面功能完整,用户体验良好

---

### 2.4 自定义 Hooks
- [ ] Hook: `useCart` `src/features/shopping-cart/hooks/useCart.ts`
  - [ ] 定义 Hook 接口
  - [ ] 实现添加到购物车逻辑
  - [ ] 实现移除逻辑
  - [ ] 实现清空逻辑
  - [ ] 实现批量操作逻辑
  - [ ] 单元测试

**验收标准**: Hook 功能正常,无内存泄漏

---

### 2.5 工具函数
- [ ] 工具函数: `storage` `src/features/shopping-cart/utils/storage.ts`
  - [ ] 实现 `saveCart` 函数
  - [ ] 实现 `loadCart` 函数
  - [ ] 添加类型定义
  - [ ] 单元测试

**验收标准**: 工具函数正确,性能良好

---

## Phase 3: 集成与配置 (预计 1.5h)

### 3.1 路由配置
- [ ] 添加路由配置 `src/app/routes/index.tsx`
  - [ ] 添加 `/cart` 路由
  - [ ] 测试路由导航

**验收标准**: 路由可正常访问,导航正常

---

### 3.2 国际化
- [ ] 添加翻译 keys 到 `src/features/i18n/locales/zh.json`
  - [ ] 添加 `cart.title`
  - [ ] 添加 `cart.addToCart`
  - [ ] 添加 `cart.removeFromCart`
  - [ ] 添加 `cart.batchInstall`
  - [ ] 添加 `cart.batchSwitch`
  - [ ] 添加 `cart.clearCart`
  - [ ] 添加 `cart.empty`
  - [ ] 添加 `cart.itemCount`
  - [ ] 添加 `cart.confirmClear`
  - [ ] 添加 `cart.installing`
  - [ ] 添加 `cart.switching`
  - [ ] 添加 `cart.success`
- [ ] 添加翻译 keys 到 `src/features/i18n/locales/en.json`
  - [ ] 同上,英文翻译
- [ ] 在组件中使用 `useTranslation` hook

**验收标准**: 中英文切换正常,所有文本已翻译

---

### 3.3 侧边栏集成
- [ ] 在侧边栏添加购物车菜单项 `src/layouts/BasicLayout/Sider.tsx`
  - [ ] 添加菜单项
  - [ ] 添加数量徽标(Badge)
  - [ ] 测试徽标更新

**验收标准**: 侧边栏显示购物车菜单项,数量实时更新

---

### 3.4 版本列表集成
- [ ] 在版本列表页面添加"添加到购物车"按钮
  - [ ] 修改 `VersionTable` 组件
  - [ ] 添加 `onAddToCart` 回调
  - [ ] 测试添加功能

**验收标准**: 版本列表可以添加到购物车

---

## Phase 4: 测试与优化 (预计 2h)

### 4.1 单元测试
- [ ] 编写 shoppingCartSlice 单元测试
- [ ] 编写 storage 工具函数单元测试
- [ ] 编写 useCart Hook 单元测试
- [ ] 编写 CartItem 组件单元测试
- [ ] 编写 CartList 组件单元测试

**验收标准**: 测试覆盖率 > 80%,所有测试通过

---

### 4.2 集成测试
- [ ] 测试购物车组件集成
- [ ] 测试 Redux 状态管理
- [ ] 测试持久化功能
- [ ] 测试路由导航

**验收标准**: 集成测试通过,无错误

---

### 4.3 手动测试
- [ ] 测试添加到购物车功能
- [ ] 测试从购物车移除功能
- [ ] 测试批量安装功能
- [ ] 测试批量切换功能
- [ ] 测试清空购物车功能
- [ ] 测试购物车持久化
- [ ] 测试边界情况(空购物车、重复添加等)
- [ ] 测试国际化切换
- [ ] 测试不同浏览器(如适用)

**验收标准**: 所有测试用例通过

---

### 4.4 性能优化
- [ ] 使用 React DevTools 分析性能
- [ ] 优化不必要的渲染
  - [ ] 使用 `React.memo` 优化 CartItem
  - [ ] 使用 `useMemo` 优化计算
  - [ ] 使用 `useCallback` 优化事件处理

**验收标准**: 性能指标达标,用户体验流畅

---

## Phase 5: 代码质量 (预计 0.5h)

### 5.1 代码审查
- [ ] 自我代码审查
  - [ ] 检查类型定义是否完整
  - [ ] 检查错误处理是否完善
  - [ ] 检查代码风格是否一致
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
- [ ] 更新类型定义注释
- [ ] 如需要,更新 README

**验收标准**: 文档完整准确

---

## Phase 6: 部署准备 (预计 1h)

### 6.1 构建测试
- [ ] 运行 `bun run build`
- [ ] 检查构建产物
- [ ] 运行 `bun run tauri build` (测试桌面应用构建)

**验收标准**: 构建成功,无错误

---

### 6.2 代码提交
- [ ] 使用 Conventional Commits 格式编写提交信息
  ```
  feat(frontend): add shopping cart feature

  - Implement shopping cart page with CRUD operations
  - Add Redux slice for cart state management
  - Implement batch install and switch functionality
  - Add internationalization support for cart
  - Integrate cart into sidebar with badge counter

  Closes #123
  ```
- [ ] 提交代码
- [ ] 推送到远程仓库

**验收标准**: 提交信息规范,代码已推送

---

### 6.3 创建 Pull Request
- [ ] 创建 PR
- [ ] 填写 PR 描述
  - [ ] 选择变更类型: ✨ 新功能
  - [ ] 填写变更说明
  - [ ] 列出主要变更
  - [ ] 填写测试情况
  - [ ] 添加截图(如有)
  - [ ] 关联相关 Issue
- [ ] 添加合适的 labels: `frontend`, `enhancement`, `pr`

**验收标准**: PR 创建成功,信息完整

---

## 任务依赖关系

```
Phase 1 (准备工作)
    ↓
Phase 2 (核心开发)
    ├── 2.1 类型定义
    ├── 2.2 状态管理
    ├── 2.3 UI 组件开发
    │   ├── 2.3.1 CartItem 组件
    │   ├── 2.3.2 CartList 组件
    │   └── 2.3.3 ShoppingCart 页面
    ├── 2.4 自定义 Hooks
    └── 2.5 工具函数
    ↓
Phase 3 (集成与配置)
    ├── 3.1 路由配置
    ├── 3.2 国际化
    ├── 3.3 侧边栏集成
    └── 3.4 版本列表集成
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
| 批量操作可能耗时较长 | 中 | 显示进度提示,允许用户取消操作 |
| 购物车数据丢失 | 高 | 使用 Tauri Store 持久化,定期测试 |
| 用户误操作清空购物车 | 中 | 添加确认对话框 |
| 类型定义不完整 | 中 | 严格的 TypeScript 配置,充分测试 |

---

## 进度跟踪

**开始日期**: [YYYY-MM-DD]
**预计完成日期**: [YYYY-MM-DD]
**实际完成日期**: [YYYY-MM-DD]

**当前进度**: 0%

| Phase | 状态 | 完成度 | 耗时 |
|-------|------|--------|------|
| Phase 1: 准备工作 | ⬜ 未开始 | 0% | 0h |
| Phase 2: 核心开发 | ⬜ 未开始 | 0% | 0h |
| Phase 3: 集成与配置 | ⬜ 未开始 | 0% | 0h |
| Phase 4: 测试与优化 | ⬜ 未开始 | 0% | 0h |
| Phase 5: 代码质量 | ⬜ 未开始 | 0% | 0h |
| Phase 6: 部署准备 | ⬜ 未开始 | 0% | 0h |

图例:
- ⬜ 未开始
- 🟡 进行中
- 🟢 已完成

---

## 备注

- 购物车功能依赖现有的版本列表、安装和切换功能
- 需要与侧边栏和版本列表页面进行集成
- 批量操作需要考虑性能和用户体验
- 所有用户可见文本需要支持中英文双语