cmake_minimum_required(VERSION 3.7)

project(vsgQtViewer
    VERSION 0.0.0
    DESCRIPTION "Qt Viewer for VulkanSceneGraph"
    LANGUAGES CXX
)
set(vsgQtViewer_RELEASE_CANDIDATE 0)

# set the use of C++17 globally as all examples require it
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(vsg REQUIRED)
find_package(Qt5 COMPONENTS Widgets REQUIRED)

vsg_setup_build_vars()
vsg_setup_dir_vars()

vsg_add_target_clang_format(
    FILES
        src/*.cpp
)
vsg_add_target_clobber()
vsg_add_target_cppcheck(
    FILES
        src/*.h
        src/*.cpp
)
vsg_add_target_docs(
    FILES
        src
)
vsg_add_target_uninstall()

vsg_add_option_maintainer(
    PREFIX ${PROJECT_NAME}
    RCLEVEL ${PROJECT_NAME}_RELEASE_CANDIDATE
)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(SOURCES
    src/main.cpp
    src/MainWindow.cpp
    src/VulkanWindow.cpp
)

set(HEADERS
    src/MainWindow.h
    src/VulkanWindow.h
)

set(FORMS
    src/MainWindow.ui
)

if(WIN32)
    add_definitions(-DVK_USE_PLATFORM_WIN32_KHR)
    set(MODE WIN32)
elseif(UNIX)
    add_definitions(-DVK_USE_PLATFORM_XCB_KHR)
else()
    add_definitions(-DVK_USE_PLATFORM_XLIB_KHR)
endif()

add_executable(vsgQtViewer ${MODE} ${SOURCES} ${HEADERS} ${FORMS})
target_link_libraries(vsgQtViewer
    PRIVATE
        vsg::vsg
        Qt5::Widgets
)

install(TARGETS vsgQtViewer
        RUNTIME DESTINATION bin
)

vsg_add_feature_summary()
