cmake_minimum_required(VERSION 3.7...3.23)
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(boost_example LANGUAGES CXX)

# ---- Fetch CPM ----

set(CPM_DOWNLOAD_VERSION 0.27.2) 
set(CPM_DOWNLOAD_LOCATION "${CMAKE_BINARY_DIR}/cmake/CPM_${CPM_DOWNLOAD_VERSION}.cmake")

if(NOT (EXISTS ${CPM_DOWNLOAD_LOCATION}))
  message(STATUS "Downloading CPM.cmake v${CPM_DOWNLOAD_VERSION}")
  file(DOWNLOAD https://github.com/TheLartians/CPM.cmake/releases/download/v${CPM_DOWNLOAD_VERSION}/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()

include(${CPM_DOWNLOAD_LOCATION})

# ---- Add dependencies via CPM ----
# See https://github.com/TheLartians/CPM.cmake for details and examples

CPMFindPackage(
  NAME Boost
  GITHUB_REPOSITORY Orphis/boost-cmake
  VERSION 1.67.0
  FIND_PACKAGE_ARGUMENTS "COMPONENTS system"
)

# ---- Create binary ----

add_executable(boost_example main.cpp)
target_link_libraries(boost_example PRIVATE Boost::system pthread)
set_target_properties(boost_example PROPERTIES CXX_STANDARD 17)
