+++
date = '2026-03-01T19:35:48+08:00'
draft = true
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

安装软件可以用 `scoop search <app>` 搜索