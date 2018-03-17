cmake_minimum_required(VERSION 3.0)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
project(dng_sdk)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)


set(PUBLIC_HDRS
    source/dng_1d_function.h
    source/dng_1d_table.h
    source/dng_abort_sniffer.h
    source/dng_area_task.h
    source/dng_assertions.h
    source/dng_auto_ptr.h
    source/dng_bad_pixels.h
    source/dng_big_table.h
    source/dng_bottlenecks.h
    source/dng_camera_profile.h
    source/dng_classes.h
    source/dng_color_space.h
    source/dng_color_spec.h
    source/dng_date_time.h
    source/dng_errors.h
    source/dng_exceptions.h
    source/dng_exif.h
    source/dng_fast_module.h
    source/dng_file_stream.h
    source/dng_filter_task.h
    source/dng_fingerprint.h
    source/dng_flags.h
    source/dng_gain_map.h
    source/dng_globals.h
    source/dng_host.h
    source/dng_hue_sat_map.h
    source/dng_ifd.h
    source/dng_image.h
    source/dng_image_writer.h
    source/dng_info.h
    source/dng_iptc.h
    source/dng_jpeg_image.h
    source/dng_lens_correction.h
    source/dng_linearization_info.h
    source/dng_local_string.h
    source/dng_lossless_jpeg.h
    source/dng_matrix.h
    source/dng_memory.h
    source/dng_memory_stream.h
    source/dng_misc_opcodes.h
    source/dng_mosaic_info.h
    source/dng_mutex.h
    source/dng_negative.h
    source/dng_opcode_list.h
    source/dng_opcodes.h
    source/dng_orientation.h
    source/dng_parse_utils.h
    source/dng_pixel_buffer.h
    source/dng_point.h
    source/dng_preview.h
    source/dng_pthread.h
    source/dng_rational.h
    source/dng_read_image.h
    source/dng_rect.h
    source/dng_ref_counted_block.h
    source/dng_reference.h
    source/dng_render.h
    source/dng_resample.h
    source/dng_safe_arithmetic.h
    source/dng_sdk_limits.h
    source/dng_shared.h
    source/dng_simple_image.h
    source/dng_spline.h
    source/dng_stream.h
    source/dng_string.h
    source/dng_string_list.h
    source/dng_tag_codes.h
    source/dng_tag_types.h
    source/dng_tag_values.h
    source/dng_temperature.h
    source/dng_tile_iterator.h
    source/dng_tone_curve.h
    source/dng_types.h
    source/dng_uncopyable.h
    source/dng_utils.h
    source/dng_xmp.h
    source/dng_xmp_sdk.h
    source/dng_xy_coord.h
)

set(SRCS 
    source/dng_1d_function.cpp
    source/dng_1d_table.cpp
    source/dng_abort_sniffer.cpp
    source/dng_area_task.cpp
    source/dng_bad_pixels.cpp
    source/dng_big_table.cpp
    source/dng_bottlenecks.cpp
    source/dng_camera_profile.cpp
    source/dng_color_space.cpp
    source/dng_color_spec.cpp
    source/dng_date_time.cpp
    source/dng_exceptions.cpp
    source/dng_exif.cpp
    source/dng_file_stream.cpp
    source/dng_filter_task.cpp
    source/dng_fingerprint.cpp
    source/dng_gain_map.cpp
    source/dng_globals.cpp
    source/dng_host.cpp
    source/dng_hue_sat_map.cpp
    source/dng_ifd.cpp
    source/dng_image.cpp
    source/dng_image_writer.cpp
    source/dng_info.cpp
    source/dng_iptc.cpp
    source/dng_jpeg_image.cpp
    source/dng_lens_correction.cpp
    source/dng_linearization_info.cpp
    source/dng_local_string.cpp
    source/dng_lossless_jpeg.cpp
    source/dng_matrix.cpp
    source/dng_memory.cpp
    source/dng_memory_stream.cpp
    source/dng_misc_opcodes.cpp
    source/dng_mosaic_info.cpp
    source/dng_mutex.cpp
    source/dng_negative.cpp
    source/dng_opcode_list.cpp
    source/dng_opcodes.cpp
    source/dng_orientation.cpp
    source/dng_parse_utils.cpp
    source/dng_pixel_buffer.cpp
    source/dng_point.cpp
    source/dng_preview.cpp
    source/dng_pthread.cpp
    source/dng_rational.cpp
    source/dng_read_image.cpp
    source/dng_rect.cpp
    source/dng_ref_counted_block.cpp
    source/dng_reference.cpp
    source/dng_render.cpp
    source/dng_resample.cpp
    source/dng_safe_arithmetic.cpp
    source/dng_shared.cpp
    source/dng_simple_image.cpp
    source/dng_spline.cpp
    source/dng_stream.cpp
    source/dng_string.cpp
    source/dng_string_list.cpp
    source/dng_tag_types.cpp
    source/dng_temperature.cpp
    source/dng_tile_iterator.cpp
    source/dng_tone_curve.cpp
    source/dng_utils.cpp
    source/dng_xmp.cpp
    source/dng_xmp_sdk.cpp
    source/dng_xy_coord.cpp   
)

