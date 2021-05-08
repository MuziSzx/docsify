# 基础知识

组件中注入服务步骤

(1)配置已创建的服务，如：
```TS
    @NgModule({
        //...
        providers:[MemberService]
    })

    export class AppModule{}
```

(2) 导入已创建的服务，如：
```TS
    import {MemberService} from '../member.service';
```

(3) 使用构造注入方式，注入服务
```TS
    export class MembersComponent implements Oninit {
        constructor(private memberService:MemberService){}
    }
```

服务使用示例

创建MemberService服务

```TS
    import {Injectable} from '@angular/core';
    import {Http} from '@angular/http';

    @Injectable()

    export class MemberService {
        constructor(private http:Http){}

        getMembers(){
            return this.http
            .get(`https://api.github.com/orgs/angular/members?page=1&per_page=5`)
            .map(res => res.json())
        }
    }
```

配置MemberService 服务

```TS
    import {MemberService} from "./member.service" ;

    @NgModule ({
        providers:[MemberService],
        bootstrap:[AppComponenet]
    })

    export class AppModule {}
```