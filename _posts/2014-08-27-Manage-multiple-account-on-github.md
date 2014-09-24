---
layout: post
title: "Manage multiple account on Github"
description: "manage multiple account on Github"
category: Protocol
tags: [Github, account, multiple]
---
{% include JB/setup %}

在这里归纳了使用git bash管理多个Github账号的方法。
===========================================
*主要依据是类似修改host，修改访问服务器的名称，所以只需要在~/.ssh/文件夹下增加一个config文件即可搞定*

前提是已经拥有不同的Github.com账号（这里以 typeA 和 typeB 举例）。本地git bash通过ssh默认连接的账户是：typeA，这里需要新增一个管理账号：typeB。

##1. 配置SSH  

* 生成两个账号的密钥  

	ssh-keygen -t rsa -C "youremail@gmail.com"
	
根据提示输入文件名为：~/.ssh/id_rsa_typeA,  
同样制作第二个账号的密钥：

	ssh-keygen -t rsa -C "youremail@163.com"

根据提示输入文件名为：~/.ssh/id_rsa_typeB.

**在Github账号中分别添加id_rsa_typeA.pub, id_rsa_typeB.pub**  

* 设置config文件  
代码如下：  

	cd ~/.ssh/  
	touch  config  
	vim config  

打开文件后，进行如下配置：

	#typeA account 即为默认连接
	Host github.com
		HostName github.com
		User git
		IdentityFile ~/.ssh/id_rsa_typeA

	#typeB account
	Host typeB.github.com  ##此处可以修改为其他名称
		HostName github.com
		User git
		IdentityFile ~/.ssh/id_rsa_typeB

以上方法将 typeB.github.com 转到typeB账号的github.com中。依次类推，也可以使用 typeA.github.com代替原github.com。  

也就是原先repo地址为：

	git@github.com:typeB/test.git

现在修改为： 

	git@typeB.github.com:typeB/test.git  
	
至此，配置已经完成。  

##以下使用实际代码测试  
（typeA=bakerwm, typeB=noncoding, typeB.github.com=ncgithub.com)  

* 测试两个账号与Github的SSH连接  

	$ssh -T git@github.com 
	Hi bakerwm! You’ve successfully authenticated, but GitHub does not provide shell access.  
	
	$ssh -T git@nc.github.com  
	Hi noncoding! You’ve successfully authenticated, but GitHub does not provide shell access.  
	
最后记住一点：操作第二个账号的repo时，使用 git@nc.github.com 代替原先 *git@github.com*。  

   	
	

