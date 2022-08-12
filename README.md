#  使用方法：

1. 将ucar_sim包复制到工作空间src目录下

2. 先catkin_make编译，再 source ~/.bashrc 或 setup.bash

3.  启动导航

   一键启动仿真下的导航以及gazebo仿真
   
   ```
   roslaunch ucar_nav gazebo_navigation.launch
   ```
   
   一键启动小车导航（激光雷达，底盘，IMU，摄像头）
   
   ```
   roslaunch ucar_nav ucar_navigation.launch
   ```



#  Package说明

1. launch文件夹下存放: 一键启动的launch文件, 其下的config文件夹下存放导航参数

   > gazebo_navigation.launch 为仿真下的导航的启动文件

   > ucar_navigation.launch 为实体小车使用的的导航的启动文件

2. launch/config/move_base下为导航参数

   > costmap_common_params.yaml

   > costmap_converter_params.yaml

   > global_costmap_params.yaml

   > global_planner_params.yaml

   > local_costmap_params.yaml

   > teb_local_planner_params.yaml

3. maps文件夹下存放地图

4. rviz文件夹下存放rviz配置



#  自定义（仿真篇）

1. 修改启动的仿真的世界模型

   修改 launch/config 下的 gazebo_navigation.launch

   ```
     <!-- gazebo -->
     <include file="$(find gazebo_ros)/launch/empty_world.launch">
       <arg name="world_name" value="$(find ucar_sim)/world/arena-3.world"/> 
       <arg name="debug" value="$(arg debug)" />
       <arg name="gui" value="$(arg gui)" />
     </include>
   ```

   其中 arg name="world_name" value="$(find ucar_sim)/world/arena-3.world"/ 将最后的arena-3.world修改为自己的world即可启动自己的仿真世界



2. 修改机器人在gazebo中出生的位姿

   修改 launch/config 下的 gazebo_navigation.launch

   ```
     <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen"
   	args="-urdf -model robot -param robot_description -x -0.05 -y -0.1 -z 0.05"/> 
   ```

   修改其中 args="-urdf -model robot -param robot_description -x -0.05 -y -0.1 -z 0.05" 即可（注：-x  -y  -z指坐标， -X -Y -Z -W指四元数, 需自行添加）



3. 修改导航使用的地图

   修改 launch/config 下的 gazebo_navigation.launch

   ```
     <node name="map_server" pkg="map_server" type="map_server" args="$(find ucar_nav)/maps/map.yaml" output="screen">
      <param name="frame_id" value="map" />
     </node> 
   ```

   修改其中 args="$(find ucar_nav)/maps/map.yaml 为想要使用的地图的路径 （默认地图存放在包目录下的maps文件夹内）



#  自定义（实车篇）

1. 修改导航使用的地图

   修改 launch/config 下的 ucar_navigation.launch

   ```
     <node name="map_server" pkg="map_server" type="map_server" args="$(find ucar_nav)/maps/map.yaml" output="screen">
      <param name="frame_id" value="map" />
     </node> 
   ```

   修改其中 args="$(find ucar_nav)/maps/map.yaml 为想要使用的地图的路径 （默认地图存放在包目录下的maps文件夹内）



#   参考：

导航功能包：https://github.com/chenfu-user/xunfei

