# 自定义组件
在angular中，我们可以通过component装饰器和自定义组件来创建自定义组件

## 基础知识
### 定义组件的元信息
   在angular中，我们可以使用component装饰器来定义组件的元信息
```js
    @Component ({
        selector:'my-app',//用于定义组件在HTML代码中匹配的标签
        template:`<h1>Hello {{name}}</h1>` //定义组件内嵌试图
    })
```
定义组件类
```js
    export class AppComponent {
        name ='Muzi'
    }
```
定义数据接口
在TypeScript中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对【对象的形状（shape）】进行描述。
```js
 interface Person{
     name:string,
     age:number
 }
 let Muzi:Person = {
     name:'Muzi', 
     age:28
 }
```
### 自定义组件实例

创建UserComponent组件

```js
    import {UserComponenet} from './user.componenet';

    @NgModule ({
        imports : [BrowserModule],
        declearation : [AppComponent,UserComponent],
        bootstrap : [AppComponent]
    })
    
    export class AppModule {}
```

使用UserComponent 组件
```js
    import {UserComponent} from './user.componenet';

    @NgModule({
        imports :[BrowserModule],
        decleartions:[AppComponent,UserComponenet],
        bootstrap:[AppComponent]
    })

    export class AppModule { }
```

使用构造函数初始化数据

```js
    @Component ({...})

    export class UserComponent {
        name:string;
        address:any;

        constructor(){
            this.name = 'Muzi';
            this.address = {
                province:'福建'
                city:'建阳'
            }
        }
    }
```

## 接口使用示例

定义Adress 接口

```js
    interface Address {
        provice : string;
        city : string;
    }
```

使用Address接口

```js
    export class UserComponent{
        name : string;
        address:Address;
    }
```