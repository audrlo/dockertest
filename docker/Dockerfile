# Full ROS 2 Humble desktop (includes rviz, rqt, tf, etc.)
FROM ros:humble

# Basic development tools
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    python3-rosdep \
    python3-vcstool \
    build-essential \
    git \
    sudo \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Initialize rosdep
RUN rosdep init || true && rosdep update

# Set up workspace
WORKDIR /ros_ws
COPY ros_ws/src ./src

# Install dependencies (ignore missing)
RUN rosdep install --from-paths src --ignore-src -r -y || true

# Build workspace
RUN /bin/bash -c "source /opt/ros/humble/setup.bash && colcon build --symlink-install"

# Copy entrypoint
COPY docker/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]
