# test122

```ruby
from launch import LaunchDescription
from launch_ros.actions import Node
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration
from ament_index_python.packages import get_package_share_directory
import os

def generate_launch_description():
    # Define the directory where your package configs are located
    share_dir = get_package_share_directory('your_package_name')

    # RViz2 configuration file path
    rviz_config_file = os.path.join(share_dir, 'config', 'slam_config.rviz')

    return LaunchDescription([
        Node(
            package='slam_toolbox',
            executable='sync_slam_toolbox_node',
            name='slam_toolbox',
            output='screen',
            parameters=[{'use_sim_time': False,
                         'map_update_interval': 5.0,
                         'resolution': 0.05,
                         'max_range': 10.0,
                         'minimum_travel_distance': 0.5,
                         'minimum_travel_heading': 0.5,
                         'scan_topic': '/scan',
                         'base_frame': 'base_link',
                         'odom_frame': 'odom',
                         'map_frame': 'map',
                         'provide_odom_frame': False,
                         'use_scan_matching': True,
                         'use_scan_barycenter': True,
                         'solver_type': 'ceres'}]
        ),
        Node(
            package='rviz2',
            executable='rviz2',
            name='rviz2',
            arguments=['-d', rviz_config_file],
            output='screen'
        )
    ])
```
