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

# hist
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/sort/hist.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(HIST_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${HIST_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${HIST_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${HIST_HEADER}
        DEPENDS ${SPIRV}
)


# sort
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/sort/sort.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(SORT_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${SORT_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${SORT_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${SORT_HEADER}
        DEPENDS ${SPIRV}
)

# calibrate        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/calibrate.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(CALIBRATE_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${CALIBRATE_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${CALIBRATE_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${CALIBRATE_HEADER}
        DEPENDS ${SPIRV}
)

#set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/common.glsl")
#get_filename_component(FILE_NAME ${GLSL} NAME_WE)
#string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
#set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
#set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
#set(COMMON_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")
#add_custom_command(
#        OUTPUT ${SPIRV}
#        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
#        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
#        DEPENDS ${GLSL})
#list(APPEND SPIRV_BINARY_FILES ${SPIRV})
#
#add_custom_command(
#        OUTPUT ${COMMON_HEADER}
#        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${COMMON_HEADER}
#        DEPENDS ${SPIRV})

# precomp_cov3d
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/precomp_cov3d.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(PRECOMP_COV3D_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${PRECOMP_COV3D_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${PRECOMP_COV3D_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${PRECOMP_COV3D_HEADER}
        DEPENDS ${SPIRV}
)

# prefix_sum        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/prefix_sum.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(PREFIX_SUM_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${PREFIX_SUM_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${PREFIX_SUM_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${PREFIX_SUM_HEADER}
        DEPENDS ${SPIRV}
)

# preprocess_sort        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/preprocess_sort.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(PREPROCESS_SORT_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${PREPROCESS_SORT_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${PREPROCESS_SORT_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${PREPROCESS_SORT_HEADER}
        DEPENDS ${SPIRV}
)

# preprocess        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/preprocess.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(PREPROCESS_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${PREPROCESS_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${PREPROCESS_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${PREPROCESS_HEADER}
        DEPENDS ${SPIRV}
)


# render        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/render.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(RENDER_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${RENDER_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${RENDER_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${RENDER_HEADER}
        DEPENDS ${SPIRV}
)

# tile_boundary        
set(GLSL "${CMAKE_SOURCE_DIR}/src/shaders/tile_boundary.comp")
get_filename_component(FILE_NAME ${GLSL} NAME_WE)
string(TOUPPER ${FILE_NAME} FILE_NAME_UPPER)
set(FILE_NAME_UPPER "SPV_${FILE_NAME_UPPER}")
set(SPIRV "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.spv")
set(TILE_BOUNDARY_HEADER "${CMAKE_BINARY_DIR}/shaders/${FILE_NAME}.h")

add_custom_command(
        OUTPUT ${SPIRV}
        COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/shaders/"
        COMMAND ${CMAKE_COMMAND} -E remove ${TILE_BOUNDARY_HEADER}
        COMMAND ${Vulkan_GLSLANG_VALIDATOR_EXECUTABLE} "--target-env" "vulkan1.2" -V ${GLSL} -o ${SPIRV} ${GLSLC_DEFINE}
        DEPENDS ${GLSL}
)
list(APPEND SPIRV_BINARY_FILES ${SPIRV})

add_custom_command(
        OUTPUT ${TILE_BOUNDARY_HEADER}
        COMMAND embedfile ${FILE_NAME_UPPER} ${SPIRV} ${TILE_BOUNDARY_HEADER}
        DEPENDS ${SPIRV}
)




# TODO: Hardcode shaders

add_custom_target(gs_shaders DEPENDS ${HIST_HEADER} ${SORT_HEADER} ${CALIBRATE_HEADER} ${PRECOMP_COV3D_HEADER} ${PREFIX_SUM_HEADER} ${PREPROCESS_SORT_HEADER} ${RENDER_HEADER} ${PREPROCESS_HEADER} ${TILE_BOUNDARY_HEADER} ${SPIRV_BINARY_FILES})
