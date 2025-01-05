set(TARGET_NAME odb-sqlite3-2.4)

# 查找所有源文件
file(GLOB  SOURCES 
${CMAKE_CURRENT_LIST_DIR}/odb/sqlite/*.cxx
${CMAKE_CURRENT_LIST_DIR}/odb/sqlite/details/*.cxx
)

file(GLOB HEADERS 
${CMAKE_CURRENT_LIST_DIR}/odb/sqlite/*.hxx 
${CMAKE_CURRENT_LIST_DIR}/odb/sqlite/details/*.txx
)

# 添加宏定义
add_definitions(-DLIBODB_SQLITE_DYNAMIC_LIB)
add_definitions(-DLIBODB_SQLITE_HAVE_UNLOCK_NOTIFY)
add_definitions(-wd4275)
add_definitions(-wd4273)
add_definitions(-wd4290)

set(CMAKE_DEBUG_POSTFIX "-d")

# 包含头文件
include_directories(${CMAKE_CURRENT_LIST_DIR})
include_directories(${CMAKE_CURRENT_LIST_DIR}/../sqlite-amalgamation-3470200)

add_library(${TARGET_NAME} SHARED ${SOURCES} ${HEADERS})

target_link_libraries(${TARGET_NAME} PRIVATE odb-2.4 sqlite3)

add_dependencies(${TARGET_NAME} odb-2.4 sqlite3)

install(TARGETS ${TARGET_NAME} 
RUNTIME DESTINATION runtime
ARCHIVE DESTINATION lib)

file(GLOB INSTALL_INCLUDE_FILES 
odb/sqlite/*.hxx 
odb/sqlite/*.txx 
odb/sqlite/*.ixx)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/sqlite)

file(GLOB INSTALL_INCLUDE_FILES 
odb/sqlite/details/*.hxx
odb/sqlite/details/*.txx
odb/sqlite/details/*.ixx
)
install(FILES ${INSTALL_INCLUDE_FILES}	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/odb/sqlite/details)