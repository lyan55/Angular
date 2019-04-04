
## 目录
   
   中文官方文档[传送门](https://angular.cn/docs)
   
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
        import {FormsModule} from '@angular/forms' import {FormsModule} from '@angular/forms' 
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

 ## <a name="2">组件传值(通信)</a>  
  `import {FormsModule} from '@angular/forms`  
  `import {Input} from '@angular/core' `
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
  `import { Router } from "@angular/router"`  
  路由:  处理url与组件的关系  
  - 路由配置  
  导入router模块:  
    `import { Router } from "@angular/router"`  
    实例化:  
    `constructor(private myRoutr: Router) { }
`  
调用:  
    `this.myRoutr.navigateByUrl("routerPath")`  
  - 路由传参  
    1.静态传参  
      1.1   `routerLink="/path/10"`  
      1.2   需要配置{ ActivatedRoute }模块  
      `import { ActivatedRoute } from "@angular/router"`  
      `this.myRoute.params.subscribe`  
    2.动态传参  
      2.1   `routerLink="/path/{{opts}}"`  
      2.2   `[routerLink]="'/path/'+opts"`  
  - 路由跳转  
  - 路由导航  
  - 路由守卫

## <a name="4">网络请求</a>  
   `import {HttpClient} from '@angular/common/http'`   
   [传送门](https://angular.cn/tutorial/toh-pt6)

## <a name="5">UI组件库</a> 
  - 
