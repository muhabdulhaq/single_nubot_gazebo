# Description 
This simulation system can simulate one robot soccer player for RoboCup Middle Size League. It can be adapted for other purposes.

# Recommended Operating Environment
1. Ubuntu 14.04; 
2. ROS Indigo; 
3. Gazebo 5.0 or 5.1;
4. gazebo_ros_pkgs;

> **NOTE:** 
> If you choose "desktop-full" install of ROS Indigo, there is a Gazebo 2.0 included initially. In order to install Gazebo 5.0/5.1, you should first remove Gazebo 2.0 by running:   
` $ sudo apt-get remove gazebo2* `  
> Then you should be able to install Gazebo 5.0 now. To install gazebo_ros_pkgs compatible with Gazebo
> 5.0/5.1, run this command:  
` $ sudo apt-get install ros-indigo-gazebo5-ros-pkgs ros-indigo-gazebo5-ros-control`

    
Other versions of Ubuntu, ROS or Gazebo may also work, but we have not tested yet.

# Complie
1. Go to the package root directory (single_nubot_gazebo)
2. If you already have CMakeLists.txt in the "src" folder, then you can skip this step. 
   If not, run these commands:
       
    ```
    $ cd src
    $ catkin_init_workspace
    $ cd ..
    ```
3. $ ./configure
4. $ catkin_make --pkg nubot_common
5. $ catkin_make

# Tutorials

## Part I. Overview
The robot movement is realized by a Gazebo model plugin which is called "NubotGazebo" generated by source files "nubot_gazebo.cc" and "nubot_gazebo.hh". Basically the essential part of the plugin is realizing basic motions: omnidirectional locomotion, ball-dribbling and ball-kicking.

Basically, this plugin subscribes to topic "nubotcontrol/velcmd" for omnidirecitonal movement and subscribes to service "BallHandle" and "Shoot" for ball-dribbling and ball-kicking respectively. You can customize this code for your robot based on these messages and services as a convenient interface.

As for ball-dribbling, there are three ways for a robot to dribble a ball, i.e.
            
Method  | Description
:-----: | -------------
(a) Setting ball pose continually  | This is the most accurate one; nubot would hardly lose control of the ball, but the visual effect is not very good (the ball does not rotate).
(b) Setting ball secant velocity  | This is less acurate than method (a) but more accurate than method (c).
(c) Setting ball tangential velocity |  This is the least accurate. If the robot moves fast, such as 3 m/s, it would probably lose control of the ball. However, this method achieves the best visual effect under low-speed condition.
**By default, we use method (c) for ball-dribbling.**
    
 As for Gaussian noise, **by default, Gaussian noise is NOT added**, but you can add it by changing the flag in nubot_gazebo.cc in function update_model_info();
         
Please read the paper "Weijia Yao et al., A Simulation System Based on ROS and Gazebo for RoboCup Middle Size League, 2015" for more information.
 
## Part II. Single robot automatic movement
 The robot will do motions according to states transfer graph. Steps are as follows:
 1. Go to the package root directory (single_nubot_gazebo)
 2. source the setup.bash file:   
   ` $ source devel/setup.bash`
 3.  `$ roslaunch nubot_gazebo sdf_nubot.launch`   
 
>  **Note:** Every time you open a new terminal, you have to do step 2. You can also write this command into the ~/.bashrc file so that you don't have to source it every time.

Finally, the robot rotates and translates with trajectory planning. That is, the robot accelerates at constant acceleration and stays at constant speed when it reaches the maximum velocity.
 
## Part III. Keyboad control robot movement
 1. In nubot_gazebo.cc, comment "nubot_auto_control();" and uncomment "nubot_be_control();" in function UpdateChild().
 2. Compile again and follow steps 1-3 listed in Part II.
 3. ` $ rosrun nubot_gazebo nubot_teleop_keyboard`
 
## Part IV. Appendix
  1. To launch an empty soccer field:   
  ` $ roslaunch nubot_gazebo empty_field.launch`
  2. To launch the simulation world with rqt_plot of nubot or ball's velocity:  
  ` $ roslaunch nubot_gazebo sdf_nubot.launch plot:=true`
