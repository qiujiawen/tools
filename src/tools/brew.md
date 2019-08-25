# Homebrew

brew是MacOS上一个软件包管理工具，MacOS上使用brew可以非常容易进行包的管理。

*   支持平台	MacOS

*   开发语言	Ruby

*   官方网站 https://brew.sh/

brew 安装目录 /usr/local/Cellar

brew 配置目录 /usr/local/etc

brew 命令目录 /usr/local/bin   注:homebrew在安装完成后自动在/usr/local/bin加个软连接，所以平常都是用这个路径

## 安装

因为 brew 是 ruby 开发的，需要使用 brew 先确认 ruby 是否已经安装

安装命令：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```


## 常用命令

软件包安装的目录是在：/usr/local/Cellar

```
brew -v                     # 版本确认
brew list                   # 列出当前安装的软件
brew install nodejs         # 安装 node
brew upgrade nodejs         # 更新 node
brew remove nodejs          # 删除 node
brew search nodejs          # 查询与 nodejs 相关的可用软件
brew info nodejs            # 查询 nodejs 的安装信息
```

## brew services

brew services 是一个非常强大的工具，可以用来管理各种服务的启停。

常用命令：

```
brew install elasticsearch          # 安装 elasticsearch
brew services start elasticsearch   # 启动 elasticsearch
brew services stop elasticsearch    # 停止 elasticsearch
brew services restart elasticsearch # 重启 elasticsearch
brew services list                  # 列出当前的状态
```