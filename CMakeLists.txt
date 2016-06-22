cmake_minimum_required(VERSION 2.8.3)
project(vocabulary_tree)

find_package(catkin REQUIRED COMPONENTS
#  opencv2
  roscpp
  cmake_modules
  cv_bridge
)

find_package(OpenCV REQUIRED)

################################################
#                                              #
#       CATKIN SPECIFIC CONFIGURATION          #
#                                              #
################################################

catkin_package(
    LIBRARIES ${PROJECT_NAME}
    INCLUDE_DIRS include
    DEPENDS cv_bridge
)

################################################
#                                              #
#                    BUILD                     #
#                                              #
################################################

# SSE
#rosbuild_check_for_sse()
#add_definitions(${SSE_FLAGS})
add_definitions(-DFOVIS_USE_SSE)

find_package(Eigen REQUIRED)
#find_package(OpenCV)
include_directories(include SYSTEM ${Eigen_INCLUDE_DIRS})

#target_link_libraries(my_awesome_library ${OpenCV_LIBRARIES})

add_library(${PROJECT_NAME}
    src/database.cpp
    src/generic_tree.cpp
)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

add_executable(make_tree src/make_tree.cpp)

add_executable(train_weights src/train_weights.cpp)
target_link_libraries(train_weights ${PROJECT_NAME})

add_executable(make_tree_gaussian src/make_tree_gaussian.cpp)

add_executable(test_kmeans test/test_kmeans.cpp)
add_executable(test_kmeans_hier test/test_kmeans_hier.cpp)

add_executable(test_distance test/test_distance.cpp)

add_executable(make_test_tree test/make_test_tree.cpp)

add_executable(find_word test/find_word.cpp)

add_executable(test_database test/test_database.cpp)
target_link_libraries(test_database ${PROJECT_NAME})

add_executable(test_generic_tree test/test_generic_tree.cpp)
target_link_libraries(test_generic_tree ${PROJECT_NAME})

