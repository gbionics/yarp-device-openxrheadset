add_library(
    openxr-gfxwrapper STATIC gfxwrapper_opengl.c
                             gfxwrapper_opengl.h
)
target_link_libraries(openxr-gfxwrapper PUBLIC openxr-glad-loader)
target_include_directories(openxr-gfxwrapper PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

