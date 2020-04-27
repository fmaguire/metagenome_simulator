# configures building lib
add_executable(metagenome_simulator metagenome_simulator.cpp)
target_link_libraries(metagenome_simulator ${SEQAN_LIBRARIES})
