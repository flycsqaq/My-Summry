# 1. 问题
Vue与angular都是提高开发效率的前端框架。其核心都在于组件管理、数据管理与路由。
## 1.1 组件管理
### 1. 数据交互
1. 父子组件：父传递数据给子，子通过发出(emit)事件改变父的数据。
2. 第三方
但数据共享就相当于产生了[依赖](../othors/philosophy.md),过多的依赖会造成数据越来越不可维护。
## 1.2 生命周期
### 1. 数据(created)
1. 内部数据
2. 外部数据
### 2. 视图(mounted)
1. 编译模板
2. 渲染模板
### 3. 更新(update)
1. 数据
2. 模板
### 4. 销毁(destory)
1. 销毁数据与监听事件
2. 销毁组件

### Angular
1. ngOnChange
2. ngOnInit
3. ngDoCheck
4. ngAfterContentInit
5. ngContentChecked
6. ngAfterViewInit
7. ngViewChecked
8. ngOnDestory

## 2. 数据管理
### Vuex
1. state
2. mutations
3. actions
4. getters

### service
1. api
2. rxjs
3. Subject / Observable / subscribe
4. localStorage

## 3. 路由
1. 路由守卫
2. 数据预获取
3. 异步路由