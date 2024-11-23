cmake_minimum_required(VERSION 3.2)
project(Codec LANGUAGES CXX C)

# Set C++17.
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

enable_testing()
add_subdirectory(deps/googletest)
include_directories(${GTEST_INCLUDE_DIRS})
add_subdirectory(deps/opencv)
SET(CMAKE_INCLUDE_PATH ${CMAKE_INCLUDE_PATH} "C:/Program Files/Boost/boost_1_86_0")
SET(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "C:/Program Files/Boost/boost_1_86_0/libs")

find_package(Boost)

# FFMPEG
find_path(AVCODEC_INCLUDE_DIR deps/FFmpeg/libavcodec/avcodec.h)
find_library(AVCODEC_LIBRARY deps/ffmpeg/libavcodec)
find_path(AVFORMAT_INCLUDE_DIR libavformat/avformat.h)
find_library(AVFORMAT_LIBRARY libavformat/)
find_path(AVUTIL_INCLUDE_DIR libavutil/avutil.h)
find_library(AVUTIL_LIBRARY libavutil)

message("aaaaaaaa-A-SD-AS-DA-SD" ${FFmpeg})

# Lib
add_library(${PROJECT_NAME}_lib SHARED
  src/H264Encoder.cpp
  src/H264Decoder.cpp
)

target_link_libraries(${PROJECT_NAME}_lib
  ${AVCODEC_LIBRARY}
  ${AVFORMAT_LIBRARY}
  ${AVUTIL_LIBRARY}
  ${Boost_LIBRARIES}
  -lboost_system
  -lpthread
)

# Example
add_executable(${PROJECT_NAME}_example
  example/Camera.cpp
)

target_link_libraries(${PROJECT_NAME}_example
  ${PROJECT_NAME}_lib
  ${OpenCV_LIBRARIES}
)

# Install.
install(TARGETS ${PROJECT_NAME}_lib
  ARCHIVE DESTINATION lib # ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION lib # ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION bin # ${CMAKE_INSTALL_BINDIR}
)

install(DIRECTORY include
  DESTINATION /usr/local/include
  FILES_MATCHING PATTERN "*.hpp*"
)
