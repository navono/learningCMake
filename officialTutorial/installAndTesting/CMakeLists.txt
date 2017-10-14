cmake_minimum_required(VERSION 3.6.0)
project(project)

ADD_SUBDIRECTORY(MathFunctions)
ADD_SUBDIRECTORY(Tutorial)

#install(TARGETS Tutorial DESTINATION bin)
#install(FILES "Tutorial/TutorialConfig.h" DESTINATION include)

# install(TARGETS Tutorial MathFunctions
#   RUNTIME DESTINATION bin
#   LIBRARY DESTINATION lib
#   ARCHIVE DESTINATION lib/static
#   PUBLIC_HEADER DESTINATION include
# )
