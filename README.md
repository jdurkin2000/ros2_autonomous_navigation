# Final Project

ROS 2 Python package that performs frontier-based exploration. The `explore`
node searches an occupancy grid for unexplored frontiers, selects a frontier,
and sends it to Nav2 through the `FollowWaypoints` action.

## Requirements

- ROS 2 with `rclpy`
- Nav2 with the `FollowWaypoints` action server enabled
- A running localization and navigation stack publishing:
  - `/map` (`nav_msgs/msg/OccupancyGrid`)
  - `/odom` (`nav_msgs/msg/Odometry`)
  - `/follow_waypoints` (`nav2_msgs/action/FollowWaypoints`)
- Python dependencies: `numpy`

## Build

From the workspace root:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
rosdep install --from-paths src --ignore-src -r -y
colcon build --packages-select final_project
source install/setup.bash
```

## Run

Start the simulator, localization, and Nav2 stack first. Then run:

```bash
source install/setup.bash
ros2 run final_project explore
```

The node publishes an initial pose on `initialpose`, waits for an odometry
pose and occupancy grid, finds frontier centroids, and repeatedly sends the
selected frontier to Nav2. The initial pose and startup waypoints are currently
defined in `final_project/wavefront_frontier.py`.

## Tests

Run the package tests from the workspace root:

```bash
colcon test --packages-select final_project
colcon test-result --verbose
```

## Package Layout

- `final_project/wavefront_frontier.py` - frontier detection and ROS 2 node
- `test/` - copyright, formatting, and documentation checks
- `resource/final_project` - ament package marker

## License

Apache-2.0. See [LICENSE](LICENSE).