# 基础知识
## 导入路由模块
```TS
    import {RouterModule} from '@angular/router';

    @NgModule({
        imports:[BrowserModule,FromsModule,RouterModule],
        declerations:[AppComponent,userComponent,MembersComponent],
        bootstrap:[AppComponent]
    })

    export class AppModule {}
```

## 配置路由信息
```TS
    import {Routes,RouterModule} from '@angular/router';
    import {UserComponent} from './user.component';

    export const ROUTES:routes = [
        {path:'user',componenet:UserComponent}
    ];

    @NgModule({
        imports:[
            BrowserModule,
            RouterModule.forRoot(ROUTES)
        ],
        //....
    })

    export class AppModule{}
```

## routerLink 指令
    为了让我们链接大搜已设置的路由，我们需要使用routerLink指令，具体示例如下
```TS
    <nav>
        <a routerLink="/">首页</a>
        <a routerLink="/user">我的</a>
    </nav>
```