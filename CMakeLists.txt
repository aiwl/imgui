cmake_minimum_required(VERSION 3.10)

project(imgui VERSION 0.0.0)

add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

# Generate files for clangd to use.
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "")

# -- library ----------------------------------------------------------------

set(SRC_IMGUI
    imconfig.h
    imgui.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
)
set(SOURCES
    ${SRC_IMGUI}
)
source_group("imgui" FILES ${SRC_IMGUI})

add_library(imgui ${SOURCES})
target_include_directories(imgui PUBLIC ".")
target_compile_features(imgui PUBLIC cxx_std_17)

# -- sdl + gl3 backend ------------------------------------------------------

set(SRC_IMGUI_SDL_GL3
    backends/imgui_impl_sdl.h
    backends/imgui_impl_sdl.cpp
    backends/imgui_impl_opengl3.h
    backends/imgui_impl_opengl3_loader.h
    backends/imgui_impl_opengl3.cpp
)
source_group("imgui/sdl_gl3" FILES ${SRC_IMGUI_SDL_GL3})
add_library(imgui_sdl_gl3 ${SRC_IMGUI_SDL_GL3})
target_include_directories(imgui_sdl_gl3 PUBLIC "./backends")
target_compile_features(imgui_sdl_gl3 PUBLIC cxx_std_17)
target_link_libraries(
    imgui_sdl_gl3
    PUBLIC
        imgui
        SDL2-static
        imgui
        opengl32
)
