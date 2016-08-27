# 折腾：coding.net IDE node环境的安装

地址：http://blog.xieliqun.com/2016/08/27/online-IDE-node-enviroment/

> 作为一个码农，我们经常会有这样一个痛点：项目出现一个紧急bug，但自己又不在公司，怎办呢？虽然项目可能在git或者svn上，但clone下来安装测试环境也是个麻烦的是啊。于是，我们想到了在线IDE。作为一个前端开发者nodeJS环境是非常重要的，今天花了一个多小时折腾了下[coding.net](https://ide.coding.net)。


----------

### IDE介绍

以前我一直在用[nitrous.io](https://nitrous.io)的在线IDE，它是一个国外的在线IDE，访问有点慢，一旦网络不好就掉线，挺好的地方就是，它每个月有50个小时的免费使用。coding.net相当于是一个国内版的github，也是一个远程的git仓库，它有个在线IDE还挺好用的。有终端命令面板、支持vim和 Emacs的编辑器、在线运行环境、支持git（哎，帮别人做广告了，不知道coding能不能发我点稿费）。

下面进入主题。


### 安装nvm

nvm是nodejs版本管理工具。可以在多个nodejs版本中进行切换，降低了升级nodejs时的成本。

下载nvm，直接在终端面板把它clone过来就行了。为了不影响项目目录，我们把它clone在项目根目录的同级目录。
```bash
-> ~ cd ..
-> ~ git clone https://github.com/creationix/nvm.git
```

接下来，进入nvm目录
```bash
-> ~ cd nvm
```

安装nvm
```bash
-> ~ ./install.sh
```

安装完后，让系统自动执行它
```bash
-> ~ source ./nvm.sh
```

现在就可以使用nvm命令了，查看一下help
```bash
-> ~ nvm -help
```


### 安装nodeJS

查看可以安装的nodejs版本，这里将会输出一大堆可供安装的nodejs版本
```bash
-> ~ nvm ls-remote
```

选择一个进行安装，我安装的是v4.5.0
```bash
-> ~ nvm install v4.5.0
```

当然你可以安装多个版本，例如我还要安装最新版本，测试一下它的新特性
```bash
-> ~ nvm install v6.4.0
```

当你想切换回原先稳定版本时，你可以
```bash
-> ~ nvm use v4.5.0
```

安装完node，你就可以使用node和npm了。你可以安装一些自动化工具啊，一些插件啊什么的。


### 一些问题

1、这些在线IDE虽然挺方便的，但毕竟没有本地的好，还有一些安全和网络限制的问题，另外到后面可能还要收费。

2、nvm问题，因为coding没有给用户root权限，所以你每次要运行node前都必须运行一下nvm。

```bash
-> ~ cd ..
-> ~ cd nvm
-> ~ source ./nvm.sh
```

### END