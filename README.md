# 操作数据(M),遥控视图(V)!!!

## 目录
   
   中文官方文档[传送门](https://angular.cn/docs)
   
   - <a href="#0">安装</a>
   - <a href="#1">基础语法</a>
   - <a href="#2">组件传值</a>
   - <a href="#3">路由</a>
   - <a href="#4">HTTP</a>
   - <a href="#5">常用模块</a>
   
   
 ## <a name="0">安装</a> 
 
   `npm i -g @angular/cli` 
    
 ## <a name="1">基础语法</a>  
  - 双花括号  
  - 指令  
  - 管道  
  - 服务  

 ## <a name="2">组件传值</a>  
  - 父传子  
  - 子传父  

 ## <a name="3">路由</a>  
  路由:  处理url与组件的关系  
  - 路由配置  
  导入router模块:  
    `import { ActivatedRoute } from "@angular/router"`  
    实例化:  
    `constructor(private myRoutr: Router) { }
`  
调用:  
    `this.myRoutr.navigateByUrl("routerPath")`  
  - 路由传参  
    1.静态传参  
      1.1  `routerLink="/path/10"`  
      1.2  需要配置{ ActivatedRoute }模块  
      `import { ActivatedRoute } from "@angular/router"`  
      `this.myRoute.params.subscribe`  
    2.动态传参  
      2.1  `routerLink="/path/{{opts}}"`  
      2.2  `[routerLink]="'/path/'+opts"`  
  - 路由跳转  
  - 路由导航  
  - 路由守卫

## <a name="4">HTTP</a>  
   [传送门](https://angular.cn/tutorial/toh-pt6)

## <a name="5">常用模块</a> 
  - 