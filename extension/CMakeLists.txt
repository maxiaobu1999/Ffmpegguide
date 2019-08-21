#示例
# CMake 最低版本号要求
cmake_minimum_required(VERSION 3.4.1)
#指定项目名
project(hello)
#指定子目录
add_subdirectory(src)


#设置库文件存放路径
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build/lib)

#设置执行文件存放路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build/bin)

#获取当前目录及子目录(递归获取),添加到头文件搜索路径
function(include_sub_directories_recursively root_dir)
    if (IS_DIRECTORY ${root_dir})               # 当前路径是一个目录吗，是的话就加入到包含目录
        message("include dir: " ${root_dir})
        include_directories(${root_dir})
    endif()

    file(GLOB ALL_SUB RELATIVE ${root_dir} ${root_dir}/*) # 获得当前目录下的所有文件，让如ALL_SUB列表中
    foreach(sub ${ALL_SUB})
        if (IS_DIRECTORY ${root_dir}/${sub})
            include_sub_directories_recursively(${root_dir}/${sub}) # 对子目录递归调用，包含
        endif()
    endforeach()
endfunction()

#项目的所有目录都为头文件搜索路径
include_sub_directories_recursively(${PROJECT_SOURCE_DIR})

#添加库文件搜索路径
link_directories(
        ${PROJECT_SOURCE_DIR}/build/lib
)

#添加子目录
add_subdirectory(src)
add_subdirectory(test)

#设置安装目录
set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/install)

#https://blog.csdn.net/bigdog_1027/article/details/79113342
# 编译出一个动态库 native-lib，源文件只有 src/main/cpp/native-lib.cpp
# 将指定的源文件生成链接文件，然后添加到工程中去
#add_library(<name> [STATIC | SHARED | MODULE]
#        [EXCLUDE_FROM_ALL]
#        [source1] [source2] [...])
add_library(
        hello-jni # so 库的名称 libhello-jni.so
        SHARED # 设置为分享库
        # 指定C源文件的路径
        ../../../cpp-src/hello-jni.c
)

find_library(
        log-lib # 设置路径变量名称
        log # 指定CMake需要加载的NDK库
)

# link_directories
#该指令的作用主要是指定要链接的库文件的路径，该指令有时候不一定需要。因为find_package和find_library指令可以得到库文件的绝对路径。不过你自己写的动态库文件放在自己新建的目录下时，可以用该指令指定该目录的路径以便工程能够找到。
#例子如下：
#link_directories(
#lib
#)


# target_link_libraries该指令的作用为将目标文件与库文件进行链接
#target_link_libraries(<target> [item1] [item2] [...]
#        [[debug|optimized|general] <item>] ...)
#上述指令中的<target>是指通过add_executable()和add_library()指令生成已经创建的目标文件。而[item]表示库文件没有后缀的名字。默认情况下，库依赖项是传递的。当这个目标链接到另一个目标时，链接到这个目标的库也会出现在另一个目标的连接线上。这个传递的接口存储在interface_link_libraries的目标属性中，可以通过设置该属性直接重写传递接口。
# 链接hello-jni库依赖的库，注意下面变量名的配置
target_link_libraries(hello-jni
#        ${android-lib}
        ${log-lib}
)





##子cmake
#递归获取当前目录及子目录下的所有c文件
file(GLOB_RECURSE c_files "*.c")

#生成动态库和静态库
add_library(helloworld_lib_shared  SHARED ${c_files})
add_library(helloworld_lib_static STATIC ${c_files})

#将动态库和静态库的名字设置为一致
set_target_properties(helloworld_lib_shared PROPERTIES OUTPUT_NAME "helloworld")
set_target_properties(helloworld_lib_static PROPERTIES OUTPUT_NAME "helloworld")

#设置动态库版本
set_target_properties(helloworld_lib_shared PROPERTIES VERSION 1.2 SOVERSION 1)

#安装动态库和静态库
INSTALL(TARGETS helloworld_lib_shared helloworld_lib_static
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

#安装头文件
INSTALL(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)
