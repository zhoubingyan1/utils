 ## 目录
 * [引子](#reason)
 * [1 光标设置和 Xcode 安装](#1)
 * [2 Shell 设置](#2)
 * [3 Homebrew](#3)
 * [4 Node 版本管理工具](#4)
 * [5 解压工具](#5)
 * [6 App 安装许可](#6)
 * [其它工具](#other)
 * [参考资料](#reference)
 
 ## 引子
 拿到一个新的 Mac ，之前配过一次，但都忘了，感觉好浪费，这次要记下来。这次网络无法访问外网。
 
 版本 macOS Catalina 10.15.3
 
 ## 1 光标设置和 Xcode 安装
 第一次开机会有引导，跟着引导走就可以了。有账号的话登录帐号，没有的就注册一个，会用到的。
 
 默认的光标在引导的时候就可以感觉到，反应比较慢，而且单击都要用力按一下。为了后面使用光标更流畅，就要设置一下。
 
 左上角“**系统偏好设置**” -> “**触摸板**”，在“**光标与点按**”栏中根据自己习惯设置，下面是个人偏好的设置：
 
* “轻点来点按”-勾选
* “跟踪速度”-快

顿时感觉流畅了不少！

然后通过自带的 App Store 安装 Xcode ，里面会包含很多工具，例如 Git 。

## 2 Shell 设置
如果要安装开发相关的工具，少不了使用 Shell 。因此要选个好用的 Shell。下面是官网的信息：

* 从 macOS Catalina 版开始，zsh (Z shell) 是所有新建用户帐户的默认 Shell。
* bash 是 macOS Mojave 及更低版本中的默认 Shell。

可以用命令先查看一下有那些，打开应用程序“**终端**”输入：

```shell
cat /etc/shells
```

推荐使用 [zsh](http://zsh.sourceforge.net) 。

有些时候会出现设置默认 Shell 为 zsh 的提示。

```
chsh -s /bin/zsh
```

### oh-my-zsh
设置后接着就是 [oh-my-zsh](https://ohmyz.sh) 。按照文档中命令执行：

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

报错：

Failed to connect to raw.github.com port 443: Connection refused

这个由于被墙，那就直接修改 hosts 指向 ip 。在 [IPAddress.com](https://www.ipaddress.com) 上找对应 ip 地址。发现有个 DNS 地址，开始下面的步骤：

1. `sudo su`，获取超级管理员权限，hosts 文件默认只读，一般权限无法修改。如果自己一开始就是就可以跳过这步。
2. `vim /etc/hosts` ，编辑 hosts 文件，加入 `199.232.28.133 github.map.fastly.net`。 vim 基本用法见[这里](https://www.cnblogs.com/dongxiaodong/p/10078725.html)。
3. 再执行上面安装 oh-my-zsh 的指令，要等一点时间。

接着就是语法高亮的插件 [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md) 。

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git

echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc

source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

安装成功后，个人经验是把 hosts 的修改复原。不然在浏览器访问相关的网站时，会失败。

[Back to top ⬆️](#index)
## 3 Homebrew
然后就要安装各种包了，这个时候就需要一个包管理工具，推荐 homeBrew 。按照官网的方法没翻墙是不行的。国内有一些镜像可以。首先先介绍一下两个概念：

* brew ：下载源码解压安装，同时会包含相关依赖库，并自动配置好环境变量。主要用来下载一些不带界面的命令行下的工具和第三方库来进行二次开发。
* brew cask ： 是已经编译好了的应用包，仅仅是下载解压，放在统一目录下。主要用来下载一些带界面的应用软件。

下面就来通过镜像安装。文档在[这里](https://mirror.tuna.tsinghua.edu.cn/help/homebrew/)。

```
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
 
 git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
 
 brew update
 ```
 
 期间碰到一个问题：
 
  fatal: cannot change to '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask': No such file or directory
 
 到对应路径下发现的确没有 **homebrew-cask** 这个文件夹，那么就到对应目录新建一个：
 
 ```shell
 mkdir homebrew-cask
 ```
 
 重新执行指令即可。安装完成后，常见的指令见自带帮助。
 
 ```shell
 brew -h
 ```
 
 ## 4 Node 版本管理工具
 Node 版本管理在 Mac 中常用有两个： [n](https://github.com/tj/n) 、 [nvm](https://github.com/nvm-sh/nvm) 。 下面尝试用 Homebrew 安装一下 nvm ：

```shell
brew install nvm
```

要配置环境变量，很多方式见[文档](https://github.com/nvm-sh/nvm/blob/master/README.md)。brew 安装结果中也会有提示：

![71-nvm](https://camo.githubusercontent.com/098d1d02ffe1e24bf0ba06dae416f35c3da317d00858046e8598b1b9e84d4ffa/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37312f6e766d2e706e67)

[Back to top ⬆️](#index)
## 5 解压工具
有些软件在 App Store 里面没有，网上去下载有的要解压。 可以使用工具 The Unarchiver ，免费无广告，在 App Store 里面可以搜索到。

## 6 App 安装许可
在“**安全性和隐私**”中，允许安装的 App 最宽松的约束条件是“**App Store 和 被认可的的开发者**”。但有些时候一些应用这个条件都达不到，这个时候执行下面的命令：

```
sudo spctl --master-disable
```

再次进入到“**安全性和隐私**”中，就出现了“**任何来源**”的选项。

![71-private](https://camo.githubusercontent.com/3e1449ac9b9d36da2ef41b52aec54343a2b4835e9ce2acc28661a813828696af/68747470733a2f2f7878686f6c69632e6769746875622e696f2f7365676d656e742f696d616765732f37312f707269766174652e6a7067)

经过上面的配置后，接着就是看自己的需要安装其它软件了。

[Back to top ⬆️](#index)
## 其它工具
### Google 浏览器
开发首选浏览器，下载见[这里](https://www.google.cn/chrome/) 。

* 火狐浏览器，下载见[这里](http://www.firefox.com.cn/)。

### VSCode
编辑器，具体见[官网](https://code.visualstudio.com/)。

### Sourcetree
Git 库管理工具，下载见[这里](https://www.sourcetreeapp.com/)

### SwitchHosts
跨平台切换 host 工具，下载见[这里](https://github.com/oldj/SwitchHosts/releases) 。

### PxCook
设计工具，具体见[官网](https://www.fancynode.com.cn/pxcook) 。

### WPS
文档、表格、PPT 查看编辑。App Store 内可以搜索到。

### whistle
跨平台抓包工具，库见[这里](https://github.com/avwo/whistle)。

### nrm
切换 npm 源，库见[这里](https://github.com/Pana/nrm)。感觉并不是很必要。

### 钉钉
工作团队交流，App Store 内可以搜索到。

### 腾讯家族
* QQ：App Store 内可以搜索到。
* 微信：App Store 内可以搜索到。
* 企业微信：App Store 内可以搜索到。
* 腾讯会议：App Store 内搜不到，下载见[这里](https://cloud.tencent.com/act/event/tencentmeeting_free?fromSource=gwzcw.3375071.3375071.3375071&utm_medium=cpc&utm_id=gwzcw.3375071.3375071.3375071)，限时免费，不知道什么时候就用不了了。

### 欧路词典
本地翻译，App Store 内可以搜索到。

### Adobe Acrobat Reader
更好的 PDF 阅读体验，下载见[这里](https://acrobat.adobe.com/cn/zh-Hans/acrobat/pdf-reader.html?promoid=C4SZ2XDR&mv=other)。

### 向日葵
TeamViewer 出现安全问题之后，还能正常免费使用的远程控制工具，具体见[官网](https://sunlogin.oray.com/personal/)。

### Docker
开发相关，官网要注册，可以通过 Homebrew 安装，具体见[这里](https://github.com/XXHolic/segment/issues/77#docker)相关说明。

### MySQL Workbench
MySQL 数据库 GUI 。下载见[这里](https://dev.mysql.com/downloads/workbench/) 。

[Back to top ⬆️](#index)
## 参考资料
* [在 Mac 上将 zsh 用作默认 Shell](https://support.apple.com/zh-cn/HT208050)
* [Oh My ZSH!](https://ohmyz.sh)
* [vim || vi 的详细使用手册](https://www.cnblogs.com/dongxiaodong/p/10078725.html)
* [Homebrew/Linuxbrew 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/homebrew/)

🗑️

