add_definitions(-DTF_DLL)

find_package(Qt5 COMPONENTS Core Network Xml Sql REQUIRED)

if(NOT Qt5_FOUND)
  message(FATAL_ERROR "Qt5 was not found. Consider setting QT5_CMAKE_PATH to the Qt5Config.cmake directory.")
endif()

file(GLOB controller_srcs ${PROJECT_SOURCE_DIR}/controllers/*.cpp)
file(GLOB controller_headers ${PROJECT_SOURCE_DIR}/controllers/*.h)
qt5_wrap_cpp(controller_mocs ${controller_headers})

add_library(controller SHARED
  ${controller_srcs}
  ${controller_mocs}
)
target_include_directories(controller PUBLIC
  ${Qt5Core_INCLUDE_DIRS}
  ${Qt5Network_INCLUDE_DIRS}
  ${Qt5Xml_INCLUDE_DIRS}
  ${Qt5Sql_INCLUDE_DIRS}
  ${TreeFrog_INCLUDE_DIR}
  ${PROJECT_SOURCE_DIR}/helpers
  ${PROJECT_SOURCE_DIR}/models
)
target_link_libraries(controller
  Qt5::Core
  Qt5::Network
  Qt5::Xml
  Qt5::Sql
  ${TreeFrog_LIB}
  helper
  model
)
set_target_properties(controller PROPERTIES
  LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib
  SOVERSION 1.0
)
add_dependencies(controller
  helper
  model
)
