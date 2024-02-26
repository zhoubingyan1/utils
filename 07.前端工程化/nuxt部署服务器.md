https://www.cnblogs.com/huangxiaotao/p/10305329.html
### 5、项目本地完成后， npm run build 打包应用

将打包文件、Nuxt配置、静态文件和所需依赖配置文件上传至服务器
```
    //打包完成后将以下四个文件上传到服务器空间
    .nuxt     //打包生成的文件夹
    static    //默认静态文件
    nuxt.config.js   //nuxt项目配置文件
    package.json   //配置文件
    package-lock.json

    //也可以将以上文件 重新放一个文件夹 然后
    

```
### 6、在服务器上部署运行
1. 使用命令模式：终端或者其他方式链接上服务器
2. 自己先测试一下服务器上是否安装支node npm,检查一下版本号 
    node -v
    npm -v
2. 进入到该项目的根目录
3. 运行 npm install 安装package里的依赖
4. 安装好3以后，再运行 npm start 就可以运行起来nuxt的服务渲染了