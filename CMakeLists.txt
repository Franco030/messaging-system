cmake_minimum_required(VERSION 3.20)
project(MessagingSystem LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(FetchContent)

FetchContent_Declare(
    asio
    GIT_REPOSITORY https://github.com/chriskohlhoff/asio.git
    GIT_TAG asio-1-30-2
)

FetchContent_Declare(
    cpp_orm
    GIT_REPOSITORY https://github.com/Franco030/CPP_ORM.git
    GIT_TAG main
)

FetchContent_MakeAvailable(asio cpp_orm)

add_executable(server server.cpp)
add_executable(client client.cpp)

add_library(network_config INTERFACE)

if(WIN32)
    target_compile_definitions(network_config INTERFACE _WIN32_WINNT=0x0A00)
    target_link_libraries(network_config INTERFACE ws2_32 mswsock)
endif()

target_compile_options(network_config INTERFACE -Wall -Wextra -Wpedantic)
target_compile_definitions(network_config INTERFACE ASIO_STANDALONE)
target_include_directories(network_config INTERFACE ${asio_SOURCE_DIR}/asio/include)

target_link_libraries(server PRIVATE network_config)
target_link_libraries(client PRIVATE network_config)