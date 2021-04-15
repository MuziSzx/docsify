# 差值表达式
    在Angular中，我们可以使用{{}}差值表达式来实现数据绑定
## 绑定普通文本
``` js
    import {Component} form '@angular/core'

    @Component({
        selector:'my-app',
        template:`<h1>Hello {{name}} </h1>`,
    })

    export class AppComponent {
        name="Angular"
    }
```
## 绑定对象属性
``` js
    import {Component} from '@angular/core'

    @Component ({
        selector :'mu-app'
        template:`
        <h2>大家好，我是{{name}}</h2>
        <p>我来自{{address.province}}省</p>
        {{address.city}}市
        `
    })

    export class AppComponent {
        name ='Muzi';
        address ={
            provice:'福建',
            city:'建阳'
        }
    }
```
值得一提的是，我们可以使用Angular内置的json管道，来显示对象的信息：
```js
    @Component ({
        selector:'my-app',
        template:`
        ...
        <p>{{address | json}}</p>
        `,
    })
    export class AppComponent {
        name ='Muzi';
        address = {
            provice:'福建',
            city:'建阳'
        }
    }
```