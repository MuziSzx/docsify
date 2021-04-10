# 差值表达式
    在Angular中，我们可以使用{{}}差值表达式来实现数据绑定
## 绑定普通文本
    import {Component} form '@angular/core'

    @Component({
        selector:'my-app',
        template:`<h1>Hello {{name}} </h1>`,
    })

    export class AppComponent {
        name="Angular"
    }