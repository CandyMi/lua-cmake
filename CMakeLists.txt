cmake_minimum_required(VERSION 2.8...3.12)

project(lua)

# 打印操作系统
message("-- Detecting OS: '${CMAKE_SYSTEM_NAME}'")

# Lua 头文件
set(LIBLUA_INCLUDE lua.h luaconf.h lualib.h lauxlib.h lua.hpp)

file(GLOB LUA_SOURCES lua.c)
file(GLOB LUAC_SOURCES luac.c)
file(GLOB LIBLUA_SOURCES l*.c)
list(REMOVE_ITEM LIBLUA_SOURCES ${LUA_SOURCES})
list(REMOVE_ITEM LIBLUA_SOURCES ${LUAC_SOURCES})

# 可选C++编译器
if (USE_CXX)
  set_source_files_properties(${LUA_SOURCES} ${LUAC_SOURCES} ${LIBLUA_SOURCES} PROPERTIES LANGUAGE CXX)
  if (CMAKE_CXX_COMPILER_ID MATCHES "Clang") # Clang 编译器会有警告
    add_compile_options(-Wno-deprecated)
  endif()
endif()

# 增加 CCache 支持
find_program(CCACHE_FOUND ccache)
if(CCACHE_FOUND)
  set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
  set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache)
endif()

# 生成 Liblua
add_library(liblua_object OBJECT ${LIBLUA_SOURCES})

add_library(liblua STATIC $<TARGET_OBJECTS:liblua_object>)
if (WIN32)
  add_library(liblua-dll SHARED $<TARGET_OBJECTS:liblua_object>)
  target_compile_definitions(liblua-dll PRIVATE LUA_BUILD_AS_DLL LUA_COMPAT_5_3 LUA_COMPAT_5_2)
else()
  add_definitions(-DLUA_USE_POSIX -DLUA_USE_DLOPEN -DLUA_COMPAT_5_3 -DLUA_COMPAT_5_2)
  target_compile_options(liblua PRIVATE -fPIC -pipe)
  target_link_libraries(liblua PRIVATE dl PRIVATE m)
endif()

# Liblua 静态库
set_target_properties (liblua PROPERTIES PUBLIC_HEADER "${LIBLUA_INCLUDE}" OUTPUT_NAME "liblua" PREFIX "")
install (TARGETS liblua ARCHIVE DESTINATION lib PUBLIC_HEADER DESTINATION include)

# Liblua 共享库
if (WIN32)
  set_target_properties(liblua-dll PROPERTIES OUTPUT_NAME "liblua" PREFIX "")
  install(TARGETS liblua-dll LIBRARY DESTINATION lib)
endif()

# 生成 Lua 可执行文件
if (LUA_SOURCES)
  add_executable(lua ${LUA_SOURCES})
  if (NOT WIN32) # 自动检测是否存在`ReadLine`
    find_path(READLINE_INC_FOUND NAMES readline/readline.h)
    find_library(READLINE_LIB_FOUND NAMES readline)
    find_library(NCURSES_LIB_FOUND NAMES ncurses)
  endif()
  if (READLINE_INC_FOUND AND READLINE_LIB_FOUND AND NCURSES_LIB_FOUND)
    add_definitions(-DLUA_USE_READLINE)
    include_directories( ${READLINE_INC_FOUND} )
    target_link_libraries(lua PRIVATE liblua PRIVATE ${NCURSES_LIB_FOUND} ${READLINE_LIB_FOUND})
  else()
    target_link_libraries(lua PRIVATE liblua)
  endif()
  install(TARGETS lua RUNTIME DESTINATION bin)
else()
  message("[Warning]: Unable to generate `lua.exe`, the `lua.c` file cannot be found.")
endif()

# 生成 Luac 可执行文件
if(LUAC_SOURCES)
  add_executable(luac ${LUAC_SOURCES})
  target_link_libraries(luac PRIVATE liblua)
  install(TARGETS luac RUNTIME DESTINATION bin)
else()
  message("[Warning]: Unable to generate `luac.exe`, the `luac.c` file cannot be found.")
endif()
