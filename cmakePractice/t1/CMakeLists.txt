#测试方法
#构建工程 ：在t1目录下执行：cmake .
#           系统会自动生成：CMakeFiles, CMakeCache.txt, cmake_install.cmake等文件，并且生成了Makefile,
#           你可以不去理会这些文件，最关键的是它自动生成了Makefile
#实际构建： 在t1目录下执行：make
#           

#COMMAND:PROJECT(projectname [CXX] [C] [Java])
#定义：定义工程名称，并可指定工程支持的语言（可忽略，默认情况支持所有语言）
#备注：这个指令隐式定义了2个cmake变量：<projectname>_BINARY_DIR, <projectname>_SOURCE_DIR
#      所以MESSAGE指令里可以直接用这2个变量,在内部编译下两者目录相同，均指向当前CMakelists.txt所在目录。
#      还帮我们预定义了另2个变量：PROJECT_BINARY_DIR(=<projectname>_BINARY_DIR), 
#                                 PROJECT_SOURCE_DIR(-<projectname>_SOURCE_DIR)
#      推荐使用后2个变量，即使修改了工程名称，也不会修改影响到这两个变量了。
PROJECT(HELLO)

#COMMAND:SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
#定义：用来显式的定义变量
SET(SRC_LIST main.cpp)

#COMMAND:MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display"...)
#定义：向终端输出用户自定义的信息
#      SEND_ERRO:   产生错误，生成过程被跳过
#      STATUS:      输出前缀为-的信息
#      FATAL_ERROR：立即终止所有cmake过程
MESSAGE(STATUS "This is BINARY dir" ${HELLO_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir" ${HELLO_SOURCE_DIR})

#COMMAND:ADD_EXECUTABLE
#定义：定义工程生成的可执行文件，以及它所依赖的相关的源文件列表
ADD_EXECUTABLE(hello ${SRC_LIST})


#附:
#1)cmake基本语法规则
#1.1）变量使用${}方式取值，但是在IF控制语句中要直接使用变量名
#1.2）指令（参数1 参数2...)
#     参数用()括起来，参数之间用空格或分号分开
#1.3）指令是大小写无关，参数和变量是大小写相关的。推荐全部使用大写指令。

#2)语法相关
#SET(SRC_LIST main.c) == SET(SRC_LIST "main.c")
#SET(SRC_LIST fun nc.c) != SET(SRC_LIST "fun nc.c") :后者的""内的字符会被当成1个文件，

#3)内部编译vs外部编译
#    在CMakeLists.txt当前目录下执行"cmake .",属于内部编译
#    它生成的临时文件和工程文件在同一目录下，删除起来非常不方便

#     不在CMakeLists.txt当前目录下执行“cmake ../<path_to_CMakeLists_Dir> 属于外部编译
#     也就是out-of-source外部编译，所有动作全部发生在编译目录，对原有工程没有任何影响，推荐！！！
#     比如在t1目录下创建build目录，进到build目录下执行："cmake ../",  此次编译过程便为外部编译
#     注：此时HELLO_SOURCE_DIR仍指工程路径：../cmake/t1(与CMakeLists.txt同级)
#            HELLO_BINARY_DIR则指编译路径：../cmake/t1/build(与当前cmake执行命令所在的目录同级)