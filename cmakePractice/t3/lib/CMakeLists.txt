SET(LIBHELLO_SRC hello.cpp)

#1)注意: 下面2句不能同时出现，因为target名称是唯一的，如果想要得到相同名称的动态库和静态库
#必须借助于SET_TARGET_PROPERTIES
ADD_LIBRARY(hello SHARED ${LIBHELLO_SRC})
#ADD_LIBRARY(hello STATIC ${LIBHELLO_SRC})


#2)注意：有了以下两句，我们就可以同时得到同名的动态库和静态库了,
#      但是你会发现libhello.so却消失了，因为cmake在构建一个新的target时，
#      会尝试清理掉其它使用这个名字的库，所以在构建libhello.a时，就会清理掉libhello.so
ADD_LIBRARY(hello_static STATIC ${LIBHELLO_SRC})
SET_TARGET_PROPERTIES(hello_static PROPERTIES OUTPUT_NAME "hello")

#3)要避免同名不同类型的库覆盖，需要再借助于SET_TARGET_PROPERTIES指令
SET_TARGET_PROPERTIES(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)
SET_TARGET_PROPERTIES(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

#4)增加动态库版本号
#   按照规则，动态库是应该包含一个版本号的，我们看一下系统的动态库，一般情况是：
#   libhello.so.1.2
#   libhello.so->libhello.so.1
#   libhello.so.1->libhello.so.1.2
#   以下命令中，VERSION指代动态库版本，SOVERSION指代API版本
SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)

#5)安装共享库和头文件
#   hello的共享库安装到<prefix>/lib目录
#   hello.h安装到<prefix>/include/hello目录
INSTALL(TARGETS hello hello_static
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

INSTALL(FILES hello.h DESTINATION include/hello)



#COMMAND: ADD_LIBRARY(libname [SHARED|STATIC|MODULE] [EXECLUDE FROM_ALL]
#             source1 source2 ... sourceN)
#注意：不需要写全libhello.so，只需要填写hello即可，cmake系统会自动为你生成libhello.X
#       类型有3种：SHARED(动态库)，
#                  STATIC(静态库),
#                  MODULE(在使用dyld的系统有效，否则被当作SHARED对待)
#       EXCLUDE_FROM_ALL:这个库不会被默认构建，除非有其它的组件依赖或者手工构建

#COMMAND: SET_TARGET_PROPERTIES(target1 target2 ...
#                               PROPERTIES prop1 value1 prop2 value2 ...)
#定义：用来设置输出的名称，对于动态库，还可以用来指定动态库版本和API版本。
#备注：其它对应的指令是：GET_TARGET_PROPERTIES(VAR target property)
#      如：GET_TARGET_PROPERTY(OUTPUT_VALUE hello_static OUTPUT_NAME)
#          MESSAGE(STATUS "This is the hello_static OUTPUT_NAME:"${OUTPUT_VALUE})
#          如果没有这个属性定义，则返回NOTFOUND