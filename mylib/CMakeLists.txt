include(GenerateExportHeader)

add_library( mylib foo.cpp )
target_include_directories( mylib PUBLIC ${CMAKE_BINARY_DIR})
generate_export_header( mylib BASE_NAME mylib )
