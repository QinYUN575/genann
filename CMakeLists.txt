cmake_minimum_required(VERSION 3.10)
project(Genann)

# 设置C标准
set(CMAKE_C_STANDARD 99)

# 编译器标志
add_compile_options(-Wall -Wshadow -O3 -g -march=native)

# 找到数学库
find_library(M_LIB m)

# 定义源文件
set(SOURCE_FILES genann.c)

# 添加可执行文件
add_executable(example1 example1.c ${SOURCE_FILES})
add_executable(example2 example2.c ${SOURCE_FILES})
add_executable(example3 example3.c ${SOURCE_FILES})
add_executable(example4 example4.c ${SOURCE_FILES})
add_executable(test test.c ${SOURCE_FILES})

# 链接数学库
if(M_LIB)
  target_link_libraries(example1 ${M_LIB})
  target_link_libraries(example2 ${M_LIB})
  target_link_libraries(example3 ${M_LIB})
  target_link_libraries(example4 ${M_LIB})
  target_link_libraries(test ${M_LIB})
endif()

# 添加自定义目标以支持不同的激活函数
add_custom_target(sigmoid COMMAND ${CMAKE_COMMAND} -Dgenann_act=genann_act_sigmoid_cached .)
add_custom_target(threshold COMMAND ${CMAKE_COMMAND} -Dgenann_act=genann_act_threshold .)
add_custom_target(linear COMMAND ${CMAKE_COMMAND} -Dgenann_act=genann_act_linear .)

# 添加测试
add_custom_target(check COMMAND test)

# 清理命令
set_property(DIRECTORY PROPERTY ADDITIONAL_MAKE_CLEAN_FILES "persist.txt")
