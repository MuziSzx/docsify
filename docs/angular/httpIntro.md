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
 put请求和post请求的区别
    PUT请求：如果两个请求相同，后一个请求会把第一个请求覆盖掉。（所以PUT用来改资源）
    Post请求：后一个请求不会把第一个请求覆盖掉。（所以Post用来增资源）

发送Patch 请求
```TS
    httpPatchExample(){
        this.http.patch("/courses/-KgVwECOnlc-LHb_B0cQ.json"),
        {
            "description": "Angular Tutorial For Beginners PATCH TEST",
        })
        .subscribe(
            (val) => {
                console.log("PATCH call successful value returned in body", 
                  val)
            }
        ),
        response => {
                console.log("PATCH call in error", response);
            },
            () => {
                console.log("The PATCH observable is now completed.");
            });
    }
```

发送Delete请求
```TS
    httpDeleteExample(){
       this.http.delete("/courses/-KgVwECOnlc-LHb_B0cQ.json")
        .subscribe(
            (val) => {
                console.log("DELETE call successful value returned in body", 
                  val);
            },
            response => {
                console.log("DELETE call in error", response);
            },
            () => {
                console.log("The DELETE observable is now completed.");
            });
    }
```

发送POST请求
```TS
    httpPostExample() {
    this.http.post("/courses/-KgVwECOnlc-LHb_B0cQ.json",
        {
            "courseListIcon": "...",
            "description": "TEST",
            "iconUrl": "..",
            "longDescription": "...",
            "url": "new-url"
        })
        .subscribe(
            (val) => {
                console.log("POST call successful value returned in body", 
                  val);
            },
            response => {
                console.log("POST call in error", response);
            },
            () => {
                console.log("The POST observable is now completed.");
            });
}
```

避免重复请求
```TS
    duplicateRequestsExample() {
    const httpGet$ = this.http
        .get("/courses.json")
        .map(data => _.values(data));

    httpGet$.subscribe(
        (val) => console.log("logging GET value", val)
    );

    this.courses$ = httpGet$;
}
```
在上面例子中，我们正在创建了一个 HTTP observable 对象 httpGet$，接着我们直接订阅该对象。然后，我们把 httpGet$ 对象赋值给 courses$ 成员变量，最后在模板中使用 async 管道订阅该对象。

这将导致发送两个 HTTP 请求，在这种情况下，请求显然是重复的，因为我们只希望从后端查询一次数据。为了避免发送冗余的请求，我们可以使用 RxJS 提供的 shareReplay 操作符：
```TS
  // put this next to the other RxJs operator imports
    import 'rxjs/add/operator/shareReplay';

    const httpGet$ = this.http
        .get("/courses.json")
        .map(data => _.values(data))
        .shareReplay();  
```

并行发送多个请求
 
并行发送HTTP请求的一种方法是使用RXjs中的forkjoin操作符
```TS
    import 'rxjs/add/observabel/forkJoin';

    parallelRequests(){

        const parallel$ = Observable.forkJoin(
            this.http.get('/courses/-KgVwEBq5wbFnjj7O8Fp.json'),
            this.http.get('/courses/-KgVwECOnlc-LHb_B0cQ.json')
        )

        parallel$.subscribe(values =>{
            console.log("all values",values)
        })
    }
```

顺序发送Http请求

```TS
    sequentialRequests() {
        const sequence$ = this.http.get<Course>('/courses/-KgVwEBq5wbFnjj7O8Fp.json')
            .switchMap(course => {
                course.description+= ' - TEST ';
                return this.http.put('/courses/-KgVwEBq5wbFnjj7O8Fp.json', course)
            });
            
        sequence$.subscribe();
    }
```

获取顺序发送Http请求的结果

```TS
    sequentialRequests() {
        const sequence$ = this.http.get<Course>('/courses/-KgVwEBq5wbFnjj7O8Fp.json')
            .switchMap(course => {
                course.description+= ' - TEST ';
                return this.http.put('/courses/-KgVwEBq5wbFnjj7O8Fp.json', course)
            },
                (firstHTTPResult, secondHTTPResult)  => [firstHTTPResult, secondHTTPResult]);

        sequence$.subscribe(values => console.log("result observable ", values) );
    }
```

请求异常处理
```TS
    throwError() {
        this.http
            .get("/api/simulate-error")
            .catch( error => {
                // here we can show an error message to the user,
                // for example via a service
                console.error("error catched", error);

                return Observable.of({description: "Error Value Emitted"});
            })
            .subscribe(
                val => console.log('Value emitted successfully', val),
                error => {
                    console.error("This line is never called ",error);
                },
                () => console.log("HTTP Observable completed...")
            );
    }
```

当发生异常时，控制台的输出结果：

```js
    Error catched 

    HttpErrorResponse {headers: HttpHeaders, status: 404, statusText: "Not Found", url: "http://localhost:4200/api/simulate-error", ok: false, … }

    Value emitted successfully {description: "Error Value Emitted"}
    HTTP Observable completed...
```

HTTP 拦截器

定义拦截器
```TS
    import {Injectable} from "@angular/core";
    import {HttpEvent, HttpHandler, HttpInterceptor} from "@angular/common/http";
    import {HttpRequest} from "@angular/common/http";
    import {Observable} from "rxjs/Observable";

    @Injectable()
    export class AuthInterceptor implements HttpInterceptor {
        
        constructor(private authService: AuthService) {
        }

        intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
            const clonedRequest = req.clone({
                headers: req.headers.set('X-CustomAuthHeader', authService.getToken())
            });
            console.log("new headers", clonedRequest.headers.keys());
            return next.handle(clonedRequest);
        }
    }
```

配置拦截器

```TS
    @NgModule({
        declarations: [
            AppComponent
        ],
        imports: [
            BrowserModule,
            HttpClientModule
        ],
        providers: [
            [ { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true } ]
        ],
        bootstrap: [AppComponent]
    })
    export class AppModule { }
```

HTTP进度事件

```TS
    longRequest() {
        const request = new HttpRequest(
            "POST", "/api/test-request", {}, 
            {reportProgress: true});

        this.http.request(request)
            .subscribe(
                event => {
                    if (event.type === HttpEventType.DownloadProgress) {
                        console.log("Download progress event", event);
                    }
                    if (event.type === HttpEventType.UploadProgress) {
                        console.log("Upload progress event", event);
                    }
                    if (event.type === HttpEventType.Response) {
                        console.log("response received...", event.body);
                    }
                }
            );
    }
```
上面示例运行后，控制台的可能的输出结果：

```TS
    Upload progress event Object {type: 1, loaded: 2, total: 2}
    Download progress event Object {type: 3, loaded: 31, total: 31}
    Response Received... Object {description: "POST Response"}
```
