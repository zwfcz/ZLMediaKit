#查找SDL2是否安装
find_package(SDL2 QUIET)
if (SDL2_FOUND)
    include_directories(${SDL2_INCLUDE_DIR})
    list(APPEND LINK_LIB_LIST ${SDL2_LIBRARY})
    message(STATUS "found SDL2:${SDL2_INCLUDE_DIR},${SDL2_LIBRARY}")
endif (SDL2_FOUND)

#查找ffmpeg/libutil是否安装
find_package(AVUTIL QUIET)
if(AVUTIL_FOUND)
    include_directories(${AVUTIL_INCLUDE_DIR})
    list(APPEND  LINK_LIB_LIST ${AVUTIL_LIBRARIES})
    message(STATUS "found  libutil ${AVUTIL_LIBRARIES}")
endif()

#查找ffmpeg/libavcodec是否安装
find_package(AVCODEC QUIET)
if(AVCODEC_FOUND)
    include_directories(${AVCODEC_INCLUDE_DIR})
    list(APPEND  LINK_LIB_LIST ${AVCODEC_LIBRARIES})
    message(STATUS "found  libavcodec ${AVCODEC_LIBRARIES}")
endif()

aux_source_directory(. TEST_SRC_LIST)
#如果ffmpeg/libavcodec ffmpeg/libavcodec SDL 都安装了则编译 test_player
if(SDL2_FOUND AND AVCODEC_FOUND AND AVUTIL_FOUND)
    message(STATUS "test_player will be compiled")
else()
    message(STATUS "test_player ingored, please install sdl2 ffmpeg/libavcodec ffmpeg/libavutil")
    list(REMOVE_ITEM TEST_SRC_LIST ./test_player.cpp)
endif()

foreach(TEST_SRC ${TEST_SRC_LIST})
    STRING(REGEX REPLACE "^\\./|\\.c[a-zA-Z0-9_]*$" "" TEST_EXE_NAME ${TEST_SRC})
    message(STATUS "add test:${TEST_EXE_NAME}")
    add_executable(${TEST_EXE_NAME} ${TEST_SRC})
    target_link_libraries(${TEST_EXE_NAME} ${LINK_LIB_LIST})
endforeach()













