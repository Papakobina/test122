# test122

```ruby
from launch import LaunchDescription
from launch_ros.actions import Node
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration
from ament_index_python.packages import get_package_share_directory
import os

def generate_launch_description():
    # Base directory for your workspace
    base_dir = os.path.join('/home', 'pi', 'ydlidar_ros2_ws', 'src')

    # Path to the SLAM parameter file
    parameter_file = os.path.join(base_dir, 'config', 'slam_params.yaml')

    # Declare the use of parameter file as a Launch Argument
    params_declare = DeclareLaunchArgument(
        'params_file',
        default_value=parameter_file,
        description='Path to the parameter file for SLAM settings.'
    )

    # SLAM Toolbox Node Configuration
    slam_toolbox_node = Node(
        package='slam_toolbox',
        executable='async_slam_toolbox_node',
        name='slam_toolbox',
        output='screen',
        parameters=[LaunchConfiguration('params_file')]
    )

    # RViz Node Configuration
    rviz_config_file = os.path.join(base_dir, 'config', 'ydlidar.rviz')
    rviz_node = Node(
        package='rviz2',
        executable='rviz2',
        name='rviz2',
        arguments=['-d', rviz_config_file],
        output='screen'
    )

    return LaunchDescription([
        params_declare,
        slam_toolbox_node,
        rviz_node
    ])

```
