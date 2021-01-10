cmake_minimum_required(VERSION 2.6)
project(http_server)


################################
# Boost
################################
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost COMPONENTS
  system
  thread
  regex
REQUIRED)
if(Boost_FOUND)
  include_directories(SYSTEM ${Boost_INCLUDE_DIR})
else(Boost_FOUND)
  message("No Boost found.")
endif(Boost_FOUND)


################################
# Library Target
################################
include_directories(".")
file(GLOB HTTP_SERVER_FILES "*.cpp")
add_library(http_server STATIC ${HTTP_SERVER_FILES})
target_link_libraries(http_server ${Boost_LIBRARIES})


################################
# Compiler settings
################################
if(MSVC)
  set(_def
    _VARIADIC_MAX=10
    _WIN32_WINNT=0x0501
  )
  set_target_properties(http_server PROPERTIES COMPILE_DEFINITIONS "${_def}")
else()
  set_target_properties(http_server PROPERTIES COMPILE_FLAGS "-std=c++11")

  if (MINGW)
    ### Compiler Definitions
    set(_def
      BOOST_THREAD_USE_LIB
      WINVER=0x0501
      _WIN32_WINNT=0x0501
      _WIN32_IE=0x0501
    )
    set_target_properties(http_server PROPERTIES COMPILE_DEFINITIONS "${_def}")
  endif()
endif()