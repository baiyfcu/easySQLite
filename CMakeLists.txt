project(EasySQLite)
cmake_minimum_required(VERSION 2.8)

#使能c++11
add_compile_options(-std=c++11)

#设置工程源码根目录
set(EasySQLite_Root ${CMAKE_CURRENT_SOURCE_DIR})
#设置头文件目录
INCLUDE_DIRECTORIES(${EasySQLite_Root})

INCLUDE_DIRECTORIES(${project_Root})

#收集源代码
file(GLOB EasySQLite_src_list ${EasySQLite_Root}/easySQLite/*.h ${EasySQLite_Root}/easySQLite/*.cpp ${EasySQLite_Root}/easySQLite/*.c ${EasySQLite_Root}/*.cpp)

#安装目录
if(WIN32)
    set(INSTALL_PATH_LIB $ENV{HOME}/EasySQLite/lib)
    set(INSTALL_PATH_INCLUDE $ENV{HOME}/EasySQLite/include)
else()
    set(INSTALL_PATH_LIB lib)
    set(INSTALL_PATH_INCLUDE include)
endif()


if(NOT WIN32)
    add_compile_options(-Wno-deprecated-declarations)
    add_compile_options(-Wno-predefined-identifier-outside-function)
else()
#防止Windows.h包含Winsock.h
	add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()

# 给解决方案中工程增加宏定义

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")

#添加库
#add_library(EasySQLite STATIC ${EasySQLite_src_list})
add_executable(EasySQLite ${EasySQLite_src_list})
#link_directories(${CMAKE_CURRENT_SOURCE_DIR}/hikDecoder)
MESSAGE(STATUS "CMAKE_BINARY_DIR: ${CMAKE_BINARY_DIR}")


set(VS_FALGS "/wd4819 /wd4996 /wd4018 /wd4267 /wd4244 /wd4101 /wd4828 /wd4309 /wd4573 /utf-8" )


if (WIN32)
    list(APPEND LINK_LIB_LIST WS2_32 Iphlpapi shlwapi)
	set_target_properties(EasySQLite PROPERTIES COMPILE_FLAGS ${VS_FALGS} )
elseif(NOT ANDROID OR IOS)
    list(APPEND LINK_LIB_LIST pthread dl)
endif ()

target_link_libraries(EasySQLite ${LINK_LIB_LIST})

#install(TARGETS EasySQLite  ARCHIVE DESTINATION ${INSTALL_PATH_LIB} LIBRARY DESTINATION ${INSTALL_PATH_LIB})
