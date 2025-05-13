# Define the ImGui static library
add_library(ImGui STATIC
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_widgets.cpp
    imgui_tables.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    imgui_demo.cpp
    backends/imgui_impl_opengl3.cpp
)

# Set the output directories for the compiled library and object files
set_target_properties(ImGui PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}/ImGui"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}/ImGui"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}/ImGui"
    OUTPUT_NAME "ImGui"
)

# Include the current source directory for header file resolution
target_include_directories(ImGui PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}/backends")

# Set C++ standard and runtime options based on the operating system
if (WIN32)
    target_compile_features(ImGui PRIVATE cxx_std_17)
    set_property(TARGET ImGui PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")
elseif (UNIX)
    target_compile_features(ImGui PRIVATE cxx_std_17)
    set_target_properties(ImGui PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

# Configure compiler options based on the build configuration
target_compile_options(ImGui PRIVATE
    $<$<CONFIG:Debug>:
        $<$<CXX_COMPILER_ID:MSVC>:/Zi>
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-g>
    >
    $<$<CONFIG:Release>:
        $<$<CXX_COMPILER_ID:MSVC>:/O2>
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-O2>
    >
)
