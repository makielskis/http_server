cmake_minimum_required(VERSION 3.10)
project(http_server)


################################
# Library Target
################################
include_directories(".")
file(GLOB HTTP_SERVER_FILES "src/*.cpp")
add_library(http_server STATIC ${HTTP_SERVER_FILES})
target_link_libraries(http_server boost)
target_include_directories(http_server PUBLIC include)


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