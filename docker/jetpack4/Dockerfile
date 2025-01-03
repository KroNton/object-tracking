# Use the ultralytics Jetson base image
FROM ghcr.io/kronton/object-tracking-base-image:v0.0.1

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=melodic

# Remove NVIDIA internal repositories causing issues
RUN rm -f /etc/apt/sources.list.d/*cuda* /etc/apt/sources.list.d/*nvidia* \
    && rm -f /etc/apt/sources.list.d/*nvidia*

# Install essential dependencies and apt-utils to avoid debconf warning
RUN apt-get update && apt-get install -y \
    curl \
    gnupg2 \
    lsb-release \
    apt-utils \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install the missing 'distro' package for rosdep
RUN pip3 install distro

# Add ROS repository and key without using deprecated apt-key
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/ros.asc.gpg

# Add ROS repository to the list
RUN echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list

# Install ROS Base and only python3-rosdep (no conflict with python-rosdep)
RUN apt-get update && apt-get install -y \
    ros-melodic-ros-base \
    python3-rosdep \
    && rm -rf /var/lib/apt/lists/*

# Initialize rosdep
RUN rosdep init && rosdep update

# Set up ROS environment
RUN echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
SHELL ["/bin/bash", "-c"]

# Default command
CMD ["bash"]
