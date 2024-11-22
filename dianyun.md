# 点云的处理

在上面的方法中，我门实现了小车的自主建图，现在的目的就是将数据如何转变成模型导入unity中.

## rtabmap将.dB数据导出.ply文件(点云)

下载rtabmap
在终端运行
```bash
rtabmap
```
导入.dB文件
file->open database

处理:
Edit->download all clouds

导出.ply文件
file->export 3D clouds(默认就行)

## 如何导入unity中
首先，我们需要知道，unity默认只会接受.fbx和.obj的模型文件

### ros#

我们首先是想是否有unity和ros的直接通信，这样可以实时的将建立的3纬地图传进unity，根据查找，我们找到了一个插件ROS#,他可以实现unity中接受ros的topic，我们利用教程实现了数据的实时通信和图像的实时传输

但是问题就在于3D地图他自己是一个点云的数据，我们没法通过纹理渲染的方式来接受这些点云数据，所以实时的方式，我们失败了

### point cloud free view
回到起点，我们现在从离线方面上来突破，我们现在手上只有.ply的点云文件。通过csdn发现了一个办法:
1. 下载meshlab,将.ply文件保存为.off文件
2. 在unity store中下载point cloud free view(免费) 然后import项目中，完成后将.off文件放到Asset/pointCloud/下，这里最好将.off文件改名，就取数字(经验)
3. 创建空物体，将pointCloud/下的Enable..脚本添加到main camera下,然后将pointManager脚本放到空物体上，在路径那里写上/pointCloud/你.off文件的名字,将Mat Vertex 设置为VertexColor
4. 运行，显示出点云

基本成功了，但是最后一步发现这个插件不支持导入AR中，build失败

### Pcx
然后发现了一个更简单的Pcx插件，是第三方的插件，可以到youtobe 搜索pcx for unity ，在视频介绍中有pcx的下载地址

1. 在Asset中，import package->custom package 选择你下载的pcx,然后import 
2. 直接将.ply文件拖到Asset中，他已经识别到点云了,直接用即可


# 总结
这就是以上的经验总结,总结日期:11月22日




