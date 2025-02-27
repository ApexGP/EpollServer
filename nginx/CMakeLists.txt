cmake_minimum_required(VERSION 3.10)

# 项目名称
project(EpollServer)

# 启用生成 compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 定义需要构建的子目录（按顺序）
set(MAKEFILES
    "nginx/signal"
    "nginx/proc"
    "nginx/net"
    "nginx/misc"
    "nginx/logic"
    "nginx/app"
)

# 遍历每个子目录，并执行 `make` 命令
foreach(MAKEFILE_DIR IN LISTS MAKEFILES)
  # 去除目录中的斜杠，替换为下划线
  string(REPLACE "/" "_" TARGET_NAME "build_${MAKEFILE_DIR}")

  add_custom_target(
        ${TARGET_NAME}
        COMMAND make -C ${CMAKE_SOURCE_DIR}/${MAKEFILE_DIR}
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    )
endforeach()

# 定义一个最顶层的目标，依赖于所有子目录的构建
add_custom_target(build_all DEPENDS build_nginx_signal build_nginx_proc
                  build_nginx_net build_nginx_misc build_nginx_logic
                  build_nginx_app)



