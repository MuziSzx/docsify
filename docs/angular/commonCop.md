# 常用指令简介

在Angular实际项目中，最常用的指令是ngIf和ngFor 指令。

## 基础知识


### ngIf指令简介

该指令用于根据表达式的值，动态控制模板内容的显示与隐藏。它与Angular1.x中的ng-if指令功能是等价的。

ngIf指令语法

```HTML
    <div *ngIf='condition'></div>
```

### ngFor 指令简介

该指令用于基于可迭代对象中的每一项创建相应的模板。它与Angular1.x中的ng-repeat指令的功能是等价的。

ngFor指令语法

```HTML

    <li *ngFor='let item of items'>...</li>

```

ngIf 和 NgFor 指令使用示例

```Ts
    import {Compoenent} from '@angular/core';

    interface Address {
        provice: string;
        city :string;
    }

    @Componenet ({
        selector:'sl-user',
        template:`
        <h2>大家好，我是{{name}}</h2>
        <p>我来自{{address.province}}省 - {{address.city}}市
        </p>
        <div #ngIf='showSkills'>
            <h3>我的技能</h3>
            <ul>
                <li *ngFor='let skill of skills'>
                    {{skill}}
                </li>
            </ul>
        <div>
        `

        export class Usercomponent {
            name:stirng;
            address:Address;
            showSkills:boolean;
            skills:string[];
        }

        constructor(){
            this.name = 'Muzi';
            this.address = {
                province:'福建',
                city:'建阳’
            }
            this.showSkills = true;
            this.skills= ['1','2','3']

        }
    })
```