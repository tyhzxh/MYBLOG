+++
title = 'cmake'
date = 2024-10-12
draft = true
+++

# cmake

## cmake基本语法

### CMakeLists.txt

### 内置的一些函数

`check_cxx_source_compiles` 是 CMake 中的一个内置函数，用于检查给定的 C++ 源代码是否可以成功编译。

它的基本用法如下：

```cmake
check_cxx_source_compiles(SOURCE_CODE [COMPILER_FLAGS] [VARIABLE_NAME])
```

其中：

- `SOURCE_CODE` 是要编译的 C++ 源代码。
- `COMPILER_FLAGS` 是可选的参数，用于指定编译器的额外标志或选项。
- `VARIABLE_NAME` 是可选的参数，用于存储编译结果的变量名。如果编译成功，则该变量被设置为 `TRUE`，否则被设置为 `FALSE`。

在给定的代码片段中，`check_cxx_source_compiles` 被用于检查 `__has_include` 特性的支持。它将一个包含 `#if defined(__has_include) &&  __has_include(<string>)` 的 C++ 源代码作为参数传递给函数，并将结果存储在 `HAVE_CXX17_HAS_INCLUDE` 变量中。如果编译成功，`HAVE_CXX17_HAS_INCLUDE` 将被设置为 `TRUE`，否则将被设置为 `FALSE`。

这段代码是一段C++代码，用于检查编译器是否支持C++17的`__has_include`特性。下面是详细解释：

1. `#if defined(__has_include) && __has_include(<string>)`：
   * `#if`是C++的预处理指令，用于条件编译。
   * `defined(__has_include)`检查是否定义了`__has_include`宏。如果定义了，则继续执行下面的代码。
   * `__has_include(<string>)`检查是否存在`<string>`头文件。如果存在，则返回`1`，否则返回`0`。
   * `&&`是逻辑与运算符，只有当两个条件都为真时，才执行下面的代码。
2. `#include <string>`：
   * 如果上面的条件成立，则包含`<string>`头文件。
3. `int main() { std::string str; return 0; }`：
   * 这是C++的主函数，用于测试`std::string`类型是否可用。
   * 如果`<string>`头文件被成功包含，则`std::string`类型可用，编译器不会报错。

总的来说，这段代码检查编译器是否支持C++17的`__has_include`特性，并且是否可以成功包含`<string>`头文件。如果检查通过，则编译器支持C++17的`__has_include`特性，否则不支持。

在CMake的`check_cxx_source_compiles`函数中，这段代码被用来检查编译器是否支持C++17的`__has_include`特性。如果编译成功，则变量`HAVE_CXX17_HAS_INCLUDE`被设置为`TRUE`，否则被设置为`FALSE`。

## cmake模块文件（后缀为cmake的文件）

😊

###  内置的cmake模块文件

放在/usr/share/cmake-3.22/Modules目录下

刚刚提到的几个内置模块是CMake提供的预定义模块，用于帮助用户简化CMake脚本的编写。以下是这些模块的作用：

1. **CheckIncludeFile**：用于检查系统头文件的存在。例如，检查`stdio.h`头文件是否存在。
```cmake
check_include_file("stdio.h" HAVE_STDIO_H)
```
如果头文件存在，则设置`HAVE_STDIO_H`变量为`TRUE`。

2. **CheckLibraryExists**：用于检查系统库的存在。例如，检查`libm`库是否存在。
```cmake
check_library_exists("m" "sin" "" HAVE_LIBM)
```
如果库存在，则设置`HAVE_LIBM`变量为`TRUE`。

3. **CheckCXXSourceCompiles**：用于检查C++源代码的编译性。例如，检查一个C++源文件是否可以编译。
```cmake
check_cxx_source_compiles("int main() { return 0; }" HAVE_CXX_COMPILER)
```
如果源代码可以编译，则设置`HAVE_CXX_COMPILER`变量为`TRUE`。

4. **FindPackage**：用于查找系统中的包。例如，查找`Boost`包。
```cmake
find_package(Boost 1.70 REQUIRED)
```
如果包存在，则设置`Boost_FOUND`变量为`TRUE`，并定义`Boost_INCLUDE_DIRS`和`Boost_LIBRARIES`变量。

这些模块可以帮助用户简化CMake脚本的编写，避免重复定义一些常用的函数和变量。

### 基本语法

😊

CMake模块的语法与CMake脚本的语法类似，但是有一些特殊的规则和约定。以下是一些CMake模块的语法规则：

1. **函数定义**：CMake模块可以定义函数，使用`function`关键字。函数名必须以大写字母开头，例如`CHECK_MY_LIBRARY`。
```cmake
function(CHECK_MY_LIBRARY LIBRARY_NAME)
  # 函数体
endfunction()
```
2. **变量定义**：CMake模块可以定义变量，使用`set`关键字。变量名必须以大写字母开头，例如`MY_LIBRARY_FOUND`。
```cmake
set(MY_LIBRARY_FOUND TRUE)
```
3. **宏定义**：CMake模块可以定义宏，使用`macro`关键字。宏名必须以大写字母开头，例如`MY_MACRO`。
```cmake
macro(MY_MACRO)
  # 宏体
endmacro()
```
4. **函数调用**：CMake模块可以调用其他函数，使用函数名和参数列表。例如：
```cmake
CHECK_MY_LIBRARY(MyLibrary)
```
5. **变量引用**：CMake模块可以引用变量，使用变量名。例如：
```cmake
if(MY_LIBRARY_FOUND)
  # 代码
endif()
```
6. **条件语句**：CMake模块可以使用条件语句，例如`if`、`else`、`endif`。例如：
```cmake
if(MY_LIBRARY_FOUND)
  # 代码
else()
  # 代码
endif()
```
7. **循环语句**：CMake模块可以使用循环语句，例如`foreach`、`endforeach`。例如：
```cmake
foreach(VAR IN LISTS MY_LIST)
  # 代码
endforeach()
```
8. **注释**：CMake模块可以使用注释，使用`#`符号。例如：
```cmake
# 这是注释
```
9. **文件包含**：CMake模块可以包含其他文件，使用`include`关键字。例如：
```cmake
include(MyOtherModule)
```
10. **返回值**：CMake模块可以返回值，使用`return`关键字。例如：
```cmake
function(MY_FUNCTION)
  # 函数体
  return(TRUE)
endfunction()
```
这些是CMake模块的基本语法规则。通过这些规则，你可以编写自己的CMake模块，封装一些常用的函数和变量，方便在多个项目中共享。