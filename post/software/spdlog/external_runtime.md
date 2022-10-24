---
title: cmake external 外部项目设置 runtime 路径
date: 2022-03-06 23:07:32
draft:  false
---

# cmake external 外部项目超级构建设置 runtime 路径

## 简介

 在使用 cmake 超级构建的时候，想要指定二进制文件的输出路径。

ExternalProject_Add  参数中有一个 BINARY_DIR 选项可以指定输出路径，在构建的过程中，发现这个参数输出路径不仅会包含编译的二进制文件，还会把 cmake 自身产生的一些二进制文件同时包含进去。

后参阅 cmake 文档，发现 ExternalProject_Add  中的 CMAKE_ARGS 选项和 set_target_properties 搭配可仅仅指定编译的二进制文件输出到指定路径

## 配置

### 目录结构

![](J:\01_note\00_hugo_blog\images\spdlog_runtime_tree.png)

### 代码

顶层 CMakeLists.txt 内容

```cmake
cmake_minimum_required(VERSION 3.5)

project(demo LANGUAGES CXX)

set_property(DIRECTORY PROPERTY EP_BASE ${CMAKE_BINARY_DIR}/subprojects)

set(STAGED_INSTALL_PREFIX ${CMAKE_BINARY_DIR}/stage)

include(ExternalProject)

ExternalProject_Add( log
  SOURCE_DIR 
    ${CMAKE_CURRENT_SOURCE_DIR}/src
  CMAKE_ARGS
    -DRUNTIME_OUTPUT_DIRECTORY=${PROJECT_SOURCE_DIR}/run
  BUILD_ALWAYS
    1
  INSTALL_COMMAND
    ""
)
```

CMAKE_ARGS 添加 runtime 参数
src 目录中 CMakeLists.txt 内容

```cmake
cmake_minimum_required(VERSION 3.5)

project(demo_core LANGUAGES CXX)

add_executable(core main.cpp)

message("=========================${RUNTIME_OUTPUT_DIRECTORY}")
set_target_properties(core
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${RUNTIME_OUTPUT_DIRECTORY}"
)
```

main.cpp

```c++
#include <iostream>


int main()
{

    std::cout << "hello!!!" << std::endl;

    return 0;
}
```

构建

```shell
mkdir  build && cd build
cmake ..
make
```

![](J:\01_note\00_hugo_blog\images\spdlog_runtime_build.png)

### 产出目录结构和运行

![](J:\01_note\00_hugo_blog\images\spdlog_runtime_run.png)



## 引用

[ExternalProject — CMake 3.23.0-rc2 Documentation](https://cmake.org/cmake/help/v3.23/module/ExternalProject.html#id3)

[cmake-buildsystem(7) — CMake 3.23.0-rc2 Documentation](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#runtime-output-artifacts)

[CMake set CMAKE_RUNTIME_OUTPUT_DIRECTORY for a specific target only - Stack Overflow](https://stackoverflow.com/questions/34011381/cmake-set-cmake-runtime-output-directory-for-a-specific-target-only)





