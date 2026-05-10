FROM ros:humble-ros-base

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=humble

RUN apt-get update && apt-get install -y \
    python3-vcstool \
    python3-rosdep \
    python3-colcon-common-extensions \
    ros-humble-gazebo-ros \
    ros-humble-gazebo-ros2-control \
    ros-humble-moveit \
    ros-humble-ros2-control \
    ros-humble-ros2-controllers \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /ros2_ws

COPY aidal_sim.repos .
RUN mkdir -p src && vcs import src < aidal_sim.repos

RUN rosdep update \
    && rosdep install -y \
        --from-paths src \
        --ignore-src \
        --rosdistro $ROS_DISTRO

RUN /bin/bash -c \
    "source /opt/ros/humble/setup.bash \
    && colcon build --symlink-install"

RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc \
    && echo "source /ros2_ws/install/setup.bash" >> ~/.bashrc
