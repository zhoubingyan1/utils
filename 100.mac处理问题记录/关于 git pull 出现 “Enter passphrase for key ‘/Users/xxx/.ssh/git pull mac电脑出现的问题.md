
### 关于 git pull 出现 “Enter passphrase for key ‘/Users/xxx/.ssh/id_rsa‘ ”的问题

>在终端输入
>解决方法：

> 1.输入命令 ssh-keygen -p
> 然后弹出，Enter file in which the key is (/c/Users/xxx/.ssh/id_rsa):
> 直接按回车
> 2.再出现Enter old passphrase:
> ->然后输入密码（当前mac锁屏密码）
> 3.Enter new passphrase (empty for no passphrase):
> 后面的都直接按回车
> Enter same passphrase again:
> 继续按回车
> 出现Your identification has been saved with the new passphrase.
> 说明已经设置成功
> ->再git pull 就可以啦



### HTTP/2 stream 1 was not closed cleanly before end of the underlying stream


最近使用 git 遇到clone 项目出现 ‘HTTP/2 stream 1 was not closed cleanly before end of the underlying stream’ 错误提示，解决办法就是停用 http/2 协议，改用 http/1.1

执行以下命令后再clone 项目就好了
`
git config --global http.version HTTP/1.1
`
