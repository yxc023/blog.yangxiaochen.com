+++
date = "2016-02-09T09:19:32+08:00"
draft = false
title = "ssh-key生成与配置"
categories = [ "ssh-key" ]
type="linux"

+++

#### 原创, 转载请注明出处[https://blog.yangxiaochen.com](https://blog.yangxiaochen.com)


## 命令

    ssh-keygen

    ssh-copy-id -i ~/.ssh/id_rsa.pub yangxiaochen@XXXXXX

## mac

什么? 你用mac? 拿走不谢.

    brew install ssh-copy-id

## 注意

尽量不用手动拷贝 id_rsa.pub 到目标机器, 拷贝之后的文件权限值不对也不能访问. 附送解决方案:

    chmod 600 authorized_keys
