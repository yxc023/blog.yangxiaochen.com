+++
date = "2016-02-09T04:19:32+08:00"
draft = false
title = "homebrew安装旧版本软件"
categories = [ "homebrew","brew" ]
type="mac"
+++
#### 原创, 转载请注明出处[https://blog.yangxiaochen.com](https://blog.yangxiaochen.com)

不需要用brew versions ,这个已经过时了.

很多有重要版本的软件都已经区分版本的设定formulae了
参见项目地址 https://github.com/Homebrew/homebrew-versions

比如nodejs   有重要的0.10.*版本

    yangxiaochen:/usr/local$ brew search node
    leafnode node node010 node04 node06 node08 nodebrew nodenv

node010就是

同理php也是这样分php56 php55 php54 ….