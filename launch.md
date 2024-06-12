## launch 文件 （planning simulation）

xml格式：

```
<launch>             //这是所有 ROS 启动文件的根元素。所有的 ROS 启动配置都被包含在 <launch> 标签中。
  <group>            //标签用于将相关的配置组合在一起，可以对一组节点或设置施加相同的命名空间或条件。使用 <group> 有助于组织代码，使启动文件更加模块化。
    <let>            //（<param> 在 ROS 1中）<let> 在 ROS 2中用于在当前上下文内定义局部变量。这些变量可以在 <group> 或 <launch> 的范围内用来控制条件逻辑或配置。它们通常不对外部环境暴露，主要用于内部逻辑决策。
      <include>      //<include> 标签用于引入另一个外部的 ROS 启动文件。这使得启动过程可以模块化，可以重用其他文件的配置。
        <arg name>   //<arg> 标签用于定义参数，这些参数可以在启动文件内部或通过命令行设置。


  </group>
</launch>
```
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <!-- Essential parameters -->
  <arg name="map_path" description="point cloud and lanelet2 map directory path"/>
  <arg name="vehicle_model" description="vehicle model name"/>
  <arg name="sensor_model" description="sensor model name"/>

  <!-- Optional parameters -->
  <!-- Map -->
  <arg name="lanelet2_map_file" default="lanelet2_map.osm" description="lanelet2 map file name"/>
  <arg name="pointcloud_map_file" default="pointcloud_map.pcd" description="pointcloud map file name"/>
  <!-- Control -->
  <arg name="enable_obstacle_collision_checker" default="false" description="use obstacle_collision_checker"/>
  <!-- System -->
  <arg name="launch_system_monitor" default="false" description="launch system monitor"/>
  <!-- Tools -->
  <arg name="rviz" default="true" description="launch rviz"/>
  
  <!-- 调用了autoware.rviz文件，用于设置 RVIZ摄像头视雷达数据可视化-->
  <arg name="rviz_config" default="$(find-pkg-share autoware_launch)/rviz/autoware.rviz" description="rviz config"/>
  <!-- Scenario simulation -->
  <arg name="initial_engage_state" default="true" description="/vehicle/engage state after starting Autoware"/>
  <arg name="perception/enable_detection_failure" default="true" description="enable to simulate detection failure when using dummy perception"/>
  <arg name="perception/enable_object_recognition" default="true" description="enable object recognition when using dummy perception"/>
  <arg name="sensing/visible_range" default="300.0" description="visible range when using dummy perception"/>
  <arg name="scenario_simulation" default="false" description="use scenario simulation"/>
  <!-- Vcu emulation -->
  <arg name="vehicle_simulation" default="true" description="use vehicle simulation"/>

  <group scoped="false">
    <!-- Vehicle -->
    <let name="launch_vehicle_interface" value="false" if="$(var vehicle_simulation)"/>
    <let name="launch_vehicle_interface" value="true" unless="$(var vehicle_simulation)"/>
    <!-- Tools -->
    <let name="launch_web_controller" value="false" if="$(var scenario_simulation)"/>
    <let name="launch_web_controller" value="true" unless="$(var scenario_simulation)"/>
    
  <!-- 调用了autoware.launch.xml文件，Autoware 的主启动文件-->
    <include file="$(find-pkg-share autoware_launch)/launch/autoware.launch.xml">
      <!-- Common -->
      <arg name="map_path" value="$(var map_path)"/>
      <arg name="vehicle_model" value="$(var vehicle_model)"/>
      <arg name="sensor_model" value="$(var sensor_model)"/>
      <!-- Modules to be launched -->
      <arg name="launch_sensing" value="false"/>
      <arg name="launch_localization" value="false"/>
      <arg name="launch_perception" value="false"/>
      <!-- Pointcloud container -->
      <arg name="use_pointcloud_container" value="false"/>
      <!-- Vehicle -->
      <arg name="launch_vehicle_interface" value="$(var launch_vehicle_interface)"/>
      <!-- System -->
      <arg name="system_run_mode" value="planning_simulation"/>
      <arg name="launch_system_monitor" value="$(var launch_system_monitor)"/>
      <!-- Map -->
      <arg name="lanelet2_map_file" value="$(var lanelet2_map_file)"/>
      <arg name="pointcloud_map_file" value="$(var pointcloud_map_file)"/>
      <!-- Control -->
      <arg name="enable_obstacle_collision_checker" value="$(var enable_obstacle_collision_checker)"/>
      <!-- Tools -->
      <arg name="rviz" value="$(var rviz)"/>
      <arg name="rviz_config" value="$(var rviz_config)"/>
      <arg name="launch_web_controller" value="$(var launch_web_controller)"/>
      <!-- Control -->
      <arg name="check_external_emergency_heartbeat" value="false"/>
    </include>
  </group>

  <!-- Simulator -->
  <group>
    <let name="launch_dummy_perception" value="false" if="$(var scenario_simulation)"/>
    <let name="launch_dummy_perception" value="true" unless="$(var scenario_simulation)"/>
    <let name="launch_dummy_vehicle" value="false" if="$(var scenario_simulation)"/>
    <let name="launch_dummy_vehicle" value="true" unless="$(var scenario_simulation)"/>
    <let name="launch_dummy_localization" value="true"/>
    
  <!-- 模拟器部分，用于启动虚拟的感知、车辆和定位模块，特别是在不接入真实硬件的情况下进行算法测试和系统验证 -->
    <include file="$(find-pkg-share tier4_simulator_launch)/launch/simulator.launch.xml">
      <arg name="launch_dummy_perception" value="$(var launch_dummy_perception)"/>
      <arg name="launch_dummy_vehicle" value="$(var launch_dummy_vehicle)"/>
      <arg name="launch_dummy_localization" value="$(var launch_dummy_localization)"/>
      <arg name="perception/enable_detection_failure" value="$(var perception/enable_detection_failure)"/>
      <arg name="perception/enable_object_recognition" value="$(var perception/enable_object_recognition)"/>
      <arg name="sensing/visible_range" value="$(var sensing/visible_range)"/>
      <arg name="vehicle_model" value="$(var vehicle_model)"/>
      <arg name="initial_engage_state" value="$(var initial_engage_state)"/>
      
  <!-- 车辆信息配置文件 -->      
      <arg name="vehicle_info_param_file" value="$(find-pkg-share $(var vehicle_model)_description)/config/vehicle_info.param.yaml"/>
    </include>
  </group>
</launch>
