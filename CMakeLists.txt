cmake_minimum_required(VERSION 3.21)

# Let's choose a name and version for the extension. Change the name to your
# liking. The version should be a string like "1.0".
set(PROJECT_NAME "HelloGP-C") # Change this to your liking
project(${PROJECT_NAME} VERSION 1.0)

# Import the SDK
include(FetchContent)
FetchContent_Declare(
  gp-sdk
  GIT_REPOSITORY https://github.com/gigperformer/gp-sdk.git
  GIT_TAG 0e95c145f69c73dbfc46324a70474c07fa61dd9f # v4.8
)
FetchContent_MakeAvailable(gp-sdk)

# Define our library including sources, include directories and dependencies
add_library(${PROJECT_NAME} SHARED)
target_sources(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/src/main.c)
target_link_libraries(${PROJECT_NAME} PRIVATE gigperformer::sdk::c)
target_include_directories(${PROJECT_NAME}
                           PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Language options: this will be a pure C11 project
set_target_properties(
  ${PROJECT_NAME}
  PROPERTIES C_STANDARD 11
             C_STANDARD_REQUIRED ON
             C_EXTENSIONS NO)

# Install the extension on the development machine
install(
  TARGETS ${PROJECT_NAME}
  LIBRARY DESTINATION "${GIG_PERFORMER_EXTENSIONS_DIRECTORY}"
  RUNTIME DESTINATION "${GIG_PERFORMER_EXTENSIONS_DIRECTORY}")
