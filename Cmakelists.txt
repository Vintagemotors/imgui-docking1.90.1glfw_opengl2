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
    find_package(PkgConfig REQUIRED)
    pkg_search_module(GLFW REQUIRED glfw3)
    target_link_libraries(example_glfw_opengl2 PRIVATE ${GLFW_LIBRARIES} -lGL)
    target_compile_options(example_glfw_opengl2 PRIVATE ${GLFW_CFLAGS_OTHER})
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
