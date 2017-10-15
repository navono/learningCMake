set(MODULE_NAME math)

# Collect sources into the variable MATH_SOURCES with
# having to explicitly list each header and source file
file(GLOB MATH_SOURCES 
  "include/*.h")

# Collect sources into the variable SIMPLE_FUNCTION_SOURCES  
file(GLOB SIMPLE_FUNCTION_SOURCES
  "simple/*.cxx")

# Another way to collect sources into a variable ADVANCED_FUNCTION_SOURCES
# by explicitly specifying the source files
set(ADVANCED_FUNCTION_SOURCES
  "advanced/AdvancedFunctions.cxx")

# Create named folders for the sources within the .vcproj
# Empty name lists them directly under the .vcproj
source_group("include" FILES ${MATH_SOURCES})
source_group("simple" FILES ${SIMPLE_FUNCTION_SOURCES})
source_group("advanced" FILES ${ADVANCED_FUNCTION_SOURCES})

# Properties->C/C++->General->Additional Include Directories
include_directories(include)
# target_include_directories(${MODULE_NAME} PUBLIC include)

# Set Properties->General->Configuration Type to Dynamic Library(.dll)
# Creates math.dll with the listed sources collected in the variables
# Also adds sources to the Solution Explorer
add_library(${MODULE_NAME} SHARED 
  ${MATH_SOURCES} 
  ${SIMPLE_FUNCTION_SOURCES} 
  ${ADVANCED_FUNCTION_SOURCES})

# Creates a folder "libraries" and adds target project (math.vcproj) under it
# Properties->General->Output Directory
set_target_properties(${MODULE_NAME} PROPERTIES
  FOLDER "libraries"
  RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

# Adds logic to INSTALL.vcproj to copy math.dll to the destination directory
install(TARGETS ${MODULE_NAME}
  RUNTIME DESTINATION ${PROJECT_SOURCE_DIR}/_install
  LIBRARY DESTINATION ${PROJECT_SOURCE_DIR}/_install)
