# 为了实现三维实时建图

## 首先第一步,就是如何实时接受点云话题pointcloud.msg

方案一:
    - 我们使用ros-sharp+u盘中的PointCloudStreaming,只需要在untiy中导入该包,然后创建空物体rosConnector,添加rosConnector+这个pointCloudSubscriber.cs,设置好ip地址和话题
    - 将enableOpenGL.cs添加到MainCamera中,创建新物体,添加Render.cs,在添加空物体,取名字叫做pointcloud_origin,然后将rosConnector物体和origin物体分别放入Render的脚本中.
    - 在ros端还要运行ros_bridge来当作桥梁

方案二:
    - 我们使用ros-tcp-connector来实现这个功能,只需要安装ros-tcp-connector和它其中的可视化包Unity Robotics Visualizations,导入后在window按钮旁边robotics中setting,设置相应的ip以及ros版本
    - 在ros端需要运行ros_tcp_endpoint 
    - 直接就可以将很多消息都可视化


问题: 方案一延迟小于方案二，方案二可以可视化的topic更多,且更方便.但是这两钟方法都没有办法实现实时接受/mapdata数据,也就是实现不了三维实时可视化.

依旧还是需要其他的方法来实现，目前也只能试一试论文中提到的VFX来渲染地图.
