CMAKE_MINIMUM_REQUIRED(VERSION 3.0) # cmake最低版本要求

PROJECT(NBody)    # 工程名 NBody

# 这段代码可以区分操作系统
MESSAGE("Identifying the OS...")
if(WIN32)
    MESSAGE("This is Windows.")
elseif(APPLE)
    MESSAGE("This is MacOS.")
elseif(UNIX)
    MESSAGE("This is Linux.")
endif()
# 这段代码可以区分操作系统

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11") # 添加c++11标准支持
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc") # 静态链接
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall") # 开启额外警告
if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --target=x86_64-w64-mingw32")
    SET(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} --target=x86_64-w64-mingw32")
endif()
# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -${PROJECT_SOURCE_DIR}/lib/freeglut.lib") # 链接库
# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -${PROJECT_SOURCE_DIR}/lib/glew32.lib") # 链接库
# link_libraries()
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -g3 -ggdb")  # debug模式下 gdb相关选项
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3") # release模式下 gdb相关选项


if(true)# 64位编译
    SET(EXECUTABLE_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/bin/64") # 可执行文件输出目录
    include_directories("${PROJECT_SOURCE_DIR}/include") # 头文件包含目录
    link_directories("${PROJECT_SOURCE_DIR}/lib/64")
else()
    SET(EXECUTABLE_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/bin/32") # 可执行文件输出目录
    include_directories("${PROJECT_SOURCE_DIR}/include") # 头文件包含目录
    link_directories("${PROJECT_SOURCE_DIR}/lib/32")
endif()

AUX_SOURCE_DIRECTORY(NBody DIR_SRCS) # 添加源代码文件夹, 自动扫描所有文件

link_libraries("opengl32")
link_libraries("glu32")
link_libraries("glew32")
link_libraries("freeglut")
# link_libraries("freeglut_static")

add_executable(NBody  # 输出名为NBody的可执行文件
    ${DIR_SRCS}
)

# SET(CMAKE_BUILD_TYPE "Debug")  # Debug模式 选项: Release Debug MinSizeRel RelWithDebInfo
# add_definitions(-DDEBUG) # 相当于代码中 #define DEBUG
# set(CMAKE_VERBOSE_MAKEFILE ON) # 开启调试 出现问题时开启
# enable_testing() # 打开测试
