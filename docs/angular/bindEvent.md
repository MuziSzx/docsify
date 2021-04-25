# 事件绑定

在Angular中，我们可以通过（eventName)的语法，实现事件绑定。

## 基础知识

事件绑定语法

```HTML
    <data-picker (dateChanged)='stateMent()'> </date-picker>

    等价于

    <date-picker on-dataChanged='stateMent()'></date-picker>

```
介绍完事件绑定的语法，接下来我们来为第五节中的 UserComponent 组件，开发一个功能，即可以让用户动态控制技能信息的显示与隐藏。

事件绑定示例

```TS
    @Component({
        selector:'Muzi',
        template:`
        ...
        <button (click)='toggleSkills()'>
            {{showSkills?"隐藏技能":"显示技能"}}
         </button>
         ...
        `
    })

    export class UserComponent {
        toggleSkills(){
            this.showSkills = !this.showSkills'
        }
    }
```