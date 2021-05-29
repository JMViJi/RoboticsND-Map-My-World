# RoboticsND-Map-My-World
Fourth project of the Robotics Software Engineer Nanodegree.

# Introduction

Fourth project in wich we have the aim to create a 2D occupancy grid and 3D octomap from a simulated environment using our previous robot with the RTAB-Map package. 

# Prerequisites
Since RTAB-Map Pacakge will be used, the package must be installed (https://github.com/introlab/rtabmap_ros)
```
$ sudo apt-get install ros-kinetic-rtabmap-ros
```

# Working area

For this project my_world is the same as the previous one used in RoboticsND-Where-Am-I project.

# Robot

Althought the same as in the last project, we need to use a Kinect camera for RTAB-Map, so an upgrade of our actual camera is necesary.

The my_robot.xacro file needs an extra code in camera section.
```
  <joint name="camera_optical_joint" type="fixed">
    <origin xyz="0 0 0" rpy="-1.5707 0 -1.5707"/>
    <parent link="camera"/>
    <child link="camera_link_optical"/>
  </joint>

  <link name="camera_link_optical">
  </link>
```  
 And the same must be done with my_robot.gazebo, substitute the actual camera code for the followign one:
 ```
 <!-- RGBD Camera -->
  <gazebo reference="camera">
    <sensor type="depth" name="camera1">
        <always_on>1</always_on>
        <update_rate>20.0</update_rate>
        <visualize>true</visualize>             
        <camera>
            <horizontal_fov>1.047</horizontal_fov>  
            <image>
                <width>640</width>
                <height>480</height>
                <format>R8G8B8</format>
            </image>
            <depth_camera>

            </depth_camera>
            <clip>
                <near>0.1</near>
                <far>20</far>
            </clip>
        </camera>
         <plugin name="camera_controller" filename="libgazebo_ros_openni_kinect.so">
            <alwaysOn>true</alwaysOn>
            <updateRate>10.0</updateRate>
            <cameraName>camera</cameraName>
            <frameName>camera_link_optical</frameName>                   
            <imageTopicName>rgb/image_raw</imageTopicName>
            <depthImageTopicName>depth/image_raw</depthImageTopicName>
            <pointCloudTopicName>depth/points</pointCloudTopicName>
            <cameraInfoTopicName>rgb/camera_info</cameraInfoTopicName>              
            <depthImageCameraInfoTopicName>depth/camera_info</depthImageCameraInfoTopicName>            
            <pointCloudCutoff>0.4</pointCloudCutoff>                
                <hackBaseline>0.07</hackBaseline>
                <distortionK1>0.0</distortionK1>
                <distortionK2>0.0</distortionK2>
                <distortionK3>0.0</distortionK3>
                <distortionT1>0.0</distortionT1>
                <distortionT2>0.0</distortionT2>
            <CxPrime>0.0</CxPrime>
            <Cx>0.0</Cx>
            <Cy>0.0</Cy>
            <focalLength>0.0</focalLength>
            </plugin>
    </sensor>
  </gazebo>
 ```
 # Launch

  1. Launch our Gazebo world and RVIZ

```
roslaunch my_robot world.launch
```
  
  2. Launch teleop node to control robot movement
   
```
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```

  3. Launch mapping node

```
roslaunch my_robot mapping.launch
```
 # Results
