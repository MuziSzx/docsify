# 表单模块简介

Angular中有两种表单
    Template Driven Forms - 模板驱动式表单
    Reactive Forms - 响应式表单

## 基础知识

导入表单模块
```TS
    import {FormsModule} from '@angular/forms';

    @NgModule({
        imports:[BrowserModule,FormsModule],
        declerations:[AppComponent,UserComponenet],
        bootstrap:[AppComponent]
    })

    export class AppModule{ }
```

模板变量语法

```HTML
    <video #player></video>
    <button (click)='player.pause()'></button>
```
等价于
```HTML
<video ref-player></video>
```

表单使用示例
```TS
@Component({
    selector:'Muzi',
    template:
    `
        <div *ngIf="showSkills">
            <h3>我的技能</h3>
            ...
            <from (submit)="addSkill(skill.value)">
                <label>添加技能</label>
                <input type='text' #skill>
            </from>
        </div>
    `
    export class UserComponent {
   // ...
    addSkill(skill: string) {
        let skillStr = skill.trim();
        if (this.skills.indexOf(skillStr) === -1) {
            this.skills.push(skillStr);
        }
    }
}
})

```