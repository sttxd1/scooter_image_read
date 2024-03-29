cmake_minimum_required(VERSION 3.14)
project(ros2_bag_to_image)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
# Find and include PCL
find_package(PCL REQUIRED COMPONENTS common io)
include_directories(${PCL_INCLUDE_DIRS})
add_definitions(${PCL_DEFINITIONS})

find_package (OpenCV REQUIRED)
find_package (Boost REQUIRED)

# topic_to_image
ament_auto_add_library(topic_to_image SHARED
        src/topic_to_image.cpp
        )

target_link_libraries(topic_to_image ${PCL_LIBRARIES}
        )

target_include_directories(topic_to_image
        SYSTEM PUBLIC
        ${OpenCV_INCLUDE_DIRS}
        )

rclcpp_components_register_node(topic_to_image
        PLUGIN "TopicToImage"
        EXECUTABLE topic_to_image_node
        )

# topic_to_image
ament_auto_add_library(bag_to_image SHARED
        src/bag_to_image.cpp
        )

target_link_libraries(bag_to_image ${PCL_LIBRARIES}
        )

target_include_directories(bag_to_image
        SYSTEM PUBLIC
        ${OpenCV_INCLUDE_DIRS}
        )

rclcpp_components_register_node(bag_to_image
        PLUGIN "BagToImage"
        EXECUTABLE bag_to_image_node
        )
######

ament_auto_package(INSTALL_TO_SHARE
        launch
        )

