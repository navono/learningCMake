#测试方法
#在t2/build目录下，执行"cmake -DCMAKE_INSTALL_PREFIX=//home/TOM/tmp .."
cmake_minimum_required(VERSION 3.2)
PROJECT(HELLO)

#COMMAND: ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
#定义：向当前工程添加存放源文件的子目录, 同时可以指定中间二进制和目标二进制存放的位置
#      EXCLUDE_FROM_ALL：将这个目录从编译过程中排除，
#      比如工程的example，可能就需要工程构建完成后，再进入example目录单独进行构建; 当然，你也可以通过定义依赖来解决此类问题
#      举例：ADD_SUBDIRECTORY(src bin)
#      将src子目录加入工程，并指定编译输出（包含编译中间结果）路径为bin目录。
#      如果不指定，编译结果(包括中间结果)都将存放在build/src目录（这个目录与原有的src目录对应），
#      指定bin目录后，相当于在编译时将src重命名为bin，所有编译结果都放在bin目录。
#     
ADD_SUBDIRECTORY(src bin)


#附：
#1)换个地方保存目标二进制
#    不论是SUBDIRS还是ADD_SUBDIRECTORY，我们都可以通过SET重新定义EXECUTABLE_OUTPUT_PATH和
#    LIBRARY_OUTPUT_PATH变量来指定最终的目标二进制的位置（指最终生成的hello或者最终的共享库，不含编译生成的是间文件）
#    SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
#    //只是用于最终生成文件
#    //且此处目录指的是：当前CMakeLists.txt所在目录在编译路径同级目录下再创建子目录bin
#    SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
#备注：
#   Q: 把上面两个指令放在哪里? 
#   A: 在哪里ADD_EXECUTABLE或ADD_LIBRARY, 如果需要改变目标存放路径，就在那里使用这两个指令，这个例子当然指的是src下面的CMakeLists.txt

#3)如何安装
#    一般情况下会用到指令(INSTALL)和变量(CMAKE_INSTALL_PREFIX)
#COMMAND: INSTALL:
#定义：用于定义安装规则。 包括多种类型：有目标二进制，动态库，静态库，文件，目录和脚本等等
#3.1)目标文件的安装
#    INSTALL(TARGETS targets...
#            [ [ARCHIVE | LIBRARY |RUNTIME] 
#              [DESTINATION <dir>]
#              [PERMISSIONS permissions ...]
#              [CONFIGURATIONS [Debug|Release|...]]
#              [COMPONENT <component>]
#              [OPTIONAL]
#            ] [...])
#     说明：TARGETS:
#                后面跟的就是我们通过ADD_EXECUTABLE或ADD_LIBRARY定义的目标文件，
#                可能是：可执行二进制(RUNTIME)，动态库(LIBRARY)，静态库(ARCHIVE)
#           DESTINATION:
#                定义安装的路径，${CMAKE_INSTALL_PREFIX}/<DESTINATION定义的路径>
#                如果DESTINATION以"/"开头，则指绝对路径,此时CMAKE_INSTALL_PREFIX无效
#                如果希望使用CMAKE_INSTALL_PREFIX来定义安装路径，就要写成相对路径，不以/开头
#                     
#     例子：INSTALL(TARGETS myrun mylib mystaticlib
#                   RUNTIME DESTINATION bin
#                   LIBRARY DESTINATION lib
#                   ARCHIVE DESTINATION libstatic
#                  )
#            解释：可执行二进制myrun安装到${CMAKE_INSTALL_PREFIX}/bin目录
#                  动态库mylib安装到${CMAKE_INSTALL_PREFIX}/lib目录
#                  静态库mystaticlib安装到${CMAKE_INSTALL_PREFIX}/libstatic目录
#                  (注：不需要关心TARGETS具体生成的路径，只要写上TARGETS名称就可以了)

#3.2)非目标文件的可执行程序安装（比如脚本这类）
#    INSTALL(PROGRAMS files...
#             DESTINATION <dir>
#             [以下参数与FILES相同，但默认权限是755])

#3.3)普通文件的安装
#    INSTALL(FILES file...
#            DESTINATION <dir>
#            [PERMISSIONS permissions...]
#            [CONFIGURATIONS [Debug|Release|...]]
#            [COMPONENT <component>]
#            [RENAME <name>] [OPTIONAL])
#    说明：用一安装一般文件，并可以指定访问权限，文件名是此指令所在路径下的树路径，默认权限644

#3.4)目录的安装
#    INSTALL(DIRECTORY dirs...
#            DESTINATION <dir>
#            [FILE_PERMISSIONS permissions...]
#            [DIRECTORY_PERMISSIONS permisssions...]
#            [USE_SOURCE_PERMISSIONS]
#            [CONFIGURATIONS [Debug|Release|...]]
#            [COMPONENT <component>]
#            [ [PATTERN <pattern> | REGEX <regex>] [EXCLUDE] [PERMISSIONS permissions...]]
#            [...] ]
#     说明：这里主要介绍DIRECTORY,PATTERN,PERMISSIONS参数
#           DIRECTORY: 后面接的是所在Source目录的相对路径，但要注意abc 和 abc/区别很大
#           前者的目录将被安装为目标路径下的abc，后者目录下的内容会被安装到目标路径下（不包含目录本身）
#           PATTERN: 用于正则静态式过滤
#           PERMISSIONS: 用于指定PATTERN过滤后的文件权限
#     例子：INSTALL(DIRECTORY icons scripts/ 
#                   DESTINATION share/myproj
#                   PATTERN "CVS" EXCLUDE 
#                   PATTERN "scripts/*"
#                   PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ)
#      解释：将icons目录安装到<prefix>/share/myproj
#            将scripts/中的内容安装到<prefix>/share/myproj,
#            不包含目录名为CVS的目录，对于scripts/*文件指定权限为:OWNER_EXECUTE ......
#
#3.5) 安装CMAKE脚本的执行
#    INSTALL([[SCRIPT <file>] [CODE <code>] ] [...])
#    SCRIPT：用于在安装时调用cmake脚本文件（也就是<abc>.cmake文件）
#    CODE:   用于执行CMAKE命令，必须以双引号括起来，比如：
#    INSTALL(CODE "MESSAGE(\"Sample install message.\")")
#
         
#安装指令
INSTALL(FILES COPYRIGHT README 
        DESTINATION share/doc/cmake/t2)

INSTALL(PROGRAMS runhello.sh run.sh
        DESTINATION bin)

INSTALL(DIRECTORY doc/ 
        DESTINATION share/doc/cmake/t2)