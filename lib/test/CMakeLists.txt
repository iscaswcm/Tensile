
set(INSTALL_GTEST OFF)
add_subdirectory(googletest)

if(HIP_FOUND)
set(CMAKE_CXX_COMPILER ${HIP_HIPCC_EXECUTABLE})
endif()

set(tensile_test_source
    test.cpp)

set(test_sources
    test.cpp
    GEMMLibraryLoading_test.cpp
    GEMMProblem_test.cpp
    GEMMSelectionLibrary_test.cpp
    GEMMSolution_test.cpp
    KernelArguments_test.cpp
    TensorDescriptor_test.cpp
    llvm/LLVMYAML_test.cpp
)

#add_library(TensileCommonTests STATIC ${test_sources})

#target_include_directories (tests PUBLIC "/opt/rocm/rocblas/include" ".")

include_directories("." ${GTEST_SOURCE_DIR}/include)


add_executable(TensileTests ${test_sources})

target_link_libraries(TensileTests gtest Tensile)

file(COPY sample_library.yaml DESTINATION .)

if(HIP_FOUND)
    add_subdirectory(hip)

    target_link_libraries(TensileTests TensileHipTests rocblas)
    # ${HIP_LIBRARIES}
endif()




#target_include_directories (test PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")