cmake_minimum_required(VERSION 2.8.3)
project(apriltag)

set(CMAKE_BUILD_TYPE Debug)
set(ROS_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS "-std=c++17 -g -static-libstdc++ -Wno-address-of-packed-member")

find_package(OpenCV REQUIRED)

find_package(
    catkin REQUIRED COMPONENTS
    std_msgs
    roscpp
    tf
    message_generation
)
generate_messages(DEPENDENCIES std_msgs)
catkin_package(CATKIN_DEPENDS message_runtime)

#==============================================================================================
#apriltag

set(APRILTAG_INC
    ./src
    ./src/lib/apriltag/
    ./src/lib/apriltag/common)

set(APRILTAG_SRC
    ./src/lib/apriltag/apriltag.c
    ./src/lib/apriltag/apriltag_pose.c
    ./src/lib/apriltag/apriltag_quad_thresh.c
    ./src/lib/apriltag/common/g2d.c
    ./src/lib/apriltag/common/getopt.c
    ./src/lib/apriltag/common/homography.c
    ./src/lib/apriltag/common/image_u8.c
    ./src/lib/apriltag/common/image_u8x3.c
    ./src/lib/apriltag/common/image_u8x4.c
    ./src/lib/apriltag/common/matd.c
    ./src/lib/apriltag/common/pam.c
    ./src/lib/apriltag/common/pjpeg.c
    ./src/lib/apriltag/common/pjpeg-idct.c
    ./src/lib/apriltag/common/pnm.c
    ./src/lib/apriltag/common/string_util.c
    ./src/lib/apriltag/common/svd22.c
    ./src/lib/apriltag/common/time_util.c
    ./src/lib/apriltag/common/unionfind.c
    ./src/lib/apriltag/common/workerpool.c
    ./src/lib/apriltag/common/zarray.c
    ./src/lib/apriltag/common/zhash.c
    ./src/lib/apriltag/common/zmaxheap.c
    ./src/lib/apriltag/tag16h5.c
    ./src/lib/apriltag/tag25h9.c
    ./src/lib/apriltag/tag36h11.c
    ./src/lib/apriltag/tagCircle21h7.c
    ./src/lib/apriltag/tagCircle49h12.c
    ./src/lib/apriltag/tagCustom48h12.c
    ./src/lib/apriltag/tagStandard41h12.c
    ./src/lib/apriltag/tagStandard52h13.c
    ./src/main.cpp)

add_executable(apriltag ${APRILTAG_SRC})
include_directories(${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${APRILTAG_INC})
target_link_libraries (apriltag ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(apriltag ${PROJECT_NAME}_generate_messages_cpp)

#==============================================================================================
# intrinsic calibration

SET(CALIBRATION_INC
        ./src/
        ./src/calibration/)

SET(CALIBRATION_SRC
	./src/calibration/intrinsic_calibration.cpp)

add_executable(calibration ${CALIBRATION_SRC})
include_directories(${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${CALIBRATION_INC})
target_link_libraries (calibration ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(calibration ${PROJECT_NAME}_generate_messages_cpp)