find_package(libjpeg-turbo REQUIRED CONFIG)
find_package(zlib REQUIRED CONFIG)
find_package(adobe-xmp REQUIRED CONFIG)
add_library(${PROJECT_NAME} ${PUBLIC_HDRS} ${SRCS})
add_library(::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_link_libraries(${PROJECT_NAME} PUBLIC ::zlibstatic jpeg::jpeg-static ::XMPCoreStatic ::XMPFilesStatic)
if (BUILD_SHARED_LIBS)
    set_target_properties(${PROJECT_NAME} PROPERTIES CXX_VISIBILITY_PRESET hidden)
    set_target_properties(${PROJECT_NAME} PROPERTIES VISIBILITY_INLINES_HIDDEN 1)
endif()
set_target_properties(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX _d)
set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER libs)
#target_compile_definitions(${PROJECT_NAME} PRIVATE -DNO_JASPER=1 -DNO_JPEG=1 -DNO_LCMS=1)
if (MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE "/MP")
    target_compile_definitions(${PROJECT_NAME} PUBLIC -DqWinOS=1 -DqWinUniversal=1 -DqDNGXMPDocOps=0 -DqDNGUseLibJPEG=1)
elseif(UNIX)
    if(APPLE)
        target_compile_definitions(${PROJECT_NAME} PUBLIC -qMacOS=1 -DqDNGXMPDocOps=0 -DqDNGUseLibJPEG=1)
    elseif(ANDROID)
        target_compile_definitions(${PROJECT_NAME} PUBLIC -qAndroid=1 -DqDNGXMPDocOps=0 -DqDNGUseLibJPEG=1)
    else()
        target_compile_definitions(${PROJECT_NAME} PUBLIC -DqLinux=1 -DqDNGXMPDocOps=0 -DqDNGUseLibJPEG=1 -DUNIX_ENV=1)
	target_link_libraries(${PROJECT_NAME} PUBLIC dl)
    endif()
endif()

source_group(${PROJECT_NAME} FILES ${PUBLIC_HDRS})

include(GenerateExportHeader)
generate_export_header(${PROJECT_NAME} EXPORT_FILE_NAME ${CMAKE_BINARY_DIR}/gen/${PROJECT_NAME}/${PROJECT_NAME}_export.h)


target_include_directories(${PROJECT_NAME} 
    PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/sources>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../xmp/toolkit/public/include>
        $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/gen/${PROJECT_NAME}>
        $<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)



include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/gen/${PROJECT_NAME}-config-version.cmake"
  VERSION 0.0.1
  COMPATIBILITY AnyNewerVersion
)

configure_file(cmake/${PROJECT_NAME}-config.cmake.in ${PROJECT_NAME}-config.cmake @ONLY)
install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}-targets DESTINATION 
    ARCHIVE DESTINATION lib LIBRARY DESTINATION lib RUNTIME DESTINATION bin)
install(FILES 
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config.cmake 
        ${CMAKE_CURRENT_BINARY_DIR}/gen/${PROJECT_NAME}-config-version.cmake
    DESTINATION 
        lib/cmake/${PROJECT_NAME})
    
install(EXPORT ${PROJECT_NAME}-targets NAMESPACE :: DESTINATION lib/cmake/${PROJECT_NAME})

install(FILES
    ${CMAKE_BINARY_DIR}/gen/${PROJECT_NAME}/${PROJECT_NAME}_export.h
	${PUBLIC_HDRS}
    DESTINATION include/${PROJECT_NAME})


option(BUILD_EXAMPLES "Build ${PROJECT_NAME} examples!" OFF)
if (BUILD_EXAMPLES)
    add_executable(dng_validate source/dng_validate.cpp)
	set_target_properties(dng_validate PROPERTIES DEBUG_POSTFIX _d)
    set_target_properties(dng_validate PROPERTIES FOLDER examples)
    target_link_libraries(dng_validate PRIVATE ::dng_sdk)
    target_compile_options(dng_validate PRIVATE -DqDNGValidateTarget=1)
    install(TARGETS dng_validate DESTINATION bin)
endif()
    
