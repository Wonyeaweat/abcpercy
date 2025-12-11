# 设置最低版本要求
cmake_minimum_required(VERSION 3.12)

# 项目支持 C 和 C++（cnf_gen.cpp 是 C++）
project(testmain LANGUAGES C CXX)

# 指定源文件
set(SOURCE_FILES testmain.c)

# 定义可执行目标
add_executable(testmain ${SOURCE_FILES})

# 添加必要的包含路径，路径基于当前目录 (zinterface/exttest)
target_include_directories(testmain PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}/../../include
	${CMAKE_CURRENT_SOURCE_DIR}/../../kitty/include
	${CMAKE_CURRENT_SOURCE_DIR}/../../abc
	${CMAKE_CURRENT_SOURCE_DIR}/../../syrup
    ${CMAKE_CURRENT_SOURCE_DIR}/../abcorig/src
)

# Note: cnf_gen.cpp is already part of the abcorig/libabc target. We don't
# compile it twice here to avoid symbol duplication. Test binary links to
# libabc and percy to pull in the CNF-generation entry points.

# 可选：链接到 percy 或其他库（如果需要，用户可取消注释）
target_link_libraries(testmain PRIVATE percy libabc)

# Treat testmain.c as C++ since it includes C++ headers via cnf_gen.h
set_source_files_properties(testmain.c PROPERTIES LANGUAGE CXX)
 
# Ensure ABC/ABC-related compile defs are set so headers use correct namespace/macros
target_compile_definitions(testmain PRIVATE ABC_NAMESPACE=pabc ABC_NO_USE_READLINE DISABLE_NAUTY LIN64)
# percylinklib removed: libabc provides the CNF generator symbols, so we don't
# compile cnf_gen.cpp twice. If you later reintroduce a separate percylinklib
# target, add its compile definitions here.