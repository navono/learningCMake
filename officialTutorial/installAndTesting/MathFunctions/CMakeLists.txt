# add_library(MathFunctions SHARED mysqrt.cc)
add_library(MathFunctions mysqrt.cc)


# Properties->General->Output Directory
set_target_properties(MathFunctions PROPERTIES
  RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)


install (TARGETS MathFunctions ARCHIVE DESTINATION bin)
install (FILES MathFunctions.h DESTINATION include)