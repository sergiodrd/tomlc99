cmake_minimum_required(VERSION 3.20.2)

project(tomlc99 C)

set(CMAKE_C_STANDARD 99)

if (WIN32 AND BUILD_SHARED_LIBS)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif ()

option(TOMLC99_BUILD_PROGRAMS "Build tomlc99 programs." ON)
option(TOMLC99_BUILD_TESTS "Build tomlc99 unit tests." OFF)

set(${PROJECT_NAME}_SOURCES
        ${CMAKE_CURRENT_LIST_DIR}/include/toml.h
        ${CMAKE_CURRENT_LIST_DIR}/src/toml.c)

add_library(${PROJECT_NAME} ${${PROJECT_NAME}_SOURCES})

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_LIST_DIR}/include)

if (TOMLC99_BUILD_PROGRAMS)

    add_executable(toml_cat
            ${${PROJECT_NAME}_SOURCES}
            ${CMAKE_CURRENT_LIST_DIR}/src/toml_cat.c)

    target_link_libraries(toml_cat PUBLIC ${PROJECT_NAME})

    add_executable(toml_json
            ${${PROJECT_NAME}_SOURCES}
            ${CMAKE_CURRENT_LIST_DIR}/src/toml_json.c)

    target_link_libraries(toml_json PUBLIC ${PROJECT_NAME})

endif ()

if (TOMLC99_BUILD_TESTS)

    add_executable(t1
            ${${PROJECT_NAME}_SOURCES}
            ${CMAKE_CURRENT_LIST_DIR}/unittest/t1.c)

    target_link_libraries(t1 PUBLIC ${PROJECT_NAME})

endif ()
