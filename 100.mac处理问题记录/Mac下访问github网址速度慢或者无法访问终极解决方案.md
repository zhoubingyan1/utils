## Mac下访问github网址速度慢或者无法访问终极解决方案


```js
    // 1.进入host文件s
    sudo vim /etc/hosts
    // 2.进入编辑模式, 按E键，然后回车
    // 3.启动编辑模式 按I键，启动编辑模式。
    // 4.把以下命令添加到host文件内

    # Github
    151.101.185.194 github.global.ssl.fastly.net
    140.82.114.4 github.com 
    151.101.112.133 assets-cdn.github.com 
    151.101.184.133 assets-cdn.github.com 
    185.199.108.153 documentcloud.github.com 
    192.30.253.118 gist.github.com
    185.199.108.153 help.github.com 
    192.30.253.120 nodeload.github.com s
    151.101.112.133 raw.github.com 
    23.21.63.56 status.github.com 
    192.30.253.1668 training.github.com 
    192.30.253.112 www.github.com 
    151.101.13.194 github.global.ssl.fastly.net 
    151.101.12.133 avatars0.githubusercontent.com 
    151.101.112.133 avatars1.githubusercontent.com

    // 注意： 目前都是本地hosts配置了http://github.com的ip地址，如果访问github失败，或者访问网速慢，可能就是github的ip地址换了或者ip地址丢包严重。可以通过ping github.com查看时长以及丢包率。

    // 如果需要修改github ip地址，可以通过https://github.com.ipaddress.com ，了解当前github.com的ip地址。

    // 5. 按esc键。退出编辑模式
    // 6.保存并退出host文件
    :wq


```


### Mac 中显示和隐藏被隐藏的文件和文件夹
在电脑中打开终端，将以下代码执行一次即可。

1、显示被隐藏文件和文件夹

defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
2、隐藏被隐藏的文件和文件夹

defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder



### Chrome 打开网站提示 “您的连接不是私密连接” 解决办法。
使用 Chrome 打开某些网站时可能会出现 “您的连接不是私密连接” 的提示，如果需要继续打开，可在当前页面使用键盘直接输入以下文字，输入完成后浏览器会自动重新刷新，刷新后即刻正常打开。

thisisunsafe