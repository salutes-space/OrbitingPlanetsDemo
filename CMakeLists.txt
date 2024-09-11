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
target_link_libraries(OrbitingPlanetsDemo PRIVATE
    sfml-system 
    sfml-window 
    sfml-graphics 
    sfml-network 
    sfml-audio
)

if(WIN32)
    # First, check if SFML DLLs exist in app_deps
    message(STATUS "${CMAKE_BINARY_DIR}")
    set(SFML_DLL_PATH "${CMAKE_BINARY_DIR}/app_deps/sfml-build/lib/Debug")
    
    # Check if the path exists
    if(NOT EXISTS "${SFML_DLL_PATH}/../..")
        message(STATUS "app_deps path not found. Checking _deps...")
        # Fall back to _deps if app_deps does not exist
        set(SFML_DLL_PATH "${CMAKE_BINARY_DIR}/_deps/sfml-build/lib/Debug")

        # Check if the fallback path exists
        if(NOT EXISTS "${SFML_DLL_PATH}/../..")
            message(FATAL_ERROR "Neither app_deps nor _deps SFML library paths exist.")
        endif()
    endif()

    # Add a post-build command to copy the required SFML DLLs
    add_custom_command(TARGET OrbitingPlanetsDemo POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${SFML_DLL_PATH}"                       # Source directory (Debug folder)
        "$<TARGET_FILE_DIR:OrbitingPlanetsDemo>" # Destination directory (where the executable is built)
        COMMENT "Copying SFML Debug folder from ${SFML_DLL_PATH} to output directory."
    )
    message(STATUS "SFML DLLs will be copied from: ${SFML_DLL_PATH}")
endif()


# Set the runtime output directory (optional, if you want control over where the binary is placed)
set_target_properties(OrbitingPlanetsDemo PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
