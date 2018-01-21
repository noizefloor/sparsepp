cmake_minimum_required(VERSION 3.5)

project(sparsepp)

option(SPARSEPP_ENABLE_TESTING "Enable testing of the sparsehash library." ON)
option(SPARSEPP_BUILD_EXAMPLES "Build examples." OFF)

# Library

add_library(sparsepp INTERFACE)

target_compile_features(sparsepp INTERFACE cxx_std_11)

target_include_directories(sparsepp
        INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/sparsepp>
        $<INSTALL_INTERFACE:include/sparsepp>)

install(DIRECTORY sparsepp/ DESTINATION include/sparsepp)

install(TARGETS sparsepp EXPORT sparsepp-config DESTINATION sparsepp/include)

install(EXPORT sparsepp-config DESTINATION share/sparsepp/cmake)


# UnitTests

if(SPARSEPP_ENABLE_TESTING)

    enable_testing()

    add_executable(perftest1 tests/perftest1.cc)
    add_executable(spp_alloc_test tests/spp_alloc_test.cc)
    add_executable(spp_test tests/spp_test.cc)

    target_compile_features(perftest1 INTERFACE cxx_std_11)
    target_compile_features(spp_alloc_test INTERFACE cxx_std_11)
    target_compile_features(spp_test INTERFACE cxx_std_11)

    target_link_libraries(perftest1 PRIVATE sparsepp)
    target_link_libraries(spp_alloc_test PRIVATE sparsepp)
    target_link_libraries(spp_test PRIVATE sparsepp)

    add_test(NAME perftest1 COMMAND perftest1)
    add_test(NAME spp_alloc_test COMMAND spp_alloc_test)
    add_test(NAME spp_test COMMAND spp_test)

endif()

# Examples

if(SPARSEPP_BUILD_EXAMPLES)
    add_executable(example_emplace examples/emplace.cc)
    add_executable(example_hash_std examples/hash_std.cc)
    add_executable(example_serialize_file examples/serialize_file.cc)
    add_executable(example_serialize_large examples/serialize_large.cc)
    add_executable(example_serialize_stream examples/serialize_stream.cc)

    target_link_libraries(example_emplace PRIVATE sparsepp)
    target_link_libraries(example_hash_std PRIVATE sparsepp)
    target_link_libraries(example_serialize_file PRIVATE sparsepp)
    target_link_libraries(example_serialize_large PRIVATE sparsepp)
    target_link_libraries(example_serialize_stream PRIVATE sparsepp)
endif()