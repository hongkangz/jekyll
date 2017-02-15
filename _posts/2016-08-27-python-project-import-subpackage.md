---
layout: post
title:  "import subpackage"
date:   2016-08-27 13:31:04 +0800
categories: jekyll
tag: jekyll
---

* content
{:toc}


# python package

## 基础知识

当你import的时候，python只会在sys.path这个变量（一个list，你可以print出来看）里面的路径中找可能匹配的package和module。

而一个package跟一个普通文件夹的区别在于，package的文件夹中多了一个__init__.py文件。换句话说，如果你在某个文件夹中添加了一个__init__.py文件，则python就认为这个文件夹是一个package。

__init__.py文件可以是空的（也推荐者这么做），它只是告诉python当前文件夹是一个package。当然，也可以在里面添加一些代码，这些代码会在import这个包的时候运行。

所以，请确保你要import的文件所在的文件夹有__init__.py文件（除非它在sys.path中某个文件夹下）。

## 错误的import做法

如上述project中，如果你想让subpackage2中的foo2来import subpackage1中的foo1，便会出现找不到subpackage1的情况。

目前网络上大部分的做法都是通过sys.path.append(yourpath)之类的方法，将你需要import的module的目录添加到sys.path中。或者，通过修改PYTHONPATH这个环境变量来将添加（跟修改sys.path效果相同）。

但是，这种做法有如下几个缺点：

如果你用PYTHONPATH，那么当有多个项目时，你需要把每个项目的根目录都加入到PYTHONPATH中，会使得PYTHONPATH变得十分臃肿
如果你使用sys.path，由于文件夹是动态添加的，所以当你使用相对路径的时候，实际路径会十分依赖于你的入口函数，当入口函数改变很可能就会导致代码无法运行
如果你使用绝对路径，将你的代码在其他机器上运行的时候需要重新配置这些变量，十分麻烦
正确的做法

## 正确的import

首先，在代码中按照正常方式导入你需要的包

比如，你需要在app.py中导入foo1，则：

from package.subpackage1 import foo1 

虽然你可能发现from subpackage1 import foo1也可以正常运行，但是请避免这种使用相对路径的方法。因为这在python3中将不再支持，同时也有可能会引起奇怪的问题。同时，虽然PEP 328中也给出了 from .subpackage1 import foo1这样的形式，但是还是不要自己给自己制造麻烦，统一使用完整路径（绝对路径）为好。
