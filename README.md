# 前期工作
1. 需要一个路由器.并且知道网址和管理员密码
2. 将路由器连出一根网线到小车
3. 电脑下载NoMachine Service 二进制文件,解压`sudo dpkg -i 二进制文件` 
4. 电脑连接路由器的wifi,进入网址192.168.1.1 ,输入管理员密码:tp324123.
5. 打开NoMachine,看是否有小车.有的话点击.输入账号密码(均为bingda).然后进入
6. 在里面设置好无线
7. 命令行`sudo gedit /etc/hosts` ,将master_ip改成你的pc的ip地址(此时你的电脑连接的是路由器的wifi)
8. 命令行`gedit ~/.bashrc` 第129行修改为`export ROS_MASTER_URI=http://master_ip:11311`
9. 在你的电脑./bashrc中添加:
    #冰达
    export ROS_IP=`hostname -I | awk '{print $1}'`
    export ROS_HOSTNAME=`hostname -I | awk '{print $1}'`
    # export ROS_MASTER_URI=http://master_ip:11311
    export ROS_MASTER_URI=http://`hostname -I | awk '{print $1}'`:11311
10. `source ~/.bashrc`



