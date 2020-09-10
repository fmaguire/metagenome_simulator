# configures building lib
add_executable(metamrsim metamrsim.cpp)
target_link_libraries(metamrsim ${SEQAN_LIBRARIES})
