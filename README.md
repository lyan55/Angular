
## 目录
   
   官方文档[传送门](https://angular.cn/docs)
   
   - <a href="#0">安装</a>
   - <a href="#1">基础语法</a>
   - <a href="#2">通信</a>
   - <a href="#3">路由</a>
   - <a href="#4">ionic</a>
   
   
 ## <a name="0">搭建开发环境</a> 
 
  ```javascript  
  npm install -g @angular/cli  //安装anglugar命令行界面
  ng new my-app
  cd my-app
  ng serve --open              //启动服务
  ```
    
 ## <a name="1">基础语法</a>  
- 组件的创建  
  `ng g component demo01`  
  `<app-demo01></app-demo01>`  
- 双花括号  
  `<any>{{expression}}</any>`  
- 指令  
1. *ngFor  
```javascript
<any *ngFor="let tmp of myList"></any>
<any *ngFor="let tmp of myList;let myIndex=index"></any>
```  
2. *ngSwitch  
```javascript  
<div [ngSwitch]="expression">
  <p *ngSwitchCase="情况1"></p>
  <p *ngSwitchCase="情况2"></p>
  <p *ngSwitchCase="情况3"></p>
  <p *ngSwitchDefault></p>
</div>
```  
2. *ngIf  
`<any *ngIf="expression"></any>`  
  
- 数据绑定  
  1. 属性绑定  
  `<any [someProp]="expression"><any>`  
  3. 2事件绑定  
  `<button (click)="handleClick()"></button>`  
  3. 3双向数据绑定  
  配置:   
```javascript   
/*src/app/app.module.ts*/
import {FormsModule} from '@angular/forms'
@NgModule({
  imports:[FormsModule]
})  
```  
调用(*.component.html):  
`[(ngModel)]="value"`  

- 管道(pipe)  
what?  
筛选、过滤、格式化 本质：有参数有返回值的方法.  
### 官方管道:  
  1. number  
  2. slice  
  3. json  
  4. uppercase/lowercase  
  5. percent  
  6. currency  

实例:  
```javascript
<p>{{ 2000 | number }}</p>
<p>The hero's birthday is {{ birthday | date }}</p>
<p>{{ 'hellosmile' | uppercase }}</p>
<p>{{ 'HELLOSMILE' | lowercase }}</p>
<p>{{ "angular is cool" | slice:1 }}</p>
```  
output: 
```javascript
2,000

The hero's birthday is May 25, 2019

HELLOSMILE

hellosmile

ngular is cool
```  
### 自定义管道:  
`ng g pipe pipeName`  
实例(判断性别):  
```javascipt
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'sex'
})
export class SexPipe implements PipeTransform {

  transform(value: number): any {
    if(value == 0) {
      return '你是女孩'
    }else if(value == 1) {
      return '你是男孩'
    }
  }
}
```  
调用(component.html):  
`<p>{{ 0 | sex }}</p>`  
output:  '你是女孩'

- 服务  
what?  
本质就是一个类,在里面封装方法,数据  
why?  
作用: 将不同组件要共享的数据和方法抽象出来  
创建服务:  
`ng g service heart-beat`  
实现服务:  定义需要的数据和方法  
调用服务:  组件是服务最大消费者  
调用(*.component.html):  
```javascript  
import { HeartBeatService } from "/path"          //引入服务类
constructor(private myService: HeartBeatService)  //实例化HeartBeatService类
this.myService.methods()                          //调用实例类对象的方法
```

 ## <a name="2">通信(Input,Output,EventEmitter)</a>  
### 组件间通信 
- 父传子--通过属性传值:  
传:  `<son myTitle="123"></son>`  
收:  
```javascript
import {Input} from '@angular/core'     //导入模块
@input() myTitle = ""                   //保存变量值
this.myTitle(.ts)/{{ myTitle }}(.html)  //调用值
```  

- 子传父--通过事件传值:  
### 父组件  
父组件(*.component.html):  
`<son (myEvent)="handleEvent($event)"></son>`  
父组件(*.ts):    
`handleEvent(opts) {opts}`  
### 子组件  
子组件(*.component.html):  
`<button (click)="sendData()">clickMes</button>`  
子组件(*.ts):  
```javascript  
import {Output,EventEmitter} from '@angular/core'   //导入模块
@Output() myEvent = new EventEmitter()              //实例化  
sendData() {
  this.myEvent.emit(opts)                           //向父组件发送参数 
}
```  

### 与远程服务器端的通信或网络通信(HttpClientModule,HttpClient)  
  步骤1: app.module.ts下引入HttpClientModule:  
  ```javascript
  import {HttpClientModule} from '@angular/common/http'  
  imports:[HttpClientModule]      //使 HttpClient 在应用中随处可用
  ```  

   步骤2: 在组件中调用HttpClient发起请求  
   ```javascript
    import {HttpClient} from '@angular/common/http'
    constructor(private myClient:HttpClient){}
    this.myClient.get/post().subscribe((result)=>{
      //result就是从服务器端请求到的数据
      //将result中数据保存，到视图中显示或者在组件类中做处理。。。
    })
  ```  
   HTTP[传送门](https://angular.cn/tutorial/toh-pt6)

 ## <a name="3">路由(Router,ActivatedRoute,canActivate)</a>  
  
  - 配置    
    ```javascript  
      1.完成一个自定义模块(包含路由配置)的创建和调用
        ng g module my-module --routing
        将自定义路由模块(MyModuleRoutingModule)导入到app.module.ts中  
      2.准备一个容器(父组件或根组件中)
        <router-outlet></router-outlet>
        forChild-->forRoot
      3.创建组件
        Login/Register/Cart...
          ng g component demo13-login
          ng g component demo13-register
      4.配置路由词典
          {path:'',component:**}    //注意：path没有'/'!!!
      ```  
  - 跳转(Router):  
  1. 编程方式:  
   ```javascript  
  import {Router} from '@angular/router'      //引入Router模块
  constructor(private myRouter:Router){}      //实例化Router模块
  this.myRouter.navigateByUrl("/path")        //调用该类对象navigateByUrl()方法跳转
  ```  
  2. 标签方式(routerLink):   
    `<button routerLink="/path">跳转路由</button>`   

- 传参(ActivatedRoute):   

1. 配置接收方的路由地址  
       `detail --> detail/:id`  
2. 静态传参  
```javascript
<any routerLink="detail/3"></any>        //标签方式
this.myRouter.navigateByUrl('detail/3')  //编程方式
```
3. 动态传参  
```javascript
//标签方式
<any routerLink="/path/{{opts}}"> 路由传参</any>      //推荐大胡子括号
<any [routerLink]="'/path/'+opts">路由传参</any>      //使用绑定属性指令

//编程方式
let params;
this.myRouter.navigateByUrl('detail/' + this.params);
this.myRouter.navigateByUrl(`detail/${this.params}`);
```  

4. 接收参数  

```javascript
  import { ActivatedRoute } from '@angular/router'    //导入ActivatedRoute模块
  constructor(private myRoute:ActivatedRoute){}       //ActivatedRoute实例化
  this.myRoute.params.subscribe((result)=>{           //异步回调接收参数
    return result.id                                  //路由地址设置detail/:id和接收参数result.id两个属性名保持一致
  })
```  

- 路由的嵌套  
what?  
一个spa，A组件需要嵌套B组件、C组件  
how?  
1. 给A组件指定一个容器
`<router-outlet></router-outlet>`   
2. 给A组件的路由对象，添加一个children   
```
{
  path:'a',
  component:A,
  children:[
    { path:'b',component:B },
    ...
  ]
}
```  
调用:  
`http://localhost:4200/a/b`  

- 路由守卫(canActivate)  
主要思路: 判断该页面(比如后台管理页面)是否显示  
操作步骤:  
1. 创建一个服务类,封装判断处理逻辑方法,数据  
`ng g service my-guard`  
2. 封装判断处理逻辑方法,数据  

```javascript
import {CanActivate} from '@angular/router'
    export class MyGuardService implements CanActivate{
  canActivate(){
    //在此来做一些功能处理 来决定是否可以访问你要保护的页面
    return true/false
  }
}
```  
3. 配置路由地址页面(my-module.ts)--皮卡丘,去守卫你保护的页面吧  
```javascript  
    import {MyGuardService} from '../my-guard.service'
    [
      {
        path:'admin',
        component:AdminComponent,
        canActivate:[MyGuardService]
      }
    ]
```   



## <a name="4">ionic</a>  
概述:  
1.ionic 基于Angular语法，简单易学。    
2.ionic 是一个轻量级框架。  
3.ionic 完美的融合下一代移动框架，支持Angularjs性，MVC，代码易维护。  
4.ionic 提供了漂亮的设计，通过SASS构建应用程序，供了很多UI 组件来帮助开发者开发强大的应用。  
5.ionic 专注原生，让你看不出混合应用和原生的区别  
6.ionic 提供了强大的命令行工具。  
7.ionic 性能优越，运行速度快。 
### what?  

onic是一个移动端的ui组件库，主要是给Angular进行使用.  
 Ionic = angular+ IonicModule(集成了很多的好看的ui组件)+ionIcons + cordova  

### why?  

1. ionic基于angular  
2. 强大的cli支持  
3. ionic内置了丰富的移动端的ui组件库,在不同的操作系统运行时会遵循对应的系统风格（android-->MaterialDesign iOS->扁平化）  
4. 维护了自己的一套图标体系  
5. ionic提供组件  
6. ionic提供类  
7. ionic提供导航工具  

### how?  
```
npm i -g ionic      //安装ionic模块
ionic start myApp   //新建一个项目
ionic serve         //启动服务
```  

常用组件如下:  
1.sheet  
2.alert  
3.button  
4.card  
5.content  
6.grid  
7.infinite scroll  
8.input  
9.slides  
10.toast  
11.tabs



官方文档[传送门](https://ionicframework.com/docs/components)   