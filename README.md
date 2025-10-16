# 介绍

  `Lua` 的 `CMAKE` 构建配置文件

# 支持

  目前已测试的**平台**与支持的**编译器**

## 版本

 Lua 5.2 及之后的版本

## 编译器

  * MSVC

  * gcc

  * clang

## 平台

  * Windows

  * Linux

  * MacOS

  * FreeBSD

# 使用

  * 在`src`目录下创建`CMakeLists.txt`文件, 并将下面内容复制进去.

  * 然后在`src`目录运行命令 `cmake -B build && cmake --build build`

# 优势

  * 自带并行编译与缓存加速，加速构建并且降低调试成本.

  * 内部脚本自动检测平台, 减少手动配置带来的复杂性.

  * 简化开发者的构建成本, 将重心集中在语言与业务.

  * 为构建提供更智能的警告和错误提示.
