if (imgui_lib_path)
    set(CURRENT_PATH ${imgui_lib_path})
else ()
    set(CURRENT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/)
endif ()
set(LIB_NAME cc_imgui)

include_directories(
        ${CURRENT_PATH}/imgui
        ${CURRENT_PATH}/imgui/misc/cpp
        ${CURRENT_PATH}/
        ${COCOS2DX_ROOT_PATH}/cocos
        ${COCOS2DX_ROOT_PATH}/cocos/base
        ${COCOS2DX_ROOT_PATH}/cocos/platform
        ${COCOS2DX_ROOT_PATH}/external
        ${COCOS2DX_ROOT_PATH}/external/glfw3/include/win32
)

if (WINDOWS)
    include_directories(${COCOS2DX_ROOT_PATH}/external/win32-specific/gles/include/OGLES)
endif ()

set(HEADER
        ${CURRENT_PATH}/CCIMGUI.h
        ${CURRENT_PATH}/CCImGuiLayer.h
        ${CURRENT_PATH}/CCImGuiColorTextEdit.h
        ${CURRENT_PATH}/imgui_impl_cocos2dx.h
        ${CURRENT_PATH}/imgui/imconfig.h
        ${CURRENT_PATH}/imgui/imgui.h
        ${CURRENT_PATH}/imgui/imgui_internal.h
        ${CURRENT_PATH}/imgui/imstb_rectpack.h
        ${CURRENT_PATH}/imgui/imstb_textedit.h
        ${CURRENT_PATH}/imgui/imstb_truetype.h
        ${CURRENT_PATH}/imgui/misc/cpp/imgui_stdlib.h
        ${CURRENT_PATH}/imgui_markdown/imgui_markdown.h
        ${CURRENT_PATH}/ImGuiColorTextEdit/TextEditor.h
        ${CURRENT_PATH}/implot/implot.h
        )

set(SOURCE
        ${CURRENT_PATH}/CCIMGUI.cpp
        ${CURRENT_PATH}/CCImGuiLayer.cpp
        ${CURRENT_PATH}/CCImGuiColorTextEdit.cpp
        ${CURRENT_PATH}/imgui_impl_cocos2dx.cpp
        ${CURRENT_PATH}/imgui/imgui.cpp
        ${CURRENT_PATH}/imgui/imgui_demo.cpp
        ${CURRENT_PATH}/imgui/imgui_draw.cpp
        ${CURRENT_PATH}/imgui/imgui_widgets.cpp
        ${CURRENT_PATH}/imgui/misc/cpp/imgui_stdlib.cpp
        ${CURRENT_PATH}/ImGuiColorTextEdit/TextEditor.cpp
        ${CURRENT_PATH}/implot/implot.cpp
        ${CURRENT_PATH}/implot/implot_demo.cpp
        )

if (BUILD_LUA_LIBS)
    include_directories(
            ${CURRENT_PATH}/lua-bindings
            ${COCOS2DX_ROOT_PATH}/external/lua/luajit/include
            ${COCOS2DX_ROOT_PATH}/external/lua/tolua
    )
    list(APPEND HEADER
            ${CURRENT_PATH}/lua-bindings/imgui_lua.hpp
            ${CURRENT_PATH}/lua-bindings/lua_conversion.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imgui_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiDrawList_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiFont_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiIO_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiStyle_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiViewport_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_ImGuiColorTextEdit_auto.hpp
            ${CURRENT_PATH}/lua-bindings/lua_implot_auto.hpp
            )
    list(APPEND SOURCE
            ${CURRENT_PATH}/lua-bindings/imgui_lua.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiDrawList_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiFont_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiIO_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiStyle_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imguiViewport_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_imgui_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_ImGuiColorTextEdit_auto.cpp
            ${CURRENT_PATH}/lua-bindings/lua_implot_auto.cpp
            )
endif ()

add_library(${LIB_NAME} STATIC ${SOURCE} ${HEADER})
add_dependencies(${LIB_NAME} cocos2d)
use_cocos2dx_compile_define(${LIB_NAME})
use_cocos2dx_compile_options(${LIB_NAME})

if (BUILD_LUA_LIBS)
    target_compile_definitions(${LIB_NAME} PUBLIC _USRLUASTATIC)
endif ()

set_target_properties(${LIB_NAME}
        PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        )
target_include_directories(${LIB_NAME} INTERFACE
        ${CURRENT_PATH}/
        ${CURRENT_PATH}/imgui
        )

if (XCODE OR VS)
    cocos_mark_code_files(${LIB_NAME})
endif ()
