---
layout: post
title: Cocoapods 常用命令
author: shefh
category: blog
published: true
---

## [安装](http://blog.devtang.com/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/)

```ruby
 sudo gem install cocoapods
```
## 查看ruby源

```ruby
 gem sources -l
```
## 删除ruby源

```ruby
sudo gem sources -r https://rubygems.org/
```

## 添加ruby源

```ruby
 sudo gem sources -a https://ruby.taobao.org/ ##淘宝镜像
```

## 设置pod环境初始化

```ruby
 pod setup
```
执行这个命令，会创建~/.cocoapods目录，并且在这个目录下创建repos 文件夹，然后会把git上的代码仓库下载到这个文件下。这个过程要等很久，如果没有vpn的话，大半天的时间都会花在等待这个下载过程结束。如果你想看下载进度，可以用命令行到这个目录，然后用 du -sh *来查看下载进度。

## 安装代码依赖

```ruby
 pod install
```

执行这个命令首先回去更新~/.cocoapods下面的代码仓库，会很慢，如果不想去更新，可以执行如下命令：

```ruby
 pod install --no-repo-update
```

## 更新代码依赖

```ruby
 pod update
 pod update --no-repo-update
```

## 更新podspec

```ruby
 pod repo update
```

## 参考资料
[官网](https://guides.cocoapods.org/)

