cmake_minimum_required(VERSION 2.8.3)
project(pal_vision_segmentation)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  stereo_image_proc
  cv_bridge
  dynamic_reconfigure
  geometry_msgs
  image_transport
  roscpp
)

## System dependencies are found with CMake's conventions
#find_package(OpenCV REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/DisparitySegment.cfg
  cfg/HistogramSegment.cfg
  cfg/TemplateSegment.cfg
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES pal_vision_segmentation
#  CATKIN_DEPENDS cv_bridge dynamic_reconfigure geometry_msgs image_transport opencv2 roscpp stereo_image_proc
#  DEPENDS system_lib
)

include_directories(include ${catkin_INCLUDE_DIRS})


add_library(${PROJECT_NAME}
    src/image_processing.cpp
    src/histogram.cpp)

## Specify libraries to link a library or executable target against
 target_link_libraries(${PROJECT_NAME}
   ${catkin_LIBRARIES}
 )


add_executable(disparity_segmentation
    src/disparity_segmentation.cpp)
add_dependencies(disparity_segmentation
    ${PROJECT_NAME}_gencfg)

target_link_libraries(disparity_segmentation
    ${PROJECT_NAME})


add_executable(template_segmentation
    src/template_segmentation.cpp
    src/image_processing.cpp)

add_dependencies(template_segmentation
    ${PROJECT_NAME}_gencfg)

target_link_libraries(template_segmentation
    ${PROJECT_NAME})




add_executable(histogram_segmentation
    src/histogram_segmentation.cpp)
add_dependencies(histogram_segmentation
    ${PROJECT_NAME}_gencfg)


target_link_libraries(histogram_segmentation
    ${PROJECT_NAME}
    )
#    ${OpenCV_LIBS}


## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(pal_vision_segmentation_node pal_vision_segmentation_generate_messages_cpp)


#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS pal_vision_segmentation pal_vision_segmentation_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
 catkin_add_gtest(${PROJECT_NAME}-test test/histogram_segmentation_unit_test.cpp)
 if(TARGET ${PROJECT_NAME}-test)
   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
 endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
