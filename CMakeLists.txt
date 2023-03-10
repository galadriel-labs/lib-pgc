# Specify the minimum version for CMake
CMAKE_MINIMUM_REQUIRED(VERSION 3.2)

# Project's name 
PROJECT(Kunlun)

IF(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    SET(OPENSSL_LIBRARIES /usr/local/openssl/lib64/libcrypto.a /usr/local/openssl/lib64/libssl.a)
    SET(OPENSSL_INCLUDE_DIR /usr/local/openssl/include)
    SET(OPENSSL_DIR /usr/local/openssl/lib64)
ELSEIF(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    SET(OPENSSL_LIBRARIES /usr/local/lib/libcrypto.a /usr/local/lib/libssl.a)
    SET(OPENSSL_INCLUDE_DIR /usr/local/include)
    SET(OPENSSL_DIR /usr/local/lib)
ENDIF()
FIND_PACKAGE(OpenMP REQUIRED)


SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -fpic -Wno-deprecated-declarations -Wno-unused-result -Xpreprocessor -O3")

SET(CMAKE_CXX_LINK_EXECUTABLE "${CMAKE_CXX_LINK_EXECUTABLE} -ldl")

INCLUDE_DIRECTORIES(
    ${OPENSSL_INCLUDE_DIR}
    "/usr/local/include"
)

LINK_DIRECTORIES(
    /usr/local/lib
)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build) 

include(CMakePrintHelpers)

foreach(key
  IN ITEMS
    NUMBER_OF_LOGICAL_CORES
    NUMBER_OF_PHYSICAL_CORES
    IS_64BIT
    HAS_SSE2
    OS_NAME
    OS_RELEASE
  )
  # query the item ${key} and save its value in the variable _${key}
  cmake_host_system_information(RESULT _${key} QUERY ${key})
  cmake_print_variables(_${key})
endforeach()

# let the preprocessor know about the system name
if(CMAKE_SYSTEM_NAME  STREQUAL "Linux")
  SET(IS_LINUX 1)
endif()
if(CMAKE_SYSTEM_NAME  STREQUAL "Darwin")
  SET(IS_MACOS 1)
endif()
if(CMAKE_SYSTEM_NAME  STREQUAL "Windows")
  SET(IS_WINDOWS 1)
endif()

configure_file(${PROJECT_SOURCE_DIR}/config/config.h.in ${PROJECT_SOURCE_DIR}/config/config.h @ONLY)

# ske
ADD_EXECUTABLE(test_aes test/test_aes.cpp)
TARGET_LINK_LIBRARIES(test_aes ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)
  
# pke  
ADD_EXECUTABLE(test_twisted_elgamal test/test_twisted_elgamal.cpp)
TARGET_LINK_LIBRARIES(test_twisted_elgamal ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_elgamal test/test_elgamal.cpp)
TARGET_LINK_LIBRARIES(test_elgamal ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_calculate_dlog test/test_calculate_dlog.cpp)
TARGET_LINK_LIBRARIES(test_calculate_dlog ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# signature
ADD_EXECUTABLE(test_accountable_ring_sig test/test_accountable_ring_sig.cpp)
TARGET_LINK_LIBRARIES(test_accountable_ring_sig ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_schnorr_sig test/test_schnorr_sig.cpp)
TARGET_LINK_LIBRARIES(test_schnorr_sig ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# nike
ADD_EXECUTABLE(test_nizk_enc_relation test/test_nizk_enc_relation.cpp)
TARGET_LINK_LIBRARIES(test_nizk_enc_relation ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_nizk_dlog_knowledge test/test_nizk_dlog_knowledge.cpp)
TARGET_LINK_LIBRARIES(test_nizk_dlog_knowledge ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_nizk_plaintext_equality test/test_nizk_plaintext_equality.cpp)
TARGET_LINK_LIBRARIES(test_nizk_plaintext_equality ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_nizk_plaintext_knowledge test/test_nizk_plaintext_knowledge.cpp)
TARGET_LINK_LIBRARIES(test_nizk_plaintext_knowledge ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_nizk_dlog_equality test/test_nizk_dlog_equality.cpp)
TARGET_LINK_LIBRARIES(test_nizk_dlog_equality ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# bulletproof
ADD_EXECUTABLE(test_innerproduct_proof test/test_innerproduct_proof.cpp)
TARGET_LINK_LIBRARIES(test_innerproduct_proof ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

ADD_EXECUTABLE(test_bullet_proof test/test_bullet_proof.cpp)
TARGET_LINK_LIBRARIES(test_bullet_proof ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# gadget
ADD_EXECUTABLE(test_range_proof test/test_range_proof.cpp)
TARGET_LINK_LIBRARIES(test_range_proof ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# cryptocurrency
ADD_EXECUTABLE(test_adcp test/test_adcp.cpp)
TARGET_LINK_LIBRARIES(test_adcp ${OPENSSL_LIBRARIES} OpenMP::OpenMP_CXX)

# mcl
# add_executable(test_mcl test/test_mcl.cpp)
# target_link_libraries(test_mcl libmcl.a libgmp.a)


