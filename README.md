
# 介绍

  `Lua` 的 `CMAKE` 构建配置文件

# 支持

  下面是目前已测试的**平台**与支持的**编译器**

## 版本

 Lua 5.2 及之后的版本

## 编译器

  * MSVC

  * GCC

  * Clang

## 平台

  * Windows

  * Linux

  * MacOS

  * FreeBSD

# 优势

  * 自带并行编译与缓存加速，加速构建并且降低调试成本.

  * 内部脚本自动检测平台, 减少手动配置带来的复杂性.

  * 便于开发调试/自行学习, 且极易与平台相关工具链集成.

  * 简化开发者的构建成本, 将重心集中在使用与业务编写.

  * 为构建提供更智能的警告和错误提示.

# 使用
  根据您的使用系统，选择下面的命令下载`CMakeLists.txt`文件到当前目录.

## 1. Windows

```bash
# (cmd)命令行
curl --ssl-no-revoke https://raw.githubusercontent.com/CandyMi/lua-cmake/refs/heads/master/CMakeLists.txt -o CMakeLists.txt
```

```bash
# (PowerShell)命令行
curl https://raw.githubusercontent.com/CandyMi/lua-cmake/refs/heads/master/CMakeLists.txt -o CMakeLists.txt
```

## 2. Linux / MacOS

```bash
curl --ssl-no-revoke https://raw.githubusercontent.com/CandyMi/lua-cmake/refs/heads/master/CMakeLists.txt -o CMakeLists.txt
```

# 调试

## Windows

  1. 打开安装`cmake`自带的`cmake-gui`工具 (假设源码目录在`d:\lua5.5\src`)

  2. 在源码目录内填入`d:\lua5.5\src`, 构建文件目录内填入`d:\lua5.5\src\build`.

  3. 点击下面的`configure`按钮并等待,  当显示`Configuring done (...)`再点击右边的`Generate`按钮.

  4. 前面的步骤都完成之后，则可以点击`Open`按钮就会自动使用`Visual Studio`打开该工程.

  5. 右键点击"解决方案'Lua'"，选择"重新生成解决方案" 开始编译. 成功后右键选择"设置Lua为启动项目".

  以上步骤均完成并成功后, 就可以开始断点调试、单步，设置事件、条件进行学习、开发、分析`Lua`.

# 构建

  * 运行命令 `cmake -B build -DCMAKE_BUILD_TYPE=Release && cmake --build build -j4` 即可完成构建.
