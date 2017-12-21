
# add the exe target that consumes the lib
add_executable( myexe main.cpp )
target_link_libraries( myexe PRIVATE mylib )
target_include_directories( myexe PRIVATE "${CMAKE_SOURCE_DIR}")

# Add a custom target to copy the dll to some other directory

# Guess the output name for simplicity here, which will only work when compiling the debug config.
# Note that the lack of support of generator expression in add_custom_command()
# OUTPUT makes live hard here if we want to do it properly.
set( dest_file "${CMAKE_CURRENT_BINARY_DIR}/Debug/mylib.dll" )
                                                                
add_custom_command(
    DEPENDS $<TARGET_FILE:mylib>
    OUTPUT ${dest_file}
    COMMAND cmake -E copy $<TARGET_FILE:mylib> ${dest_file}
)

add_custom_target(
    copy_mylib_to_myexe
    DEPENDS ${dest_file}
)
add_dependencies( myexe copy_mylib_to_myexe ) # make sure the dll is deployed before the exe is build.


# Try to check if the copied file already is generated.
get_property( is_generated SOURCE ${dest_file} PROPERTY GENERATED )
message("file is generated? ${is_generated}")

