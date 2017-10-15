set(MODULE_NAME app)

# Properties -> C/C++ -> General -> Additional Include Directories
include_directories("${PROJECT_SOURCE_DIR}/math/include")

# Set Properties->General->Configuration Type to Application(.exe)
# Creates app.exe with the listed sources (main.cxx)
# Adds sources to the Solution Explorer
add_executable (${MODULE_NAME} main.cc)

# Properties->Linker->Input->Additional Dependencies
target_link_libraries (${MODULE_NAME} PRIVATE math)

# Creates a folder "executables" and adds target 
# project (app.vcproj) under it
set_property(TARGET ${MODULE_NAME} PROPERTY FOLDER "executables")

# Properties->General->Output Directory
set_target_properties(${MODULE_NAME} PROPERTIES
  RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

# Adds logic to INSTALL.vcproj to copy app.exe to destination directory
install (TARGETS ${MODULE_NAME}
  RUNTIME DESTINATION ${PROJECT_SOURCE_DIR}/_install)
