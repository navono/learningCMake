PROJECT(hello)

#标识此目录下的工程文件将会生成可执行二进制
ADD_EXECUTABLE(${PROJECT_NAME} main.cpp)  

#设置最终文件的输出目录
#只是用于最终生成文件
#且此处目录指的是：当前CMakeLists.txt所在目录在编译路径同级目录下再创建子目录bin
#（可依据情况是否选择此功能）
#SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

#设置最终文件的安装目录
INSTALL(TARGETS ${PROJECT_NAME}
        DESTINATION bin)