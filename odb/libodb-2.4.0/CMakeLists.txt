
set(TARGET_NAME odb-2.4)

# 查找所有源文件
file(GLOB  SOURCES 
${CMAKE_CURRENT_LIST_DIR}/odb/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/meta/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/shared-ptr/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/win32/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/tr1/*.cxx
)

file(GLOB HEADERS 
${CMAKE_CURRENT_LIST_DIR}/odb/*.hxx 
${CMAKE_CURRENT_LIST_DIR}/odb/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/details/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/details/*.hxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/*.
${CMAKE_CURRENT_LIST_DIR}/odb/details/meta/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/details/meta/*.hxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/shared-ptr/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/details/shared-ptr/*.hxx
${CMAKE_CURRENT_LIST_DIR}/odb/details/win32/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/details/win32/*.hxx
${CMAKE_CURRENT_LIST_DIR}/odb/tr1/*.txx
${CMAKE_CURRENT_LIST_DIR}/odb/tr1/*.hxx
${CMAKE_CURRENT_LIST_DIR}/odb/compilers/vc/*.hxx
)

# 添加宏定义
add_definitions(-DLIBODB_DYNAMIC_LIB)
add_definitions(-wd4275)
add_definitions(-wd4290)

set(CMAKE_DEBUG_POSTFIX "-d")

# 包含头文件
include_directories(${CMAKE_CURRENT_LIST_DIR})

add_library(${TARGET_NAME} SHARED ${SOURCES} ${HEADERS})

install(TARGETS ${TARGET_NAME} 
RUNTIME DESTINATION runtime
ARCHIVE DESTINATION lib)

file(GLOB INSTALL_INCLUDE_FILES 
odb/*.hxx 
odb/*.txx 
odb/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb)

file(GLOB INSTALL_INCLUDE_FILES 
odb/details/*.hxx 
odb/details/*.txx 
odb/details/*.ixx
odb/details/*.h)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/details)

file(GLOB INSTALL_INCLUDE_FILES 
odb/details/meta/*.hxx 
odb/details/meta/*.txx 
odb/details/meta/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/details/meta)

file(GLOB INSTALL_INCLUDE_FILES 
odb/details/shared-ptr/*.hxx 
odb/details/shared-ptr/*.txx 
odb/details/shared-ptr/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/details/shared-ptr)

file(GLOB INSTALL_INCLUDE_FILES 
odb/details/win32/*.hxx 
odb/details/win32/*.txx 
odb/details/win32/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/details/win32)

file(GLOB INSTALL_INCLUDE_FILES 
odb/tr1/*.hxx 
odb/tr1/*.txx 
odb/tr1/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/tr1)

file(GLOB INSTALL_INCLUDE_FILES odb/compilers/vc/*.hxx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/compilers/vc)