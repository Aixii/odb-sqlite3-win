
set(CMAKE_VERBOSE_MAKEFILE ON)

cmake_minimum_required(VERSION 3.10)

# 项目名称和版本
project(odbtest VERSION 1.0 LANGUAGES CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)



# odb 路径
if(CMAKE_SIZEOF_VOID_P EQUAL 4) 
	set(ODB_PATH 3rdparty/odb-x86)
elseif(CMAKE_SIZEOF_VOID_P EQUAL 8) 
	set(ODB_PATH 3rdparty/odb-x64)
endif()

# ODB 编译器执行路径
file(GLOB ODB_EXEC ${ODB_PATH}/bin/odb.exe)

include_directories(${ODB_PATH}/include)

option(BUILD_DEBUG "构建Debug版本" ON)

if(BUILD_DEBUG)
    set(CMAKE_BUILD_TYPE "Debug")
    add_definitions(-D_DEBUG)
else()
    set(CMAKE_BUILD_TYPE "Release")
    add_definitions(-D_NODEBUG)
endif()

set(ODB_COMMAND_PARAMETER 
--generate-query 
--generate-session 
--generate-schema 
--generate-prepared 
-d sqlite 
--default-pointer std::tr1::shared_ptr)

# 生成 ODB 代码的源文件
add_custom_command(
    OUTPUT ${PROJECT_SOURCE_DIR}/employee-odb.hxx ${PROJECT_SOURCE_DIR}/employee-odb.ixx ${PROJECT_SOURCE_DIR}/employee-odb.cxx
	WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
    COMMAND ${ODB_EXEC} ${ODB_COMMAND_PARAMETER} employee.hxx 
    DEPENDS employee.hxx
    COMMENT "generate employee-odb"
)

# 定义一个目标来依赖上面的自定义命令
add_custom_target(odb_target
    DEPENDS employee-odb.ixx employee-odb.hxx employee-odb.cxx
)

# 添加源文件
file(GLOB SOURCES "*.cpp")
file(GLOB HEADER "*.h")
file(GLOB HEADER_HXX "*.hxx")
file(GLOB SOURCES_CXX "*.cxx")

add_definitions(-D_CRT_SECURE_NO_DEPRECATE)
add_definitions(-DDATABASE_SQLITE)
add_definitions(-wd4275)
add_definitions(-D_SILENCE_TR1_NAMESPACE_DEPRECATION_WARNING)

link_directories(${ODB_PATH}/lib)

# 生成可执行文件
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADER} ${HEADER_HXX} ${SOURCES_CXX} employee-odb.cxx)

# 确保 odb_target 在编译之前被执行
add_dependencies(${PROJECT_NAME} odb_target)


if(CMAKE_BUILD_TYPE MATCHES Debug)
    set(LIBRARY_SUFFIX "-d")
else()
    set(LIBRARY_SUFFIX "")
endif()

# 假设有一个第三方库在 lib/ 目录下
target_link_libraries(${PROJECT_NAME} PRIVATE odb-2.4${LIBRARY_SUFFIX} odb-sqlite3-2.4${LIBRARY_SUFFIX})

# 安装
unset(CMAKE_INSTALL_PREFIX CACHE)
set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/bin" CACHE STRING "")

message(STATUS ${CMAKE_INSTALL_PREFIX})

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})

install(FILES 
${ODB_PATH}/runtime/odb-2.4${LIBRARY_SUFFIX}.dll  
${ODB_PATH}/runtime/odb-sqlite3-2.4${LIBRARY_SUFFIX}.dll 
${ODB_PATH}/runtime/sqlite3${LIBRARY_SUFFIX}.dll 
DESTINATION ${CMAKE_INSTALL_PREFIX})