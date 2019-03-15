# 1. ngModule
## 1.1总览
### 1.1.1 参数表
|参数|作用|
|:---|:----------------|
|imports（导入表）|将其他module中exports的内容导入|
|exports（导出表）|将能在其他module中使用的内容导出|
|declarations（可声明对象表）|那些属于本NgModule的组件、指令、管道|
|providers（服务）|能在该NgModule及其下属部分使用的服务（数据），在上级作用域创建的服务实例，能在下级作用域下使用|
|bootstrap（根导入）|根模块设置的属性|

### 1.1.2 总结
因此，在该模块下的组件能直接使用的有
1. 导入表的模块所导出的对象
2. 自身可声明对象表
3. 上级模块创建的服务实例
4. NgModule是用来代替js的export/import管理模块。
## 1.2运用
### 1.2.1 可复用组件
根据1.1.2中的第一条，我们可以设计一个Shared模块，将可复用的组件都添加到该模块的exports参数中，然后在需要调用这些组件的模块的imports参数中添加Shared模块。
### 1.2.2 服务
1. 服务的调用遵循js语法关于作用域下的变量覆盖。
2. 服务大多是异步获取数据，可以使用rxjs来处理异步。
## 1.3 关联
模块将组件与服务关联起来，组件从服务中获取数据，再将数据渲染成视图。
<br>
<br>
<cite>参考自[angular官方文档架构概览](https://angular.cn/guide/architecture)</cite>

# 2. 组件
## 2.1 生命周期钩子
1. ngOnChanges(): 组件接受数据绑定输入 @Input()
2. ngOnInit(): 第一次调用ngOnChanges()之后调用，且只调用一次。
3. ngDoCheck(): 检测，并在发生 Angular 无法或不愿意自己检测的变化时作出反应。可能是指在组件内创建的变量变化。(疑惑)
4. ngAfterContentInit(): 父件第一次将外部内容投影进子件调用
5. ngAfterContentChecked(): 检测，父件投影内容变更后调用
6. ngAfterViewInit(): 初始化完视图及其子视图后调用
7. ngAfterViewChecked(): 视图变更检测之后调用
8. ngOnDestroy(): 销毁前调用

# 2.2 钩子调用
1. 创建组件，组件是一个类，会先创建组件实例，然后再根据生命周期调用各个函数。
2. 先接受父件传递的数据，然后再接受父件传递的内容投影，再完成视图。可能是一个递归创建的过程。
```
function createViews() {
  // ngOnChanges --> ngAfterContentInit
  for (let i = 0; i<childViews.length; i++) {
    return createView(childViews[i])
  }
  // ngAfterViewInit
  return views
}
```
3. 接受数据后，调用ngOnChanges, ngOnInit, ngDoCheck
4. 接受投影后，调用ngAfterContentInit, ngAfterContentChecked 
5. 视图完成后，调用ngAfterViewInit, ngAfterViewChecked
6. 当数据变动时，angular进行脏检查，调用相应的组件的相应的函数钩子，例如接受的数据变化，

# 3. 指令
## 3.1 结构型指令
1. *ngIf
2. *ngFor
3. ngSwitch
```
<div [ngSwitch]="emotion">
  <div *ngSwitchCase="happy"></div>
  <div *ngSwitchCase="sad"></div>
  <div *ngSwitchDefault></div>
</div>
```
4. ng-template: 用于渲染html元素的空白容器
5. ng-container: 用于渲染文字内容的空白容器
6. *appUnless指令
```
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({ selector: '[appUnless]'})
export class UnlessDirective {
  private hasView = false;

  constructor(
    private templateRef: TemplateRef<any>, 
    private viewContainer: ViewContainerRef 
  ) { }
  // 参数: condition
  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef); //创建元素
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear(); //清除元素
      this.hasView = false;
    }
  }
}
```

## 3.2 属性型指令
constructor: 传入DOM元素
1. 设置背景色

```
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
// 传入DOM元素，设置元素的背景色
export class HighlightDirective {
    constructor(el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```
2. 根据监听鼠标事件改变元素颜色

```
import { Directive, ElementRef, HostListener } from '@angular/core';
 
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) { }
 
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }
 
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }
 
  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```
3. 接受数据

```
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  constructor(private el: ElementRef) { }

  @Input('appHighlight') highlightColor: string;

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'red');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

# 4. 表单
angular提供了两种不同的方式来通过表单处理用户输入：响应式表单和模板驱动表单。
## 4.1 响应式表单
响应式表单，借助rxjs中的Observable流来控制数据，更改input输入框中的内容，吐出新数据，通过subscribe订阅数据吗，因此是同步的。因此你在请求时可以确信数据是一致的。因为Observable流只负责吐出新数据，并不会对数据进行改变。大概是这样的
```
<input type="text" [formControl]="name">
private name = new FormControl('');
```
## 4.2 模板驱动表单
模板驱动表单，与Vue中创建表单的方式相似，先更改ts模板中的数据，再在检测到数据变化更改视图，因此是异步的。
## 4.3 表单验证
可以看出是函数中的定义域。

# 5.路由
## 5.1 NgModule
注意模块导入顺序

```
const appRoutes: Routes = [
  { path: 'heroes', component: HeroListComponent },
  { path: '', redirectTo: '/heroes', pathMatch: 'full' },
]
imports: [
  RouterModule.forRoot(
    appRoutes
  )
]
exports: [
  RouterModule
] 
//子路由
imports: [
  RouterModule.forchild(
    appRouteChild
  )
]
exports: [
  RouterModule
] 
```
## 5.2 路由链接
1. routerLink
2. router-outlet
3. 

```
this.router.navigate(['/heroes']);
```
## 5.3 路由参数
通过ActivatedRoute模块导航，获取参数
1. 复用，通过Observable实现的，每次都是同一个组件，只改变了路由，例如从article/1 => artile/2

```
this.hero$ = this.route.paramMap.pipe(
    switchMap((params: ParamMap) =>
      this.service.getHero(params.get('id')))
  );
```
2. Snapshot快照
```
let id = this.route.snapshot.paramMap.get('id');
```

因此采用1比较好。 2是1的子集

## 5.4 动画
1. 导入BrowserAnimationsModule
2. 每个路由定义添加data对象，例如

```
const heroesRoutes: Routes = [
  { path: 'heroes',  component: HeroListComponent, data: { animation: 'heroes' } },
  { path: 'hero/:id', component: HeroDetailComponent, data: { animation: 'hero' } }
];
```
3. 创建动画效果
4. 添加动画，例如

```
<div [@routeAnimation]="getAnimationData(routerOutlet)">
  <router-outlet #routerOutlet="outlet"></router-outlet>
</div>
```
5. 组件ts模板中定义函数，例如
```
  getAnimationData(outlet: RouterOutlet) {
    return outlet && outlet.activatedRouteData && outlet.activatedRouteData['animation'];
  }
```
## 5.5 子路由

```
const crisisCenterRoutes: Routes = [
  {
    path: 'crisis-center',
    component: CrisisCenterComponent,
    children: [
      {
        path: '',
        component: CrisisListComponent,
        children: [
          {
            path: ':id',
            component: CrisisDetailComponent
          },
          {
            path: '',
            component: CrisisCenterHomeComponent
          }
        ]
      }
    ]
  }
];
```

## 5.6 第二路由

```
{
  path: 'compose',
  component: ComposeMessageComponent,
  outlet: 'popup'
},
<a [routerLink]="[{ outlets: { popup: ['compose'] } }]">Contact</a>

closePopup() {
  // Providing a `null` value to the named outlet
  // clears the contents of the named outlet
  this.router.navigate([{ outlets: { popup: null }}]);
}
```

## 5.7 路由守卫
1. 保护路由
主要确定用户的权限，通过服务关于是否登陆的数据。

```
// 路由
const adminRoutes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [AuthGuard],
    children: [
      {
        path: '',
        children: [
          { path: 'crises', component: ManageCrisesComponent },
          { path: 'heroes', component: ManageHeroesComponent },
          { path: '', component: AdminDashboardComponent }
        ],
      }
    ]
  }
];

AdminComponent
<router-outlet></router-outlet>

AuthGuard
@Injectable({
  providedIn: 'root',
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): boolean {
    let url: string = state.url;

    return this.checkLogin(url);
  }

  checkLogin(url: string): boolean {
    if (this.authService.isLoggedIn) { return true; }

    // Store the attempted URL for redirecting
    this.authService.redirectUrl = url;

    // Navigate to the login page with extras
    this.router.navigate(['/login']);
    return false;
  }
}
```
2. 保护子路由

```
 canActivateChild: [AuthGuard]
```
3. 处理未保存的数据
通过对话框的形式询问用户
4. 预先获取数据
想获取数据，再渲染视图
5. 查询参数及片段
跳转后返回的锚点
## 5.8 异步路由
1. 惰性加载

```
{
  path: 'admin',
  loadChildren: './admin/admin.module#AdminModule',
},
```
2. 保护特性模块
```
{
  path: 'admin',
  loadChildren: './admin/admin.module#AdminModule',
  canLoad: [AuthGuard]
},
```
3. 预加载

```
RouterModule.forRoot(
  appRoutes,
  {
    enableTracing: true, // <-- debugging purposes only
    preloadingStrategy: PreloadAllModules
  }
)
```