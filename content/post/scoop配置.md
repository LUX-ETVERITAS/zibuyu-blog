+++
date = '2026-03-01T19:35:48+08:00'
draft = false
title = 'Scoop配置'
+++

Scoop 是一个在 Windows 系统的软件安装器, 主要用于开源软件和命令行开发工具的安装, 并支持软件版本切换

## 安装 Scoop

需要安装 [PowerShell](https://learn.microsoft.com/zh-cn/powershell/) 或者 [Windows PowerShell 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)

- 设置 PowerShell 语言模式为 `FullLanguage`:
- 将执行策略设置为 `RemoteSigned` , `Unrestricted` 或 者 `Bypass`. 下面是设置为 `RemoteSigned` 的命令:

```pwsh
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

然后运行以下命令安装 Scoop:

```pwsh
irm get.scoop.sh | iex
```

因为我只有一个C盘所以没有修改默认安装路径, 如果需要修改安装路径可以下载安装器并手动执行:

```pwsh
irm get.scoop.sh -outfile 'install.ps1'
```

查看配置参数

```pwsh
.\install.ps1 -?
```

scoop 和普通程序安装到D盘, 全局程序安装到F盘

```pwsh
.\install.ps1 -ScoopDir 'D:\Applications\Scoop' -ScoopGlobalDir 'F:\GlobalScoopApps' -NoProxy
```

还可以用 `-Proxy` 参数设置代理

## 安装软件

安装软件可以用 `scoop search <app>` 或者 [scoop bucket](https://scoop.sh/) 查找软件, 默认的 `scoop search` 搜索速度慢, 金额已安装 `scoop-search` 插件后搜索速度会快很多, 安装 `scoop-search` 插件的命令是 `scoop install scoop-search`

用 `scoop install <app>` 安装软件 `scoop install <app>` 如果需要全局安装可以加上 `-g` 参数 `scoop install <app> -g`.
如果需要安装特定版本的软件可以用 `scoop install <app>@<version>` 例如 `scoop install nodejs@14.21.3` 安装 Node.js 14.17.0 版本
其它的命令还有：

- 查看已安装的软件：`scoop list`
- 查看可以升级的软件：`scoop status`
- 查看某一程序的网站主页：`scoop home <app>`
- 删除已安装软件的旧版本，如删除所有软件旧版本：`scoop cleanup *`
- 清理软件缓存，通常是下载的软件安装包。以下命令清除所有缓存，即清空 Scoop 目录下的 cache 文件夹：`scoop cache rm *`
- 切换版本，比如已经安装 Python 3.x 和 Python 2.7，想从 3.x 切换到 2.7：`scoop reset python27`
- 彻底卸载软件：`scoop uninstall <app> -p`，这个命令会删除软件的所有文件和配置
  有些在 `persist` 目录下的软件文件使用 `scoop uninstall -p <app>` 或者需要手动删除
- 升级软件：`scoop update <app>` 升级某个软件, `scoop update *` 升级所有软件
- 查看软件安装路径：`scoop which <app>`，例如 `scoop which nodejs` 查看 Node.js 的安装路径
- 查看软件版本：`scoop info <app>`，例如 `scoop info nodejs` 查看 Node.js 的版本信息
- 诊断 Scoop 安装和配置问题：`scoop checkup`，这个命令会检查 Scoop 的安装和配置是否正确，并提供修复建议. 如果发现问题, 请根据提示进行修复即可 `scoop install 7zip innounp dark`
- 停止更新软件：`scoop hold <app>`，例如 `scoop hold nodejs` 停止更新 Node.js 软件, 之后就不会再更新这个软件了, 如果需要恢复更新可以用 `scoop unhold <app>` 恢复更新

## bucket

Scoop 的 bucket 是一个软件仓库, 包含了很多软件的安装脚本, 可以通过 `scoop bucket add <bucket>` 添加 bucket, 例如 `scoop bucket add extras` 添加 extras bucket, 然后就可以安装 extras bucket 中的软件了
目前 Scoop 官方提供了以下 bucket:
main
extras
versions
nirsoft
sysinternals
php
nerd-fonts
nonportable
java
games
还有一些第三方 bucket, 可以在 [Scoop Bucket](https://scoop.sh/#/buckets) 页面找到
查看已添加的 bucket 可以用 `scoop bucket list`, 添加官方 bucket 使用 `scoop bucket add <bucket>`，例如 `scoop bucket add extras` 添加 extras bucket, 之后就可以安装 extras bucket 中的软件了
如果是第三方 bucket, 可以用 `scoop bucket add <bucket> <url>` 添加 bucket, 例如 `scoop bucket add mybucket https://github.com/myuser/mybucket`
删除 bucket 可以用 `scoop bucket rm <bucket>`
删除 bucket 后安装的软件会被保留, 但是无法再通过 scoop 更新这些软件了

## 镜像

[南京大学镜像](https://mirrors.nju.edu.cn/) 可以用于加速 bucket 服务（但不能加速软件下载）, 直接在搜索框输入 Scoop 就可以找到 Scoop 的镜像.
可以通过以下命令设置 Scoop 使用南京大学镜像:

```pwsh
scoop config SCOOP_REPO https://mirrors.nju.edu.cn/git/scoop.git/
scoop bucket rm main
scoop bucket add main https://mirrors.nju.edu.cn/git/scoop-main.git/
scoop bucket rm extras
scoop bucket add extras https://mirrors.nju.edu.cn/git/scoop-extras.git/
scoop bucket rm versions
scoop bucket add versions https://mirrors.nju.edu.cn/git/scoop-versions.git/
scoop bucket rm nerdfonts
scoop bucket add nerdfonts https://mirrors.nju.edu.cn/git/scoop-nerdfonts.git/
```

## 创建自己的 bucket

直接根据官方 template 创建一个新的 bucket 仓库, 修改 `appname.json` 文件, 例如 `nodejs.json` 文件. 然后加入 Scoop 的 bucket 列表中, 例如 `scoop bucket add mybucket <url>`

本文作者： zibuyu
本文链接： https://blog.imkasen.com/scoop-guide/
版权声明： 本博客所有文章除特别声明外，均采用 BY-NC-SA 许可协议。转载请注明出处。
