# Start from official ROS 2 Humble Desktop image (Ubuntu 22.04 base)
FROM osrf/ros:humble-desktop-full
ENV DEBIAN_FRONTEND=noninteractive

# # Install ros-humble-desktop package
# RUN apt-get update && apt-get install -y \
#     ros-humble-desktop \
#     && rm -rf /var/lib/apt/lists/*

# Install OpenCV (C++ libs + Python bindings)
RUN apt-get update && apt-get install -y \
    gedit libopencv-dev python3-opencv libglfw3-dev libglew-dev libncurses5-dev libncursesw5-dev libdw-dev ros-humble-mavros* ros-humble-pcl* ros-humble-rmw-zenoh-cpp\
    && rm -rf /var/lib/apt/lists/*

RUN apt-get install -y libeigen3-dev  \     
    && ln -s /usr/include/eigen3/Eigen /usr/include/Eigen

# Clone NLopt repository
RUN git clone https://github.com/stevengj/nlopt.git && \
    cd nlopt && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install

RUN apt-get update && apt-get install -y \
    ros-humble-cv-bridge \
    ros-humble-visualization-msgs \
    libnlopt-dev \
    libpcl-dev \
    python3-argcomplete \
    ros-dev-tools \
    && rm -rf /var/lib/apt/lists/*

# Source ROS setup automatically

RUN echo "source /opt/ros/humble/setup.bash" >> /etc/bash.bashrc
RUN echo "source /home/workspace/install/setup.bash" >> /etc/bash.bashrc
# RUN echo "export RMW_IMPLEMENTATION=rmw_zenoh_cpp" >> /etc/bash.bashrc
RUN echo "alias zenohd='ros2 run rmw_zenoh_cpp rmw_zenohd'" >> /etc/bash.bashrc
RUN echo "alias start='source install/setup.bash && ros2 launch plan_manage kino_replan_launch.xml'" >> /etc/bash.bashrc
RUN echo "alias build='colcon build --symlink-install && source install/setup.bash'" >> /etc/bash.bashrc

RUN echo "echo -e \"Run 'build' to build ROS2 ws, and 'start' to launch the ROS2 point click demo.\"" >> /etc/bash.bashrc


# Create a user (example: workspace) with a home directory
ARG USERNAME=workspace
ARG USER_UID=1001
ARG USER_GID=1001

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set permissions for the user
WORKDIR /home/$USERNAME
USER $USERNAME

ENV ROS2_WS /home/$USERNAME
WORKDIR $ROS2_WS

# Set the entrypoint
ENTRYPOINT ["/bin/bash"]
