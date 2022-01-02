# Motion Capture System Setup

This is the tutorial for setting up Motion Capture System at SDSU SMILE Lab

## Hardware Setup
<img src="https://github.com/SMILE-SDSU/Motion-Capture-System-Setup/blob/master/MCS.png?raw=true" width="450" height="240"/>
- Hardware Components: Cameras, Network Switch with (Power over Ethernet) PoE, Computer with Motive installed (Host), Computer (Guest) that gets data streaming from Motion Capture System.
- All cameras and computers are connected to Network Switch throught network adapters so that they are in the same local network. 
- The cameras need to be calibrated by following procedures in this [website] (https://v22.wiki.optitrack.com/index.php?title=Calibration). Note that during the calbration the number of samples should not exceed 10000. Generally, 2,000 - 5,000 samples are enough even if it shows poor quality.

## Software Setup
- To connect Guest to the Local Network, specify Link-Local Only option in network configuration as shown below
<img src="https://github.com/SMILE-SDSU/Motion-Capture-System-Setup/blob/master/network.png?raw=true" width="360" height="300"/>

## Data Streaming with ROS in Guest Computer
- Usages and installation steps of ROS can be found at http://wiki.ros.org/ROS/Tutorials
- After installing ROS, ROS package 'vrpn_client_ros' need to installed for data streaming with ROS by 'sudo apt-get install ros-<ROS Version>-vrpn-client-ros'
- Configuring Motive: To activate the VRPN streaming engine, activate the checkbox Broadcast Frame Data in the VRPN Streaming Engine group. Set the VRPN Broadcast Port if necessary (default: 3883). The local interface should specify IP address of the Guest computer in the local network.
<img src="https://github.com/SMILE-SDSU/Motion-Capture-System-Setup/blob/master/motive_streaming_configs.png?raw=true" width="200" height="400"/>
  
 - Configuring the launch file of ROS as follows:
  ```
  <launch>
  <arg name="server" default="128.130.39.61"/>
  <node pkg="vrpn_client_ros" type="vrpn_client_node" name="vrpn_client_node" output="screen">
    <rosparam subst_value="true">
      server: $(arg server)
      port: 3883
      frame_id: world
      broadcast_tf: true
      # Must either specify refresh frequency > 0.0, or a list of trackers to create
      refresh_tracker_frequency: 1.0
      #trackers:
      #- FirstTracker
      #- SecondTracker
    </rosparam>
  </node>
</launch>
```
The server specifies the server's IP address. For more information, please see [1](http://wiki.ros.org/vrpn_client_ros) and [2](https://tuw-cpsg.github.io/tutorials/optitrack-and-ros/) for references.
  
- Visualization in ROS with rviz: Running the command to visualize the streamming data: `roslaunch sandbox vrpn.launch server:=128.130.39.61 & rosrun rviz rviz`
  
  <img src="https://github.com/SMILE-SDSU/Motion-Capture-System-Setup/blob/master/rviz.png?raw=true" width="450" height="300"/>
  <br />
  
  
  
