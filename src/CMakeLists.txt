# Create a library of all the shared functions.
add_library(LTE_MISC capbuf.cpp constants.cpp itpp_ext.cpp macros.cpp searcher.cpp common.cpp dsp.cpp lte_lib.cpp)

# Create the cell search executable
ADD_EXECUTABLE (CellSearch CellSearch.cpp)
SET (common_link_libs ${Boost_LIBRARIES} ${LAPACK_LIBRARIES} ${FFTW_LIBRARIES} ${RTLSDR_LIBRARIES})
TARGET_LINK_LIBRARIES (CellSearch general LTE_MISC)
TARGET_LINK_LIBRARIES (CellSearch debug itpp_debug ${common_link_libs})
TARGET_LINK_LIBRARIES (CellSearch optimized itpp ${common_link_libs})

# Create a test that tests the overall functionality of the cell searcher.
# The tests in the 'tests' subdirectory test some of the smaller internal
# functions.
ADD_TEST(FullTest CellSearch -s 739000000 -l -d "${PROJECT_SOURCE_DIR}/test")
SET_TESTS_PROPERTIES(FullTest PROPERTIES PASS_REGULAR_EXPRESSION cell.ID..271)

INSTALL( TARGETS CellSearch DESTINATION bin )

