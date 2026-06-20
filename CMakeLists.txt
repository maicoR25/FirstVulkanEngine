cmake_minimum_required(VERSION 4.0)

project (FirstVulkanEngine)

find_package (Vulkan REQUIRED)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/CMake")

# set up Vulkan C++ module (enabled when ENABLE_CPP20_MODULE=ON)
add_library(VulkanCppModule)
add_library(Vulkan::cppm ALIAS VulkanCppModule)

find_package (slang CONFIG HINTS "C:/VulkanSDK/1.4.350.0/Lib/cmake")
find_package (glfw3 REQUIRED)
find_package (glm REQUIRED)

target_compile_definitions(VulkanCppModule PUBLIC
        VULKAN_HPP_DISPATCH_LOADER_DYNAMIC=1
        VULKAN_HPP_NO_STRUCT_CONSTRUCTORS=1
)

target_include_directories(VulkanCppModule PRIVATE "${Vulkan_INCLUDE_DIR}")

target_link_libraries(VulkanCppModule PUBLIC Vulkan::Vulkan)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/CMake")

add_executable(FirstVulkanEngine src/firstvulkanengine.cpp)

target_link_libraries (${PROJECT_NAME} Vulkan::cppm glfw glm::glm)

set_target_properties(VulkanCppModule PROPERTIES CXX_STANDARD 20)
set_target_properties(FirstVulkanEngine PROPERTIES CXX_STANDARD 20)

target_sources(VulkanCppModule
        PUBLIC
        FILE_SET cxx_modules TYPE CXX_MODULES
        BASE_DIRS "${Vulkan_INCLUDE_DIR}"
        FILES "${Vulkan_INCLUDE_DIR}/vulkan/vulkan.cppm"
)