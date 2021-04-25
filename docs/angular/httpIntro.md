# Angular Http Client 

## HttpClientModule 应用

导入新的HTTP Module

```TS
    import {HttpClientModule} from '@angular/common/http';

    @NgModule({
        declarations:[AppComponent],
        imports:[BrowserModule,HttpClientModule],
        providers:[],
        bootsrtap:[AppComponent]
    })

    export class AppModule {}
```

需要注意的是，现在JSON是默认的数据格式，我们不需要再进行显示的解析。即我们不需要再使用以下代码：
```TS
    http.get(URL).map(res => res.json()).subscribe(...)
```

现在我们可以这样写:
```TS
    http.get(url).subscribe(...)
```

发送Get请求

```ts
    import {Component,OnInit} from '@angular/core';
    import {Obsrevable} from 'rxjs/Observable';
    import {HttpClient} from '@angular/common/http';
    import * as _ from 'loadsh';

    interface Course {
        description : string;
        couserListIcon : string;
        iconUrl : string;
        longDescription : string;
        url : string;
    }

    @Component({
        selector:'app-root',
        template:`
              <ul *ngIf="courses$ | async as courses else noData">
                <li *ngFor="let course of courses">
                    {{course.description}}
                </li> 
             </ul>
             <ng-template #noData>No Data Available</ng-template>
        `

        export class AppComponent implemenets OnInit {
            courses$:Observable<any>;
            construcor(private http:HttpClient){}

            ngOnInit(){
                this.courses$ = this.http.get('https://angular-http-guide.firebaseio.com/courses.json').map(data => _.values(data)).do(console.log());
            }
        }
    })
```

设置查询参数

假设发送GET请求时，需要设置对应的查询参数，预期的URL地址如下
``` http
    https://angular-http-guide.firebaseio.com/courses.json?orderBy="$key"&limitToFirst=1
```

创建HttpParams 对象
```TS
 import {HttpParams} from "@angular/common/http" ;

 const params = new HttpParams().set('orderBy','$key').set('limitToFirst','1);

 this.courses$ = this.http
    .get("/courses.json", {params})
    .do(console.log)
    .map(data => _.values(data))
 ```

 需要注意的是，我们通过链式语法调用set()方法，构建HttpParams对象。这是因为HttpParams对象。这是因为HttpParams对象是不可变的，通过
 set（）方法可以防止该对象被修改。

 每当调用set()方法，将会返回包含新值得HttpParams对象，因此如果使用下面的方式，将不能正确的设置参数。
 ```TS
    const params = new HttpParams();

    params.set('orderBy','"$key"');
    params.set('limitToFirst',"1");
 ```

 使用fromString 语法
 ```ts
    const params = new HttpParams({fromString: 'orderBy="$key"&limitToFirst=1'});
 ```

使用request（） API
```TS
    const params = new HttpParams({fromString:'orderBy = "$key"&limitToFirst=1'});

    this.courses$ = this.http.request("GET","/courses.json",{
        responseType:"json",
        params
    })
    .do(console.log)
    .map(data => _.values(data));
```
 
 设置HTTP Headers
 ```TS
    const headers = new HttpHeaders().set("X-CustomHeader","custom header value");

    this.courses$ = this.http.get("/courses.json",{headers}).do(console.log).map(data => _.values(data));
 ```

 发送Put请求 

 ```TS
    httpPutExample(){
        const headers = new HttpHeaders().set("Content-Type","application/json");

        this.http.put("/courses/-KgVwECOnlc-LHb_B0cQ.json"),
        {
            "courseListIcon": ".../main-page-logo-small-hat.png",
            "description": "Angular Tutorial For Beginners TEST",
            "iconUrl": ".../angular2-for-beginners.jpg",
            "longDescription": "...",
            "url": "new-value-for-url"
        },
        {headers})
        .subscribe(
            val => {
                console.log("PUT call successful value returned in body",val);
            },
            response => {
                console.log("PUT call in error",responese);
            },
            () => {
                console.log("The PUT observable is now completed.")
            }
        )
    }
 ```