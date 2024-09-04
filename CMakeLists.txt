# Specify the minimum version of CMake required
cmake_minimum_required(VERSION 3.15)

# Set the project name
project(OrbitingPlanetsDemo)

# # Specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Add the include directory
include_directories(${CMAKE_SOURCE_DIR}/include)

# Include the FetchContent module
include(FetchContent)

# Configure FetchContent to download SFML
FetchContent_Declare(
  SFML
  GIT_REPOSITORY https://github.com/SFML/SFML.git
  GIT_TAG 2.6.1
)

# Make available the SFML content
FetchContent_MakeAvailable(SFML)

# Add the executable
add_executable(OrbitingPlanetsDemo
    src/main.cpp
    src/Planet.cpp
)

# Link the SFML libraries to the executable
target_link_libraries(OrbitingPlanetsDemo 
    sfml-system 
    sfml-window 
    sfml-graphics 
    sfml-network 
    sfml-audio
)
