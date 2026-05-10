FROM ros:humble-ros-base

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=humble

RUN apt-get update && apt-get install -y \
    python3-vcstool \
    python3-rosdep \
    python3-colcon-common-extensions \
    ros-humble-gazebo-ros-pkgs \
    ros-humble-gazebo-ros2-control \
    ros-humble-moveit \
    ros-humble-moveit-setup-assistant \
    ros-humble-moveit-ros-visualization \
    ros-humble-ros2-control \
    ros-humble-ros2-controllers \
    ros-humble-robot-state-publisher \
    ros-humble-joint-state-publisher \
    ros-humble-joint-state-publisher-gui \
    ros-humble-xacro \
    ros-humble-kinematics-interface \
    ros-humble-kinematics-interface-kdl \
    ros-humble-rviz2 \
    ros-humble-teleop-twist-keyboard \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /ros2_ws

COPY aidal_sim.repos .
RUN mkdir -p src && vcs import src < aidal_sim.repos

RUN cd /ros2_ws/src/ros2_rm_robot/rm_driver/lib \
    && bash lib_install.sh

RUN rosdep update \
    && rosdep install -y \
    --from-paths src \
    --ignore-src \
    --rosdistro $ROS_DISTRO || true

RUN /bin/bash -c \
    "source /opt/ros/humble/setup.bash \
    && colcon build --symlink-install"

RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc \
    && echo "source /ros2_ws/install/setup.bash" >> ~/.bashrc
