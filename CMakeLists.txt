# Standard routine
cmake_minimum_required(VERSION 3.16)
project(project_template_prometheus)

# Tip: in a production project don't use branch name, use a specific name tag (i.e. v1.1.1),
# but for a sandbox project branch name is perfectly enough
set(AUI_VERSION master)

# Use AUI.Boot
file(
        DOWNLOAD
        https://raw.githubusercontent.com/aui-framework/aui/${AUI_VERSION}/aui.boot.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/aui.boot.cmake)
include(${CMAKE_CURRENT_BINARY_DIR}/aui.boot.cmake)

# import AUI
auib_import(aui https://github.com/aui-framework/aui
        COMPONENTS core curl crypt
        VERSION ${AUI_VERSION})

# import prometheus-cpp. it uses zlib and curl, and aui.boot would pull them from aui since aui uses them as well.
auib_import(prometheus-cpp https://github.com/jupp0r/prometheus-cpp
        VERSION v1.3.0)

# Create the executable. This function automatically links all sources from the src/ folder,
# creates CMake target and places the resulting executable to bin/ folder.
aui_executable(${PROJECT_NAME})

# Link required libs
aui_link(${PROJECT_NAME} PRIVATE aui::core prometheus-cpp::core prometheus-cpp::pull)
