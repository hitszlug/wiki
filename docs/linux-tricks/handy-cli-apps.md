---
title: 另类好用的 Linux 命令行工具
date: 2021-10-31
---

!!! tips "软件名字旁边的 Badge 是什么？"
    这个 badge 显示了这个软件在多少个 **软件仓库**（与发行版并非一一对应，如 Ubuntu 每个版本拥有独立的软件仓库）中可用。直接点击它可以打开详情信息，了解这个软件在不同软件仓库中的版本。

## Homebrew

### 简介

Homebrew 是一款起源于服务 macOS 的包管理器，特色是在用户态进行包管理和安装，做法非常整齐优雅，并且有非常大的社区支持。虽说 Linux 下各发行版也有用很多包管理器，但是总是会缺少一些好用的小软件，这时 Homebrew 就有用武之地了。强烈推荐先安装 Homebrew，因为可能会更方便地安装下面介绍的软件 ~~指下面的软件全部都能用 `brew install` 安装~~。

### 资源

- 官方网站、快速入门：<https://brew.sh>
- Homebrew TUNA 镜像源配置教程：<https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/>

## tldr [![tldr status](https://repology.org/badge/tiny-repos/tldr.svg)](https://repology.org/project/tldr/versions)

### 简介

Too long; didn't read. ~~即 “太长不看”~~ 废话不多说，就是这款小工具的目的。想必各位时不时都要用一些自己没用过的命令，做一些简单的事情，但却要翻看天书一般 man，可谓杀鸡用牛刀。tldr 提供的文档则恰恰相反：直击痛点，通俗易懂，示例清晰。

### 资源

- 官方网站：<https://tldr.sh>
- 快速入门：用法与 `man` 类似，在想要查询的命令前加上 `tldr`，比如查询 `tar` 的用法就运行 `tldr tar`。
- 其他实现：~~因为使用 NodeJS 实现，~~ 官方的实现很慢。可以使用 Rust 实现 tealdeer 来代替。 [![tealdeer status](https://repology.org/badge/tiny-repos/tealdeer.svg)](https://repology.org/project/tealdeer/versions)
- 相关中文教程：<https://learnku.com/articles/23834>

## fasd [![fasd status](https://repology.org/badge/tiny-repos/fasd.svg)](https://repology.org/project/fasd/versions)

!!! danger
    fasd 已经不再被维护，这意味着最好寻找其他的代替方案。
    可以考虑一下使用更现代化的 zoxide。

### 简介

虽说 `z` 快速转跳鼎鼎大名，但我用过最舒服的还是这个工具。只要你曾经访问过某个目录货文件，fasd 能让你只输入目录名或文件名就实现全路径的引用或转跳，而且转跳命令也是 `z`——例如，输入 `z dow` 即可从任意位置转跳到 `~/Download`。fasd 的命令还方便记忆：`f` 代表 file，`a` 代表 all，`s` 代表 show，`d` 代表 directory，完全没有上手难度。

### 资源

- 官方 GitHub：<https://github.com/clvv/fasd>
- 快速入门：安装后可能需要配置 rc 文件（如 zsh 为 `.zshrc`），详细配置参考官方文档。注意，只有正确配置之后它才能记录目录历史（也就是说，只会记录安装后的历史）。
- 相关中文教程：<https://www.howtoing.com/fasd-quick-access-to-linux-files-and-directories>

## sfk (swissfileknife) [![swissfileknife status](https://repology.org/badge/tiny-repos/swissfileknife.svg)](https://repology.org/project/swissfileknife/versions)

### 简介

违背 UNIX 哲学的 “瑞士军刀”，堪比 “要你命 3000”，但是确实好用。它可以批量转换文件换行符，集成并改进了大多数 GNU/Linux 的工具，快速启动一个简易的服务器，还可以转换 Word 文档……同时，还配套了很贴心的教程与资料——“只有想不到，没有做不到”。

软件有点 Old School，不喜勿喷。

### 资源

- 官方网站：<http://stahlworks.com/dev/swiss-file-knife.html>
- 快速入门：输入 `sfk` 查看帮助。

## httpie [![httpie status](https://repology.org/badge/tiny-repos/httpie.svg)](https://repology.org/project/httpie/versions)

### 简介

觉得 `curl` 太麻烦？想要在 CLI 快速收发数据包来测试 API？那就用 **更现代** 的 httpie 吧！httpie 有 GUI 版本（像 Postman 一样），也有 CLI 版本，命令用法非常丝滑。举个例子：`http -a USERNAME POST https://api.github.com/repos/httpie/httpie/issues/83/comments body="HTTPie is awesome! :heart:"`。

~~curl sucks!!~~

### 资源

- 官方网站：<https://httpie.io/cli>
- 快速入门：`http net.lolicon.app/detail` 会以 HTTP 为协议，`GET` 为方法请求这个 URL，`https` 同理。另外，这个程序的 tldr 页面相当实用，建议执行 `tldr http` 查看。
- 相关中文教程：<https://linux.cn/article-10765-1.html>
