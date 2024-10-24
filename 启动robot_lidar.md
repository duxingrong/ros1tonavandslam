# base_control.launch

在功能包base_control中，他的主要作用是给各个参数设置默认值，并且根据不同的小车给出不同的imu_to_base的tf变换

由arg定义的参数，是可以在运行launch时候赋值的，他也就相当于一个变量

<arg name="base_type"  default="$(env BASE_TYPE)"/>
这是将环境中的$BASE_TYPE赋值给base_type，可以在命令行中运行`echo $BASE_TYPE` 来确定这个是多少


这里用了一个功能包,`robot_pose_ekf`估计机器人位姿的扩展卡尔曼滤波器（EKF）节点。它的主要作用是融合来自多个传感器的数据，以估计和更新机器人的位置和姿态，详细的文档查看[robot_pose_ekf](http://wiki.ros.org/robot_pose_ekf),简单来说就是订阅odom,imu,vo(视觉里程计),然后发布robot_pose_ekf/odom_combined，值得注意的是这里的他默认的imu_data,所以用的时候最好重映射

下面是它的一个简单列子:
 <launch>
  <node pkg="robot_pose_ekf" type="robot_pose_ekf" name="robot_pose_ekf">
    <param name="output_frame" value="odom"/>
    <param name="base_footprint_frame" value="base_link"/>
    <param name="freq" value="30.0"/> 频率
    <param name="sensor_timeout" value="1.0"/>  等待传感器的容忍度
    <param name="odom_used" value="true"/>  
    <param name="imu_used" value="true"/>
    <param name="vo_used" value="true"/>
    <param name="debug" value="false"/>
    <param name="self_diagnose" value="false"/>
  </node>
 </launch>


这里还有一个静态tf变换的例子:
<node pkg="tf" type="static_transform_publisher" name="imu_to_base" args="-0.03 0.04 0.08 0.0 0.0 0.0 '$(arg base_frame)' $(arg imu_frame) 20 " if="$(eval base_type=="NanoOmni)"/>

args="<x> <y> <z> <roll> <pitch> <yaw> <parent_frame> <child_frame> <frequency>"固定公式 ，这里加上单引号是更加规范

这里如果你启动的时候输入的robot_name 例如robot_0,那会有robot_0/base_footprint,robot_0/odom,robot_0/imu


# robot_lidar.launch

这个launch文件在robot_navigation功能包中,它的主要内容是开启底盘控制和激光雷达

首先是对参数进行了配置
<arg name="pub_imu"  default="False"/>
这里为什么不开启imu呢?因为前面使用了robot_pose_ekf功能包来估计机器人姿态
imu可以实时测量物体的欧拉角，线加速度，角速度

<arg name="sub_ackermann" default="False"/>
这个默认是对的，因为我们的小车是全向轮

<arg name="lidar_frame" default="base_laser_link"/>
将雷达的frame默认为base_laser_link

启动底盘的launch文件
<include file="$(find base_control)/launch/base_control.launch"
    <arg name="pub_imu" value="$(arg pub_imu)"/>
    <arg name="sub_ackermann"  value="$(arg sub_ackermann)"/>
</include>

然后启动雷达lidar.launch文件
<include file="$(find robot_navigation)/launch/lidar.launch">
    <arg name="lidar_frame"            value="$(arg lidar_frame)"/>  
</include>



# lidar.launch文件
首先注意:
<arg name="lidar_type" default="$(env LIDAR_TYPE)"/> 需要自己去命令行看

然后根据型号启动了不同的雷达launch文件
<include file="$(find robot_navigation)/launch/lidar/$(arg lidar_type).launch">
    <arg name="Lidar_frame"  value="$(arg lidar_frame)"/>
</include>

这里也进行了一个静态的tf变换
<group if="$(eval base_type=="NanoOmni")">
    <group unless="$(eval lidar_type=="yp350")>
        <node pkg="tf" type="static_transform_publisher" name="base_footprint_to_laser" arg="-0.05188 0.0 0.16 3.1415926 0.0 0.0 $(arg base_frame) $(arg lidar_frame) 20">
        </node>
    </group>
</group>




# rplidar.launch
这里主要是利用了功能包`rplidar_ros`,网址[rplidar_ros](http://wiki.ros.org/rplidar_ros) 

<launch>
    <arg name="lidar_frame" default="lidar"/>   
    <node name="rplidarNode"          pkg="rplidar_ros"  type="rplidarNode" output="screen">
        <param name="serial_port"         type="string" value="/dev/lidar"/> 端口
        <param name="serial_baudrate"     type="int"    value="115200"/><!--A1/A2 --> 波特率
        <!--param name="serial_baudrate"     type="int"    value="256000"--><!--A3 -->
        <param name="frame_id"            type="string" value="$(arg lidar_frame)"/> 雷达坐标系名称
        <param name="inverted"            type="bool"   value="false"/> 是否反转雷达数据
        <param name="angle_compensate"    type="bool"   value="true"/>  是否角度补偿
    </node>
</launch>





