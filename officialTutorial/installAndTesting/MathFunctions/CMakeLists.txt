# add_library(MathFunctions SHARED mysqrt.cc)
add_library(MathFunctions mysqrt.cc)

install (TARGETS MathFunctions DESTINATION bin)
install (FILES MathFunctions.h DESTINATION include)
