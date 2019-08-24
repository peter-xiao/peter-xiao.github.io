---
title: 第二个实习的issue
date: 2019-08-07 21:37:51
tags:  
  - sphinx
  - rst
categories: 实习
---
`doxygen`能生成给程序员看的文档；`sphinx`能生成给用户看的文档，使用`rst`语言编写文档，利用`sphinx`来生成`html`供用户查看。
<!--more-->

这个工作其实是和代码的关联度不是很高，我大致弄清机理后，写了份`demo`给导师就没有继续做下去了。

# sphinx 
`shinx`这个软件有两个主要的功能，一个是为`python`的注释生成`rst`文档，另一个是把`rst`文档编译成`html`。我分配的任务是生成`cpp`的用户文档，因为`sphinx`是一款为`python`服务的软件，故我需要借助一款名为`breathe`的工具来完成`issue`。  


# rst
这种语言类似`md`，比较好学。

# 实现 
* 首先要利用`doxygen`生成代码及注释的`xml`文件，关于这点如何做，可以参考我写的[第一个实习issue](https://peter-xiao.github.io/2019/07/31/%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%AE%9E%E4%B9%A0issue/)，只需要将`Doxyfile`里面的`GENERATE_HTML`关闭，将`GENERATE_XML`打开即可。
* 创建一个叫`sphinx-doc`的文件夹，进入其中完成所有操作 
```
mkdir sphinx-doc
cd sphinx-doc
```
* 创建一个`sphinx`项目，`sphinx-quickstart` 
```
> Separate source and build directories (y/n) [n]: y
> Project name: sphinx-doc
> Author name(s): my team
> Project version: 1.0
其他的直接回车就好了
```
然后就能在`shpinx-doc`目录下看到如下文件 
```
│  conf.py
│  index.rst
│  make.bat
│  Makefile
│
├─_build
├─_static
└─_templates
```
* 需要进一步配置`conf.py`，使用拓展工具`breathe`，这样就能向文本中插入`cpp`的代码及注释了，所需要的信息，全部由`breathe`在`doxygen`生成的`xml`文件中去寻找
```
extensions = [ "breathe" ]

# 配置breathe，告诉它去找哪个cpp项目的xml
breathe_default_project = "[cpp_project_name]"
```
* 然后你就可以号召你的团队去学习`sphinx`的指令以及由`breathe`拓展出来的指令，一起去完成这个用户文档的编写。

* 最后将`sphinx`集成到项目中，并通过编写`CMakeLists.txt`告诉`breathe`可以到哪里去找到由`doxygen`生成的`xml`。
```
find_package(Sphinx REQUIRED)
 
set(SPHINX_SOURCE ${CMAKE_CURRENT_SOURCE_DIR})
set(SPHINX_BUILD ${CMAKE_CURRENT_BINARY_DIR}/docs/sphinx)
 
add_custom_target(Sphinx ALL
                  COMMAND ${SPHINX_EXECUTABLE} -b html
                  # Tell Breathe where to find the Doxygen output
                  -Dbreathe_projects.CatCutifier=${DOXYGEN_OUTPUT_DIR}
                  ${SPHINX_SOURCE} ${SPHINX_BUILD}
                  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                  COMMENT "Generating documentation with Sphinx")
```

