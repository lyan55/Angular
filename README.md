
## 目录
   
   官方文档[传送门](https://angular.cn/docs)
   
   - <a href="#0">安装</a>
   - <a href="#1">基础语法</a>
   - <a href="#2">组件传值(通信)</a>
   - <a href="#3">路由</a>
   - <a href="#4">网络请求</a>
   - <a href="#5">UI组件库</a>
   
   
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
  
      1. 循环 
      ```javascript
      <any *ngFor="let tmp of myList"></any>
      <any *ngFor="let tmp of myList;let myIndex=index"></any>
      ```  
      2. 条件--switch  
      
      ```javascript  
      <div [ngSwitch]="expression">
        <p *ngSwitchCase="情况1"></p>
        <p *ngSwitchCase="情况2"></p>
        <p *ngSwitchCase="情况3"></p>
        <p *ngSwitchDefault></p>
      </div>
      ```  
      2.1 条件--if  

      `<any *ngIf="expression"></any>`  
      
      3.绑定  
        3.1  属性绑定  
        `<any [someProp]="expression"><any>`  
        3.2  事件绑定  
        `<button (click)="handleClick()"></button>`  
        3.3  双向数据绑定  
        配置:   
       ```javascript   
        import {FormsModule} from '@angular/forms'  import {FormsModule} from '@angular/forms'  
      ```  
      `[(ngModel)]="value"`  
  - 管道(pipe)  
    官方:  
      1. number  
      2. slice  
      3. json  
      4. uppercase/lowercase  
      5. percent  
      6. currency  
  - 服务  
    本质: 就是一个类,在里面封装方法,数据  
    作用: 将不同组件要共享的数据和方法抽象出来  
    创建服务:  
    `ng g service heart-beat`  
    实现服务:  定义需要的数据和方法  
    调用服务:  组件是服务最大消费者  
    ```javascript 
    import { HeartBeatService } from "/path"          //引入服务类
    constructor(private myService: HeartBeatService)  //实例化HeartBeatService类
    this.myService.thods()                            //调用实例类对象的方法
    ```

 ## <a name="2">组件传值(通信)</a>  
  `import { Input,Output,EventEmitter } from '@angular/core' `   
  ### 组件内部通信 
  - 父传子--通过属性传值:  
  传:  `<son myTitle="123"></son>`  
  收:  
  ```javascript
  import {Input} from '@angular/core'     //导入模块
  @input() myTitle = ""                   //保存变量值
  this.myTitle(.ts)/{{ myTitle }}(.html)  //调用值
  ```  
  
  - 子传父--通过绑定、触发事件传值:  
  事件绑定:   
  html:  
  `<son (myEvent)="handleEvent($event)"></son>`  
  .ts  
  `handleEvent(opts) {opts}`  
  事件触发:  
  ```javascript  
  import {Output,EventEmitter} from '@angular/core'   //导入模块  
  @Output() myEvent = new EventEmitter()              //抛出事件  
  this.myEvent.emit(opts)                             //发送参数  
  ```

 ## <a name="3">路由</a>  
  `import { Router,ActivatedRoute,canActivate } from "@angular/router"`  
  路由:  处理url与组件的关系  
  - 基本用法    
    ```javascript  
      1.完成一个自定义模块(包含路由配置)的创建和调用
        ng g module my-module --routing
        app.module.ts/引入/imports

      2.准备一个容器
        <router-outlet></router-outlet>
        forChild-->forRoot
      3.创建组件
        Login/Register/Cart...
          ng g component demo13-login
          ng g component demo13-register
      4.配置路由词典
          {path:'',component:**}
          引入组件
          各自指定url地址
      ```  
  - 跳转与传参  
    #### 跳转(Router):  
    编程方式:  
   ```javascript  
     import {Router} from '@angular/router'    //引入Router模块
	 constructor(private myRouter:Router){}    //实例化Router模块
	 this.myRouter.navigateByUrl("/path")      //调用该类对象navigateByUrl()方法跳转
  ```  
  标签方式:  routerLink  
    `<button routerLink="/register">注册</button>`   
  #### 传参(ActivatedRoute):   
  1.配置接收方的路由地址  
       `detail --> detail/:id`  
  2.1  静态传参  
  ```javascript
  this.myRouter.navigateByUrl('detail/3')  //编程方式
  <any routerLink="detail/3"></any>        //标签方式
  ```
  2.2  动态传参  
  ```javascript
  routerLink="/path/{{opts}}"       //推荐
  [routerLink]="'/path/'+opts"      //使用绑定属性指令
  ```
  3. 接收  
  ```javascript
    import {ActivatedRoute} from '@angular/router'    //导入
    constructor(private myRoute:ActivatedRoute){}     //ActivatedRoute实例化
    this.myRoute.params.subscribe((result)=>{         //调用类对象属性,方法.回调获取传递过来的参数
      return result.id       //返回参数id的值,路由地址设置detail/:id 和 接收参数result.id 两个属性名字要保持一致
    })
  ```

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

## <a name="4">网络请求</a>  
   `import {HttpClient} from '@angular/common/http'`   
   [传送门](https://angular.cn/tutorial/toh-pt6)

## <a name="5">UI组件库</a> 
  - 
