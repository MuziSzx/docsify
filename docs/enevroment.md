# Angular 环境搭建
## 基础要求
    Node.js 
    git
## Angular 开发环境配置方式
    · 基于Angular Quickstart
        https://github.com/angular/quickstart
    · 基于Angular Cli
        npm install -g @angular/cli
## 配置开发环境
### 基于Angular Quickstart 项目
     1、使用git克隆quickstart项目
        git clone https://github.com/angular/quickstart ng4-quickstart
     2、使用IDE打开已新建的项目
        code ./ng4-quickstart'
     3、安装项目所需依赖
        npm install
     4、验证环境是否搭建成功
        npm start 
### 基于Angular CLI
    1、安装Angular CLI
       npm install -g @angular/cli
    2、检测Angular CLI是否安装成功
       ng -v
    3、创建新的项目 
       ng new PROJECT -NAME
    4、启动本地服务器
       cd PROJECT -NAME
       ng serve
    