cmake_minimum_required(VERSION 3.26)
project(shaders)

add_executable(embedfile ${CMAKE_SOURCE_DIR}/cmake/embedfile.c)

if (CMAKE_BUILD_TYPE MATCHES Debug AND NOT APPLE)
    list(APPEND GLSLC_DEFINE "-DDEBUG")
else ()
    list(APPEND GLSLC_DEFINE "-DNDEBUG")
endif ()

set(SHADER_HEADER "${CMAKE_BINARY_DIR}/shaders/shaders.h")

message(STATUS "Shader header file: ${SHADER_HEADER}")

# Delete old header file
add_custom_command(
        OUTPUT ${SHADER_HEADER}
        COMMAND ${CMAKE_COMMAND} -E remove ${SHADER_HEADER}
)


set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/render.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(RENDER_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")
add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL})
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${RENDER_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${RENDER_HEADER}
        DEPENDS ${SPIRV})


set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/preprocess.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(PREPROCESS_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")
add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL})
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${PREPROCESS_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${PREPROCESS_HEADER}
        DEPENDS ${SPIRV})



# TODO: Hardcode shaders

add_custom_target(gs_shaders DEPENDS ${RENDER_HEADER} ${PREPROCESS_HEADER} ${SPIRV_BINARY_FILES})
