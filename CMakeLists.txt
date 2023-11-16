cmake_minimum_required(VERSION 3.21)

project(
  OpenCV-C
)

if(PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  message(
    FATAL_ERROR
      "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there."
  )
endif()

set(CMAKE_EXPORT_COMPILE_COMMANDS True)

include(cmake/CPM.cmake)

CPMAddPackage(
  NAME opencv
  GITHUB_REPOSITORY opencv/opencv
  GIT_TAG 4.8.1
  VERSION 4.8.1
  OPTIONS
    "BUILD_SHARED_LIBS OFF"
    "BUILD_JASPER ON"
    "BUILD_JPEG ON"
    "WITH_OPENEXR ON"
    "BUILD_OPENEXR ON"
    "BUILD_PNG ON"
    "BUILD_TBB ON"
    "BUILD_TIFF ON"
    "BUILD_ZLIB ON"
    "BUILD_EXAMPLES OFF"
    "BUILD_TESTS OFF"
    "BUILD_DOCS OFF"
    "BUILD_TESTS OFF"
    "BUILD_PERF_TESTS OFF"
    "WITH_1394 OFF"
    "WITH_VTK OFF"
    "WITH_CUDA OFF"
    "WITH_EIGEN OFF"
    "WITH_GSTREAMER OFF"
    "WITH_GTK ON"
    "WITH_GTK_2_X ON"
    "WITH_JPEG ON"
    "WITH_PNG ON"
    "WITH_WEBP ON"
    "WITH_QT OFF"
    "WITH_QUICKTIME OFF"
    "WITH_V4L ON"
    "WITH_XINE OFF"
    "WITH_GDAL OFF"
    "WITH_GPHOTO2 ON"
    "WITH_IPP OFF"
    "BUILD_opencv_python2 OFF"
    "BUILD_opencv_python3 OFF"
    "BUILD_EXAMPLES OFF"
    "BUILD_opencv_apps OFF"
    "INSTALL_C_EXAMPLES OFF"
    "INSTALL_PYTHON_EXAMPLES=OFF"
)

#CPMAddPackage("gh:opencv/opencv#4.8.1")

file(GLOB_RECURSE headers CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/Source/*.h")
file(GLOB_RECURSE sources CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/Source/*.cpp")

add_library(${PROJECT_NAME} SHARED ${sources})
target_include_directories(${PROJECT_NAME}
  PUBLIC
  "${CMAKE_CURRENT_SOURCE_DIR}/Source"

  PRIVATE
  ${opencv_SOURCE_DIR}/include
  ${opencv_SOURCE_DIR}/modules/core/include
  ${opencv_SOURCE_DIR}/modules/calib3d/include
  ${opencv_SOURCE_DIR}/modules/features2d/include
  ${opencv_SOURCE_DIR}/modules/dnn/include
  ${opencv_SOURCE_DIR}/modules/dnn/src/tengine4dnn/include
  ${opencv_SOURCE_DIR}/modules/dnn/src/vkcom/include
  ${opencv_SOURCE_DIR}/modules/dnn/src/ocl4dnn/include
  ${opencv_SOURCE_DIR}/modules/stitching/include
  ${opencv_SOURCE_DIR}/modules/world/include
  ${opencv_SOURCE_DIR}/modules/gapi/include
  ${opencv_SOURCE_DIR}/modules/imgcodecs/include
  ${opencv_SOURCE_DIR}/modules/highgui/include
  ${opencv_SOURCE_DIR}/modules/objdetect/include
  ${opencv_SOURCE_DIR}/modules/video/include
  ${opencv_SOURCE_DIR}/modules/videoio/include
  ${opencv_SOURCE_DIR}/modules/core/include
  ${opencv_SOURCE_DIR}/modules/features2d/include
  ${opencv_SOURCE_DIR}/modules/calib3d/include
  ${opencv_SOURCE_DIR}/modules/ml/include
  ${opencv_SOURCE_DIR}/modules/flann/include
  ${opencv_SOURCE_DIR}/modules/ts/include
  ${opencv_SOURCE_DIR}/modules/imgproc/include
  ${opencv_SOURCE_DIR}/modules/photo/include
  ${PROJECT_BINARY_DIR})
target_link_libraries(${PROJECT_NAME}
  PRIVATE
  -Wl,--whole-archive
  opencv_stitching
  opencv_photo
  opencv_objdetect
  opencv_calib3d
  opencv_highgui
  opencv_videoio
  opencv_imgcodecs
  opencv_imgproc
  opencv_core
  -Wl,--no-whole-archive)
set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 17)

add_executable(demo Demo/Demo.c)
target_link_libraries(demo ${PROJECT_NAME})
