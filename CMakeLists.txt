cmake_minimum_required(VERSION 3.10)
project(example_glfw_opengl2)

set(IMGUI_DIR ../..)

set(SOURCES
    main.cpp
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl2.cpp
)

add_executable(example_glfw_opengl2 ${SOURCES})

target_include_directories(example_glfw_opengl2 PRIVATE ${IMGUI_DIR} ${IMGUI_DIR}/backends)

if(UNIX AND NOT APPLE)
    # Linux
    set(GLFW_BUILD_DIR "/home/runner/work/imgui-docking1.90.1glfw_opengl2/imgui-docking1.90.1glfw_opengl2/build-null-shared")

    find_path(GLFW_INCLUDE_DIR GLFW/glfw3.h
        HINTS ${GLFW_BUILD_DIR}/include
        PATH_SUFFIXES glfw
    )

    find_library(GLFW_LIBRARY
        NAMES glfw3
        HINTS ${GLFW_BUILD_DIR}/lib
    )

    if(NOT GLFW_INCLUDE_DIR OR NOT GLFW_LIBRARY)
        message(FATAL_ERROR "GLFW3 not found. Set GLFW_BUILD_DIR to the path where GLFW is built.")
    endif()

    target_include_directories(example_glfw_opengl2 PRIVATE ${GLFW_INCLUDE_DIR})
    target_link_libraries(example_glfw_opengl2 PRIVATE ${GLFW_LIBRARY} -lGL)
    target_compile_options(example_glfw_opengl2 PRIVATE -pthread)
endif()

    elseif(APPLE)
        # macOS
        find_package(glfw3 REQUIRED)
        target_link_libraries(example_glfw_opengl2 PRIVATE glfw)
        target_compile_options(example_glfw_opengl2 PRIVATE -framework OpenGL -framework Cocoa -framework IOKit -framework CoreVideo)
else()
    # Windows
    target_link_libraries(example_glfw_opengl2 PRIVATE glfw3 gdi32 opengl32 imm32)
    target_compile_options(example_glfw_opengl2 PRIVATE ${GLFW_CFLAGS_OTHER})
endif()

set_target_properties(example_glfw_opengl2 PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)

message("Build complete for ${CMAKE_SYSTEM_NAME}")
