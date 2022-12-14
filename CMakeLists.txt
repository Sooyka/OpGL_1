cmake_minimum_required(VERSION 3.16.3)

#Set the build output directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

#Print selected build type / available build types in case of Visual Studio
if (CMAKE_GENERATOR MATCHES "Visual Studio")
	message(STATUS "Build types: ${CMAKE_CONFIGURATION_TYPES}")
else()
	message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
endif()

#Set the project name (and version)
project(LennardJones)

#Specify CPP standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

#Build current project as an executable
add_executable(${PROJECT_NAME})

#Add preprocessor definitions
target_compile_definitions(${PROJECT_NAME} PRIVATE "GLFW_INCLUDE_NONE")

#Specify source files
file(GLOB_RECURSE headers src/*.h)
file(GLOB_RECURSE sources src/*.cpp)
target_sources(${PROJECT_NAME} PRIVATE ${headers} ${sources})

#Set current project as Visual Studio start project
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})

#Add glfw
add_subdirectory(vendor/glfw)
target_include_directories(${PROJECT_NAME} PUBLIC vendor/glfw/include)
target_link_directories(${PROJECT_NAME} PRIVATE vendor/glfw/src)
target_link_libraries(${PROJECT_NAME} glfw)

#Add glad
add_subdirectory(vendor/glad)
target_include_directories(${PROJECT_NAME} PUBLIC vendor/glad/include)
target_link_directories(${PROJECT_NAME} PRIVATE vendor/glad/src)
target_link_libraries(${PROJECT_NAME} glad)
