---
title: 第三个实习issue
date: 2019-08-09 23:18:07
tags: 
  - googletest
  - unittest
  - cmake
categories: 实习
---
这是我花的时间最久的一个`issue`，不是因为它很难，是在想如何做以及在错误的路上走时花了些时间。
<!--more-->

这个`issue`就是集成`googletest`，然后为项目组的人提供`unittest`的模板。之前将重心放在了`googletest`上，为此花了较多时间看一些大型`cpp`项目的测试案例源码。不得不说，想让代码变得只有参与这个项目的人才看得懂挺容易的。如果一个大型项目使用大量的别名和定义很多带参数的宏，需要借助一些工具才能正常的读，光靠文本编辑器是不够了。

# googletest
只要做测试，就需要一些测试框架，`googletest`为我们提供了这样的框架，同时`google`考虑了将`googletest`集成到其他项目中。将项目克隆下来放到自己的项目中就好了。

# unittest
一个优秀的程序员肯定会为自己写的代码写下单元测试，方便日后维护时的校验，好的测试能达到一个很高的代码覆盖率，当然能到90%的就是很好的单元测试了。

# cmake
很好用的一款生成`makefile`的工具，还能传入参数控制`makefile`的生成。

# 项目目录
```
.
├── CMakeLists.txt
├── include
│   ├── CMakeLists.txt
│   ├── example_1
│   │   ├── CMakeLists.txt
│   │   ├── test_1_1.hpp
│   │   └── test_1_2.hpp
│   └── example_2
│       ├── CMakeLists.txt
│       ├── test_2_1.hpp
│       └── test_2_2.hpp
├── src
│   ├── CMakeLists.txt
│   ├── example_1
│   │   ├── CMakeLists.txt
│   │   ├── test_1_1.cpp
│   │   └── test_1_2.cpp
│   └── example_2
│       ├── CMakeLists.txt
│       ├── test_2_1.cpp
│       └── test_2_2.cpp
├── third-party
│   ├── CMakeLists.txt
│   └── googletest
└── unittest
    ├── CMakeLists.txt
    ├── example_test_1
    │   ├── CMakeLists.txt
    │   ├── unittest_test_1_1.cpp
    │   └── unittest_test_1_2.cpp
    └── example_test_2
        ├── CMakeLists.txt
        ├── unittest_test_2_1.cpp
        └── unittest_test_2_2.cpp
```

# 重点注意
重点是`CMakeLists.txt`的编写，有很多的细节要注意，我只把一些基本的写下来，能保证编译以及运行没问题，但是对于一个大型项目来说，这些是不够的，仍需要更多的一些指令来防止可能的问题出现。比如软件版本，再比如出现错误后，能够报错让程序员快速定位错误等等。 


另外就是对于一个大型项目来说，层次清晰，能分模块进行单元测试也很重要，否则每次执行几万条单元测试很费时间，效率较低。

# CMakeLists.txt例子 
一个`CMakeLists.txt`的作用范围为该目录下的除非子目录的其他文件，子目录利用`add_subdiretory`指令加入即可。
这里一共要写11个`CMakeLists.txt`,但从分类角度来说，一共有五类要写，但是由于`third-party`的`googletest`已经被写好了，所以一共是四类要写，分路径举四个例子。 
* .
```
PROJECT(example)
ENABLE_TEST()
ADD_SUBDIRECTORY(include)
ADD_SUBDIRECTORY(src)
ADD_SUBDIRECTORY(third-party)
ADD_SUBDIRECTORY(unittest)
IF(MSVC)
   message("run here")
   set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
   set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
endif()
```

* ./include
```
#由于include目录下仅有子目录而没有需要执行的其他文件
ADD_SUBDIRECTOR(example_1)
ADD_SUBDIRECTOR(example_2)
```

* ./src/example_1
```
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include)
AUX_SOURCE_DIRECTORY(. EXAMPLE_SRC)
ADD_LIBRARY(LIB_EXAMPLE_1 ${EXAMPLE_SRC})
```

* ./unittest/example_test_1
```
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include
${PROJECT_SOURCE_DIR}/third-party/googletest/googletest/include
)
LINK_DIRECTORIES(${PROJECT_BINARY_DIR}/lib)
AUX_SOURCE_DIRECTORY(. UNITTEST_EXAMPLE_1)
ADD_EXECUTABLE(TEST_EXAMPLE_1 ${UNITTEST_EXAMPLE_1})
TARGET_LINK_LIBRARIES(TEST_EXAMPLE_1 LIB_EXAMPLE_1 gtest gtest_main)
ADD_TEST(NAME TEST_EXAMPLE_1 COMMAND TEST_EXAMPLE_1)
```

其他目录仿造上面就可以了，总的来说这个`issue`让我更加了解了一个大型项目的结构，能够让我对于`cmake`的语法更加熟悉了。
