---
title: 第一个实习issue
date: 2019-07-31 02:35:51
tags: 
 - doxygen 
 - cmake 
 - ninja
 - nginx 
categories: 实习
---
利用`doxygen`来生成`API`文档，利用`nginx`搭载静态服务器便于项目组成员查阅代码
<!--more-->
### doxygen

`doxygen`能给`cpp`代码生成很漂亮的文档，同时`doxygen`能调用`graphviz`生成很漂亮的思维导图。但是，`doxygen`自身对代码里的注释是有一定要求的，我们常用的双斜杠`//`并不能被`doxygen`识别，三斜杠`///`才是`doxygen`的识别范围，很明显，`doxygen`的注释与`cpp`的注释是被包涵关系。

项目组经验老道的人都是用的三斜杆，还有一些刚来不是很久的喜欢用双斜杠。这给处理代码的人带来了一定的工作量。

还有另外一种注释方式，它有点像这样：
```
/**   
  * comment text here   
  */  
```
给他人写注释的时候，我们的想法无非如下四点：

*   接口的功能
*   接口输入参数需要遵循的条件
*   接口的返回值
*   接口注意事项

对应的例子如下：

*   接口的功能
```
/**  
  * @param url the media loacation to play  
  */  
bool play(const std::string&url);  
```
*   接口输入参数需要遵循的条件
```
/**   
  * @param\[out\] dest The memory area to copy to   
  * @param\[in\] src The memory area to copy from   
  * @param\[in\] n The number of bytes to copy   
  */   
void memcpy(void \*dest,const void \*src,size.t n);  
```
*   接口的返回值
```
/**   
  * @return true on success, false otherwise   
  */   
bool play(const std::string&url);  
```
*   接口的注意事项
```
/**   
  * @throw std::out\_of\_range if idx>=vetor.size()   
  */   
bool vector::at(size_t idx);  
```
```
/**   
  * @warning application may crash. if dest or src is null   
  */   
void memcpy(void \*dest, const void \*src, size.t n);  
```
### cmake

写好了代码，当然不希望每次都要手动生成`API`文档，更加希望每次代码编译的时候就能热生成`API`文档。`cmake`提供了这样的方法，只需将需要集成的工具的配置文件加入，然后编写`CMakeLists.txt`。工具就被集成到项目中了。

### ninja

一款编译器，如其名的快速编译。

### nginx

生成好了`API`文档，当然希望它可以被项目组的其他成员访问。利用`nginx`可以快速搭建一个静态服务器，只需告诉项目组成员的人端口号，项目组的成员即可查看文档。

### 实现

*   在分支中创建一个用于自己工作的文件夹`docs`
*   在`docs`中插入`doxygen`的配置文件`Doxyfile`，`doxygen -g -s(用于省去注释)`
*   配置`Doxyfile`，虽然配置文件中选项很多，但是我们需要修改的并不多，以下涉及一些cmake中定义的参数
```
<OUTPUT_DIRECTORY> = (这一栏可以删去，在CMakeLists.txt中可以写)   
<INPUT> = @PROJECT\_SOURCE\_DIR@   
<RECURSIVE> = YES(可以递归地处理文档目录，类似-rf中r)   
<EXTRACT_ALL> = YES   
<EXTRACT_PRIVATE> = YES   
<EXTRACT_STATIC> = YES   
<GENERATE_LATEX> = NO   
<SOURCE_BROWSER> = YES  
```
*   配置`cmake`，在`docs`中创建一个`CMakeLists.txt`，然后进行编辑
```
# add a target to generate API documentation with Doxygen  
IF(NEED_TO_GENERATE_DOCUMENT)  ///  编译时传入参数，控制文档生成   
FIND_PACKAGE(Doxygen)  ///  确认有系统中`doxygen`   
OPTION(BUILD_DOCUMENTATION "Create and install the HTML based API documentation (requires Doxygen)" ${DOXYGEN_FOUND}) ///  生成文档是`cmake`中的一个选项   
IF(BUILD_DOCUMENTATION)   
 IF(NOT DOXYGEN_FOUND)   
 MESSAGE(FATAL_ERROR "Doxygen is needed to build the documentation.")  
 ENDIF()  ///  再次确认系统中有`doxygen`   
    
 SET(doxyfile ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile) ///  别名  
 SET(doxyfile ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile) ///  别名   
    
 ///将源代码中一些cmake形参通过cmake变成实参   
 CONFIGURE_FILE(${doxyfile} ${doxyfile} @ONLY)   
    
 ADD_CUSTOM\TARGET(doc   
 COMMAND ${DOXYGEN_EXECUTABLE} ${doxyfile}   
 WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}   
 COMMENT "Generating API documentation with Doxygen"   
 VERBATIM)  ///把编译时需要的东西列出来   
    
 INSTALL(DIRECTORY ${CMAKE\_CURRENT\_BINARY_DIR}/html DESTINATION share/doc/html)  ///输出文档，并在宿主机上进行拷贝   
ENDIF()   
ENDIF()  
```
*   利用cmake和ninja对源代码进行编译，`cmake -G Ninja -D NEED_TO_GENERATE_DOCUMENT=ON ..`，`ninja`
*   可以在`docs/html`中看到生成的`html`文件了
*   由于在宿主机上拷贝了一份`html`，可以方便的利用`nginx`搭载静态服务器来展示内容了
*   创建一个`nginx`容器，`docker run --name [docker_name](mynginx) -P -d -v /share/doc/html:/usr/share/nginx/html nginx`
*   配置`nginx`容器，如果你的静态服务器要给大量的人访问，最好配置一下，不让电脑顶不住，我因为是给项目组的人看，所以没有配置
*   利用`docker ps`查看宿主机的哪个端口同容器链接了，在浏览器中输入`[localhost_ip]:[port_id]`即可访问文件
*   提交项目，提交`issue`，完成`issue`

### `issue`收获

*   学习规范的编程，学习规范的注释
*   学习`cmake`语法
*   了解`cmake`预定义参数
*   了解`doxygen`配置
*   了解`ninja`
*   了解如何利用`nginx`搭载静态服务器
