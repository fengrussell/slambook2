cmake_minimum_required(VERSION 2.8)
project(vo1)

set(CMAKE_BUILD_TYPE "Release")
add_definitions("-DENABLE_SSE")
set(CMAKE_CXX_FLAGS "-std=c++14 -O2 ${SSE_FLAGS} -msse4")
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(OpenCV 3 REQUIRED)

set(CMAKE_PREFIX_PATH "/home/russell/code/github/g2o/lib")
set(G2O_INCLUDE_DIRS "/home/russell/code/github/g2o/build")
find_package(G2O REQUIRED)
find_package(Sophus REQUIRED)

include_directories(
        ${OpenCV_INCLUDE_DIRS}
        ${G2O_INCLUDE_DIRS}
        ${Sophus_INCLUDE_DIRS}
        "/usr/include/eigen3/"
        "/home/russell/code/github/g2o/build"
        "/home/russell/code/github/g2o/"
        "/home/russell/code/github/Sophus"
)

add_executable(orb_cv orb_cv.cpp)
target_link_libraries(orb_cv ${OpenCV_LIBS})

add_executable(orb_self orb_self.cpp)
target_link_libraries(orb_self ${OpenCV_LIBS})

link_directories(/home/russell/code/github/g2o/lib/)
SET(G2O_LIBS g2o_cli g2o_ext_freeglut_minimal g2o_simulator g2o_solver_slam2d_linear g2o_types_icp g2o_types_slam2d g2o_core g2o_interface g2o_solver_csparse g2o_solver_structure_only g2o_types_sba g2o_types_slam3d g2o_csparse_extension g2o_opengl_helper g2o_solver_dense g2o_stuff g2o_types_sclam2d g2o_parser g2o_solver_pcg g2o_types_data g2o_types_sim3 cxsparse )

# add_executable( pose_estimation_2d2d pose_estimation_2d2d.cpp extra.cpp ) # use this if in OpenCV2 
add_executable(pose_estimation_2d2d pose_estimation_2d2d.cpp)
target_link_libraries(pose_estimation_2d2d ${OpenCV_LIBS} )

# # add_executable( triangulation triangulation.cpp extra.cpp) # use this if in opencv2
add_executable(triangulation triangulation.cpp)
target_link_libraries(triangulation ${OpenCV_LIBS} )

add_executable(pose_estimation_3d2d pose_estimation_3d2d.cpp)
target_link_libraries(pose_estimation_3d2d
        # g2o_core g2o_stuff
        ${OpenCV_LIBS} ${G2O_LIBS} fmt)

add_executable(pose_estimation_3d3d pose_estimation_3d3d.cpp)
target_link_libraries(pose_estimation_3d3d
        # g2o_core g2o_stuff
        ${OpenCV_LIBS} ${G2O_LIBS} fmt)
