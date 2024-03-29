cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(SimpleKV)

#. . . . . . . . . . . . . . . . . . . .
include(GNUInstallDirs)
if(MSVC)
    add_compile_options(/W4)
    add_link_options(/W4)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin)
else()
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endif()


# place binaries and libraries according to GNU standards
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})



if(CMAKE_CXX_COMPILER_ID MATCHES GNU)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")
endif()
#. . . . . . . . . . . . . . . . . . . .

# ------------------------------------
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)

# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# GoogleTest requires at least C++11
set(CMAKE_CXX_STANDARD 14)
 
enable_testing() 


# ------------------------------------



#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
#   Grab all cpp files from includes folder
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
set(INCLUDES_FOLDER includes)
FILE(GLOB_RECURSE SOURCE_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}   "${INCLUDES_FOLDER}/*.cpp" )
#- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 



ADD_EXECUTABLE(main
    main.cpp
    ${SOURCE_FILES}
)

ADD_EXECUTABLE(test_avl
    _tests/_test_files/test_avl.cpp
    ${SOURCE_FILES}
)

ADD_EXECUTABLE(test_hash
    _tests/_test_files/test_hash.cpp
    ${SOURCE_FILES}
)

ADD_EXECUTABLE(test_fileIO
    _tests/_test_files/test_fileIO.cpp
    ${SOURCE_FILES}
)

ADD_EXECUTABLE(test_serializer
    _tests/_test_files/test_serializer.cpp
    ${SOURCE_FILES}
)

ADD_EXECUTABLE(test_simpleKV
    _tests/_test_files/test_simpleKV.cpp
    ${SOURCE_FILES}
)

TARGET_LINK_LIBRARIES(test_avl gtest)
TARGET_LINK_LIBRARIES(test_hash gtest)
TARGET_LINK_LIBRARIES(test_fileIO gtest)
TARGET_LINK_LIBRARIES(test_serializer gtest)
TARGET_LINK_LIBRARIES(test_simpleKV gtest)