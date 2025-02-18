if (${PYQUBO_BUILD_SHARED_LIBS})
  pybind11_add_module(cpp_pyqubo SHARED
    pybind_mapping.cpp
    express.cpp
    coeff.cpp
    encoder.cpp
    placeholderpoly.cpp
    expanded.cpp
    poly.cpp
  )
else()
  pybind11_add_module(cpp_pyqubo MODULE
    pybind_mapping.cpp
    express.cpp
    coeff.cpp
    encoder.cpp
    placeholderpoly.cpp
    expanded.cpp
    poly.cpp
  )
endif()

set_target_properties(cpp_pyqubo PROPERTIES
    CXX_VISIBILITY_PRESET "hidden"
    VISIBLITY_INLINES_HIDDEN ON
    INTERPROCEDURAL_OPTIMIZATION TRUE
    POSITION_INDEPENDENT_CODE ON
    PREFIX "${PYTHON_MODULE_PREFIX}"
    SUFFIX "${PYTHON_MODULE_EXTENSION}"
)

target_link_libraries(cpp_pyqubo PRIVATE 
    pybind11::thin_lto
    $<$<TARGET_EXISTS:OpenMP::OpenMP_CXX>:OpenMP::OpenMP_CXX>
)

if (PYQUBO_BUILD_SHARED_LIBS)
  add_library(cpp_pyqubo_body SHARED
    express.cpp
    coeff.cpp
    encoder.cpp
    placeholderpoly.cpp
    expanded.cpp
    poly.cpp
  )
else()
  add_library(cpp_pyqubo_body STATIC
    express.cpp
    coeff.cpp
    encoder.cpp
    placeholderpoly.cpp
    expanded.cpp
    poly.cpp
  )
endif()
target_include_directories(cpp_pyqubo_body PUBLIC ${PROJECT_SOURCE_DIR}/cpp_dimod)
target_include_directories(cpp_pyqubo_body PUBLIC ${PROJECT_SOURCE_DIR}/include)

target_include_directories(cpp_pyqubo PRIVATE ${PROJECT_SOURCE_DIR}/cpp_dimod)
target_include_directories(cpp_pyqubo PRIVATE ${PROJECT_SOURCE_DIR}/include)
