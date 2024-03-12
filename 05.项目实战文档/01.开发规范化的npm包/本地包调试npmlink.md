 ## 引子
 再次碰到调试 npm 本地包的情况，一时想不起来，看了下文档，实际操作了一下，发现了一些文档上没明写的东西。
 
 ## 介绍
 根据文档介绍，包的链接分为两步。
 
 ### 第一步
 在一个包文件夹内执行 `npm link` 将在全局文件 **{prefix}/lib/node_modules/** 内，创建一个符号链接（symlink），这个链接指向 `npm link` 命令执行的地方。
 
 ### 第二步
 到其它目录下，执行 `npm link packageName` 命令，将会创建一个从全局安装的 **packageName** 到当前文件内的 **node_modules** 下的符号链接。
 
 需要注意的的是， **packageName** 是取自包的 **package.json** 中 **name** 字段，不是文件夹名称。
 
 包的名称可能有作用域前缀，如果有， **packageName** 也要对应加上。
 
 ## 实践
 基于文档，结合实际的操作，对照看下产生的效果。
 
 ### 运行环境
 * 项目是基于 webpack 简单配置，本地运行的 server 。
 * node 使用 nvm 管理的。
 * 项目已引用了包，需在基础上进行修改调试。
 * 系统是 macOS 。
 
 ### 操作
 首先在包根目录下面执行 `npm link` 命令，出现下面的提示：
 
 ![79-link](https://camo.githubusercontent.com/4e82cfcb34b9e7bbc689040987e38e0d2b337a300cfe6a00ec2d2c0a9700e7be/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37392f6e706d2d6c696e6b2e706e67)
 
 到对应的目录下，发现生成了提示中所说的文件，就是文档中所说**符号链接**（symlink）：
 
 ![79-result](https://camo.githubusercontent.com/ee21aaffa255948a83c664231166152b82d72b1d50102bc0792b3a71d63e1fab/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37392f6c696e6b2d726573756c742e706e67)
 
 试着改了一下本地的源文件，发现全局包里面对应的文件内容也跟着变化。
 
 然后到项目中执行 `npm link packageName` 指令，出现了下面的提示：
 
 ![79-link-package](https://camo.githubusercontent.com/9e1fe704dabecbcd8ee2140ed4a18849378b2a3e342a8a6cc93943f5019f2f4c/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37392f6c696e6b2d7061636b6167652e706e67)
 
 到 node_modules 下发现对应的依赖包已经发生了变化：
 
 ![79-link-module](https://camo.githubusercontent.com/5f3914823d18d3af41b1008d45c3eb3774300a53a150a3a67fdbee2a0d16ae1d/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37392f6c696e6b2d6d6f64756c652e706e67)
 
 这里的包跟全局那个生成的包是一样的，包更新了，本地看没什么效果，原因是本地的 sever 有缓存，需要重新启动一下。注意这个文件夹图标多了一个箭头的标记，未 `link` 之前没有这个。
 
 重启服务后，到源库修改源码，发现项目 **node_modules** 下同步了修改的内容，webpack 也检测到变化，自动刷新。
 
 修改好后，想要恢复到原本的包，解除 link 该怎么做？很奇怪，在 npm 官方文档上没找到说明。
 
 ### 解除 link
 到项目下执行下面的命令：
 
 ```
 npm unlink --no-save package && npm install
 ```
 
 [npm uninstall](https://docs.npmjs.com/cli-commands/uninstall.html) 文档中可以发现，`unlink` 其实是 `uninstall` 的别名，实质上也是删除了包。
 
 包不需要的 link 的时候，建议也解除，到包目录下执行下面的命令：
 
 ```
 npm unlink
 ```
 
 [Back to top ⬆️](#index)
 ## 参考资料
 * [npm-link](https://docs.npmjs.com/cli-commands/link.html)
 * [Understanding npm-link](https://medium.com/dailyjs/how-to-use-npm-link-7375b6219557)
 * [NPM Linking and Unlinking](https://dev.to/erinbush/npm-linking-and-unlinking-2h1g)
 
 🗑️

