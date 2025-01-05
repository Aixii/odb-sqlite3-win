
cmake_minimum_required(VERSION 3.10)

project(odb-sqlite3 VERSION 1.0 LANGUAGES CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if(CMAKE_SIZEOF_VOID_P EQUAL 4) 
	set(CMAKE_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX}/odb-x86)
elseif(CMAKE_SIZEOF_VOID_P EQUAL 8) 
	set(CMAKE_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX}/odb-x64)
endif()

# 包含子目录
add_subdirectory(libodb-2.4.0)
add_subdirectory(sqlite-amalgamation-3470200)
add_subdirectory(libodb-sqlite-2.4.0)

# 配置安装路径
install(DIRECTORY odb-2.4.0-i686-windows/bin DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY odb-2.4.0-i686-windows/etc DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY odb-2.4.0-i686-windows/man DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY odb-2.4.0-i686-windows/mingw DESTINATION ${CMAKE_INSTALL_PREFIX})