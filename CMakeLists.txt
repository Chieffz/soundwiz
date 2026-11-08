cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_COMPILER "C:/mingw64/bin/g++.exe")
set(CMAKE_C_COMPILER "C:/mingw64/bin/gcc.exe")
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(SoundWiz CXX)

# ===WxWidgets Config===
option(wxBUILD_EXAMPLES "Build wxWidgets examples" OFF)
option(wxBUILD_TESTS "Build wxWidgets tests" OFF)

set(wxWidgets_ROOT_DIR "${CMAKE_SOURCE_DIR}/ext/wxWidgets")
set(wxWidgets_CONFIGURATION mswu)

# == OpenSSL Config ===
set(OPENSSL_ROOT_DIR "${CMAKE_SOURCE_DIR}/ext/OpenSSL")
set(OPENSSL_INCLUDE_DIR "${OPENSSL_ROOT_DIR}/include")
set(OPENSSL_LIBRARIES  "${OPENSSL_ROOT_DIR}/lib")
set(OPENSSL_CRYPTO_LIBRARY "${OPENSSL_LIBRARIES}/libcrypto.a")
set(OPENSSL_SSL_LIBRARY "${OPENSSL_ROOT_DIR}/lib/libssl.a")

set(INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
set(EXT_DIR ${CMAKE_SOURCE_DIR}/ext)

## Libraries
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}" ${CMAKE_MODULE_PATH})
find_package(wxWidgets REQUIRED COMPONENTS core base)
find_package(OpenSSL REQUIRED)
include(${wxWidgets_USE_FILE})

# === Configuring Source Directories ===

set(SRC_DIR ${CMAKE_SOURCE_DIR}/src)

# === Include Directories ===

include_directories(
    ${INCLUDE_DIR}
    ${wxWidgets_ROOT_DIR}/include
)

add_executable(Soundwiz ./src/main.cpp)

## Link Libraries/Directories
target_link_libraries(Soundwiz 
    PRIVATE 
    ## Actual Project Directories/Libraries

    PUBLIC
    wsock32
    ws2_32
    ${wxWidgets_LIBRARIES}
    OpenSSL::SSL
    OpenSSL::Crypto
    )

# target_link_directories(Soundwiz 
#     PRIVATE

#     )

target_include_directories(Soundwiz 
    PRIVATE 
    ${INCLUDE_DIR}
    PUBLIC
    ${OPENSSL_INCLUDE_DIR}
)

# target_compile_definitions(Soundwiz 
#     PRIVATE
#     ${wxWidgets_DEFINITIONS}
#     )
