cmake_minimum_required(VERSION 3.24)
# Add your project name
project(YOUR_PROJECT_NAME)

set(CMAKE_CXX_STANDARD 17)

# Packages versions
set(SFML_VERSION 2.6.0)
set(IMGUI_VERSION 1.87)
set(IMGUI_SFML_VERSION 2.5)

# SFML
include(FetchContent)
FetchContent_Declare(SFML
        GIT_REPOSITORY https://github.com/SFML/SFML.git
        GIT_TAG 2.6.x)
option(SFML_BUILD_AUDIO "Build audio" OFF)
option(SFML_BUILD_NETWORK "Build network" OFF)
FetchContent_MakeAvailable(SFML)

# ImGUI
FetchContent_Declare(
        imgui
        URL "https://github.com/ocornut/imgui/archive/v${IMGUI_VERSION}.zip"
)
FetchContent_MakeAvailable(imgui)

# ImGUI-SFML
FetchContent_Declare(
        imgui-sfml
        URL "https://github.com/SFML/imgui-sfml/archive/v${IMGUI_SFML_VERSION}.zip"
)
set(IMGUI_DIR ${imgui_SOURCE_DIR})
option(IMGUI_SFML_FIND_SFML "Use find_package to find SFML" OFF)
option(IMGUI_SFML_IMGUI_DEMO "Build imgui_demo.cpp" OFF)
FetchContent_MakeAvailable(imgui-sfml)

add_executable(YOUR_PROJECT_NAME main.cpp)

# ImGUI-SFML
target_link_libraries(YOUR_PROJECT_NAME PUBLIC ImGui-SFML::ImGui-SFML)
# SFML
target_link_libraries(YOUR_PROJECT_NAME PRIVATE sfml-graphics)
target_compile_features(YOUR_PROJECT_NAME PRIVATE cxx_std_17)
if (WIN32 AND BUILD_SHARED_LIBS)
    add_custom_command(TARGET YOUR_PROJECT_NAME POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_RUNTIME_DLLS:YOUR_PROJECT_NAME> $<TARGET_FILE_DIR:YOUR_PROJECT_NAME> COMMAND_EXPAND_LISTS)
endif()
install(TARGETS YOUR_PROJECT_NAME)