# Leonardo

<img src=http://www.volarecon.it/ridimensiona-dicodici.jpg width="850" title="Macchina volante di Leonardo">

## Introduction

Here are overviews of the operating system, software and concepts used in the project.

### Operating System - Linux

Linux is a Unix-like and POSIX compliant computer operating system, originally developed as a free and open-source software for Intel x86-based personal computers.
The defining component of Linux is the Linux kernel, released on 5 October 1991 by Linus Torvalds.
Linux is packaged in a form known as a Linux distribution. Some popular Linux distributions include Debian, Ubuntu, Linux Mint, Fedora, openSUSE, Arch Linux. Linux distributions include the Linux kernel, supporting utilities and libraries and usually a large amount of application software to fulfill the distribution's intended use.

#### Lubuntu

In this project we used the distribution known as Lubuntu. It's a fast and lightweight operating system. The core of the system is based on Linux and Ubuntu. Lubuntu uses the minimal desktop LXDE, and a selection of light applications. It is focused on speed and energy-efficiency. Because of this, Lubuntu has very low hardware requirements that make it suitable for the use on a virtual machine like in this project.

### ROS - Robot Operating System

ROS is a flexible framework for writing robot software. It is a collection of tools, libraries, and conventions that aim to simplify the task of creating complex and robust robot behavior across a wide variety of robotic platforms.
There are different versions of ROS, for the project we used ROS Indigo.

<img src="https://dl.dropboxusercontent.com/u/52788948/screenshot.png" width="800" title="Screenshot ROS">

#### Core components

- **Roscore**: Collection of nodes and programs that are pre-requisites of a ROS-based system. Must have a roscore running in order for ROS nodes to communicate.

- **Nodes**: A node is a process that performs computation. Nodes are combined together into a graph and communicate with one another using streaming topics, RPC services, and the Parameter Server. A robot control system will usually comprise many nodes.

- **Comunication protocol - Publish/Subscribe**: The publisher writes a message on a topic provided by the roscore. All nodes that want to receive the message can request it to roscore and they are known as subscribers.

#### Rviz - ROS visualization

Rviz is a 3D visualizer for displaying sensor data and state information from ROS. Display live representations of sensor values coming over ROS Topics including camera data, infrared distance measurements, sonar data, and more.

<img src="https://dl.dropboxusercontent.com/u/52788948/Screenshot%20from%202015-06-03%2020_32_12.png" width="800" title="Screenshot Rviz">

### Marker based navigation

The core problem of the project is the navigation of an ARDrone based on marker. In the marker based navigation the robot moves between two positions by following a
topological path consisting of a sequence of visual markers.
The details about the implementation are in the Marker detection chapter.

<img src="https://dl.dropboxusercontent.com/u/52788948/Marker%20navigation.PNG" title="Marker path" >

## AR Drone

<img src="https://dl.dropboxusercontent.com/u/52788948/IMG_20150511_171256.jpg" width="700" title="AR Drone with marker">

### Introduction

Parrot AR.Drone is a radio controlled flying quadcopter built by the French company Parrot. The drone is designed to be controlled by mobile or tablet operating systems such as the supported iOS or Android within their respective apps.
The airframe of the AR.Drone, constructed of nylon and carbon fiber parts, measures 57 cm across. Two interchangeable hulls were supplied with the airframe, one designed for indoor and one for external flight. The indoor hull is made from foam, and encases the circumference of the blades for protection. The outdoors use hull is made from lightweight plastic, and allows for increased maneuvrability. In total, the AR.Drone has six degrees of freedom, with a miniaturized inertial measurement unit tracking the pitch, roll and yaw for use in stabilisation.
Inside the airframe, a range of sensors assist flight, enabling the interface used by pilots to be simpler, and making advanced flight easier. The onboard computer runs a Linux operating system, and communicates with the pilot through a self-generated Wi-Fi hotspot. The onboard sensors include an ultrasonic altimeter, which is used to provide vertical stabilisation up to 6 m. The rotors are powered by 15 watt, brushless motors powered by an 11.1 Volt lithium polymer battery. This provides approximately 12 minutes of flight time at a speed of 5 m/s. Coupled with software on the piloting device, the forward facing camera allowed the drone to build a 3D environment, track objects and other drones.
In this project we used the **AR.drone version 2.0**, rather than the first version it was significantly upgraded to improve the drone's function. The camera quality was increased, and many of the onboard sensors were made more sensitive, allowing for greater control. The ultrasound altimeter was enhanced with the addition of an air pressure sensor, allowing for more stable flight and hovering. The resources available to the onboard computer were also improved, and the Wi-Fi hardware was updated to a newer standard. Other sensor improvements upgraded the gyroscope to a 3-axis version, along with a 3-axis accelerometer and magnetometer.

### Camera

The AR.drone version 2.0 provides two cameras with different features:
- **Front camera**: 720p sensor with 93° lens, recording up to 30fps;
- **Vertical camera**: QVGA sensor with 64° lens, recording up to 60fps.
In this project we used the vertical camera to recognize the markers. An important aspect is the standard reference system of the camera, from the center of which the z axis comes out.

<img src="http://blog.wirelesszone.com/Portals/41194/images/altimeter.jpg" title="AR drone bottom">

### Wi-fi connection

The connection between the AR.drone and the computer used for the control is created using the self-generated **Wi-Fi 802.11n** hotspot provided by the AR.drone. The Wi-Fi network has an ESSID usually called ardrone2_parrot, the DHCP server grants the client with an IP address which is like 192.168.1.x.

### Ardrone_autonomy

Ardrone_autonomy is a ROS driver for Parrot AR-Drone quadrocopter. This driver is based on official AR-Drone SDK version 2.0.1. The driver supports both AR-Drone 1.0 and 2.0.

#### Legacy navigation data

Information received from the drone will be published to the ardrone/navdata topic. The message type is ardrone_autonomy::Navdata and contains the following information:

- **header**: ROS message header;
- **batteryPercent**: The remaining charge of the drone's battery (%);
- **state**: The Drone's current state: 0: Unknown 1: Inited 2: Landed 3,7: Flying 4: Hovering 5: Test (?) 6: Taking off 8: Landing 9: Looping;
- **rotX**: Left/right tilt in degrees (rotation about the X axis);
- **rotY**: Forward/backward tilt in degrees (rotation about the Y axis);
- **rotZ**: Orientation in degrees (rotation about the Z axis);
- **magX, magY, magZ**: Magnetometer readings (AR-Drone 2.0 Only);
- **pressure**: Pressure sensed by Drone's barometer (AR-Drone 2.0 Only);
- **temp**: Temperature sensed by Drone's sensor (AR-Drone 2.0 Only);
- **wind_speed**: Estimated wind speed (AR-Drone 2.0 Only);
- **wind_angle**: Estimated wind angle (AR-Drone 2.0 Only);
- **wind_comp_angle**: Estimated wind angle compensation (AR-Drone 2.0 Only);
- **altd**: Estimated altitude (mm);
- **motor1..4**: Motor PWM values;
- **vx, vy, vz**: Linear velocity (mm/s);
- **ax, ay, az**: Linear acceleration (g);
- **tm**: Timestamp of the data returned by the Drone returned as number of micro-seconds passed since Drone's boot-up.

#### Cameras

As we have already said both AR-Drone 1.0 and 2.0 are equipped with two cameras. One frontal camera pointing forward and one vertical camera pointing downward. This driver will create three topics for each drone: ardrone/image_raw, ardrone/front/image_raw and ardrone/bottom/image_raw. Each of these three are standard ROS camera interface and publish messages of type image transport. The driver is also a standard ROS camera driver, therefor if camera calibration information is provided either as a set of ROS parameters or appropriate ardrone_front.yaml and/or ardrone_bottom.yaml, the information will be published in appropriate camera_info topics.
The ardrone will always contain the selected camera's video stream and information. Only one of ardrone/front or ardrone/bottom topics will be updated based on which camera is selected at the time.

#### Sending Commands to AR-Drone

The drone will takeoff, land or emergency stop/reset by publishing an Empty ROS messages to the following topics: ardrone/takeoff, ardrone/land and ardrone/reset respectively.

In order to fly the drone after takeoff, you can publish a message of type geometry_msgs::Twist to the cmd_vel topic.

    -linear.x: move backward
    +linear.x: move forward
    -linear.y: move right
    +linear.y: move left
    -linear.z: move down
    +linear.z: move up

    -angular.z: turn left
    +angular.z: turn right

The range for each component should be between -1.0 and 1.0. The maximum range can be configured using ROS parameters discussed later in this document.

geometry_msgs::Twist has two other member variable called angular.x and angular.y which can be used to enable/disable "auto-hover" mode. "auto-hover" is enabled when all six components are set to zero. If you want the drone not to enter "auto hover" mode in cases you set the first four components to zero, set angular.x and angular.y to arbitrary non-zero values.

#### Services

##### Toggle AR-Drone's Camera

Calling ardrone/togglecam service with no parameters will change the active video camera stream.

##### Flat Trim

Calling ardrone/flattrim service without any parameter will send a "Flat Trim" request to AR-Drone to re-calibrate its rotation estimates assuming that it is on a flat surface. Do not call this service while Drone is flying or while the drone is not actually on a flat surface.

## Marker detection

One of the main problem in marker based navigation is to identify a marker using the camera. Here are the library used to solve this issue.

### ARToolkit

ARToolKit is a software library for building Augmented Reality applications. These are applications that involve the overlay of virtual imagery on the real world. We used this library only at the beginning of the project, but...

### Aruco

Working on the project we found a better solution to the marker detection provided by the Aruco library.
Aruco is a minimal C++ library for Augmented Reality applications based on OpenCv developed by the University of Cordoba.

We also made a modify on the library deleting the code used to print on the console.

<img src="http://iplimage.com/blog/wp-content/uploads/2012/02/marker.png" width="250" title="Marker 23">

The library relies on the use of coded markers. Each marker has an unique code indicated by the black and white colors in it. The libary detect borders, and analyzes into the rectangular regions which of them are likely to be markers. Then, a decoding is performed and if the code is valid, it is considered that the rectangle is a marker.

The codification included into the marker is a slighly modified version of the Hamming Code. It has a total of 25 bits didived in 5 rows of 5 bits each. So, we have 5 words of 5 bits. Each word, contains only 2 bits of real information, the rest is for  and error detection/correction (error correction is yet to be done). As a conclusion, a marker contains 10 bits of real information wich allows 1024 different markers.

<img src="https://raw.githubusercontent.com/jchillerup/rokoko/master/aruco/rokoko/2a0board/2a0board.png" title="Markers table">


#### ArUco classes

The ArUco library contents are divided in two main directories. The src directory, which contains the library itself. And the utils directory which contains the applications.

The library main classes are:

   - **aruco::Marker**: which represent a marker detected in the image;
   - **aruco::MarkerDetector**: that is in charge of deteting the markers in a image Detection is done by simple calling the member funcion ArMarkerDetector::detect(). Additionally, the classes contain members to create the required matrices for rendering using OpenGL. See aruco_test_gl for details;
   - **aruco::BoardConfiguration**: A board is an array of markers in a known order. BoardConfiguracion is the class that defines a board by indicating the id of its markers. In addition, it has informacion about the distance between the markers so that extrinsica camera computations can be done;
   - **aruco::Board**: This class defines a board detected in a image. The board has the extrinsic camera parameters as public atributes. In addition, it has a method that allows obtain the matrix for getting its position in OpenGL;
   - **aruco::BoardDetector**: This is the class in charge of detecting a board in a image. You must pass to it the set of markers detected by ArMarkerDetector and the BoardConfiguracion of the board you want to detect. This class will do the rest for you, even calculating the camera extrinsics.

#### OpenCV
OpenCV (Open Source Computer Vision) is a library of programming functions for realtime computer vision. It uses a BSD license and hence it's free for both academic and commercial use. It has C++, C, Python and Java (Android) interfaces.

#### Camera calibration

Camera calibration is the process of obtaning the fundamental parameters of a camera. These parameter allows us to determine where a 3D point in the space projects in the camera sensor.
To calibrate in OpenCV we only need to show images of a chessboard panel of known dimensions.
It should take at least five different pictures. A frontal one, and four where the border of the pattern is near the image border. By placing the pattern near the image border it will be possible to estimate accurately the camera distorsion.

<img src="http://www.uco.es/investiga/grupos/ava/sites/default/files/images/calibrationimage.png" width="500" title="Calibration">

We also had to indicate the number of corners of the pattern in both axes, and the real size of the square.
As output, the program generates a .yml file that can be used in ArUco.


## Project Architecture

<img src="https://dl.dropboxusercontent.com/u/52788948/Architettura.png" title="Project architecture">

(TODO classes vs actor model)

### Marker Detector

This function is made with two file that could be found in the folder src/detector. 

####main.cpp

The functions provided by this node are:
- Loading the file containing the bottom camera calibration (camera.yaml);
- Defining marker size;
- Creating a detector (as specified in the file simple_detector.cpp);
- Subscribing to the camera topic;
- Calculating marker transform and publish it.

######Publish & Subscribe

This node subscribes to:
- camera topic -> topic where are published frames from the camera.

And publishes:
- Marker transform.

####simple_detector.cpp

This file mainly provide the detection function, using aruco library, the step are:
- Take the frame from the bottom camera;
- Detect a marker;
- Return the marker pose (position and orientation) and the the marker ID to the other components.

### Odometry Node

The code related to this node is contained in the file odometry_node.cpp that could be found in the folder src.
The functions provided by this node are:
- Reading and processing navdata:
    - Transforming roll, pitch and yaw data from radians to degree;
    - Calculating x and y velocity [mm/s];
    - Calculating altitude [mm];
    - Calculating orientation and position;
- Publishing odometry using publish_odometry() function;
- Handles command "zero" resetting the position, this command is used when the quality of the odometry starts degrading.

######Publish & Subscribe

This node subscribes to:
- twist_topic -> topic where the twist for the drone is published;
- navdata_topic -> topic where navdatas are published;
- command_topic -> topic where commands are published.

And publishes on:
- odometry_topic -> topic where the odometry is published;
- cmd_vel_topic -> topic where velocity are published;
- takeoff_topic -> topic where takeoff command is published;
- land_topic -> topic where land command is published.

### Localization Node

The code related to this node is contained in the file localization_node.cpp that could be found in the folder src.

The functions provided by this node are:
- Marker map:
    - List of the markers with relative position on the map:
        - Marker 428(0, 0);
        - Marker 341(0, 0);
        - Marker 985(2, 0);
- Reading odometry:
    - The odometry published by the Odometry Node is read and transformed in the_tf_odom_to_drone;
- Odometry correction:
    - If one of the known markers is found the odometry is corrected with the known position provided by the map;
- Estimating position:
    - Estimated position is calculated by multiplying the_tf_odom with the_tf_odom_to_drone;
- Publishing estimate position.

######Publish & Subscribe

This node subscribes to:
- odometry_topic -> topic where the odometry is published;
- markers_topic -> topic where markers are published.

And publishes on:
- estimate_topic -> topic where the estimate position is published.


### Path Planner Node

The code related to this node is contained in the file  path_planner_node.cpp that could be found in the folder src.

The functions provided by this node are:
- List of pose -> list of positions in the space;
- Motion profile:
    -   We used a sin function to implement the motion profile;
    -   It's used to avoid abrupt movements of drone;
-   Publishing set-point:
    - Everytime is published the set-point for the drone so that it could follow the motion profile;

######Publish & Subscribe

This node subscribes to:
- command_topic -> topic where the commands are published.

And publishes on:
- set_point_topic -> topic where the set_point is published.


### Control Node

The code related to this node is contained in the file controller_node.cpp that could be found in the folder src.

The functions provided by this node are:
- Controlling:
    - Read the estimate position published by the localization node;
    - Read the set_point published by the path planner;
    - Compute the twist control;
    - Estimate the velocity as the ratio between space and time:
        - Due to the variability of the result, the speed is filtered with a moving average filter;
    - Finally the control is made with a PD conrol;
- Command:
    - The commands processed by the node are:
        - Takeoff -> the control is disabled and an empty message is published on the takeoff_topic;
        - Land -> hover is done and an empty message is published on the land_topic;
        - Hover -> hover is done;
        - Toggle_ctrl -> if not already active the control is activated;
- Hovering:
    - This function is used to ensure a stable position for the drone and to have a vertical land.

####Publish & Subscribe
This node subscribe:
- estimate_topic -> topic where are published messages with the estimated position;
- set_point_topic -> topic where are published messages with the set point for the drone;
- command_topic -> topic where are published messages with the command sent to the drone.

And publish on:
- twist_topic -> topic where are published messages with the twist;
- land_topic -> topic where are published landing messages for thedrone;
- takeoff_topic -> topic where are published takeoff messages for the drone.

### Teleop Node

The code related to this node is contained in the file teleop_node.cpp that could be found in the folder src.

The functions provided by this node are:
- Commanding manually the drone (see image for the details of the commands);
- Coordinating odometry, control and path_planner nodes.

In this node is used the keyboard node provided by ROS.

#### Command list
<img src="https://dl.dropboxusercontent.com/u/52788948/PicsArt_1433772654507.jpg" title="Keyboard">

- **0-9** -> pose in the space;
- **q** -> quit:
    - Used in case of emergency;
    - The drone lands;
    - Deactivate the control node;
- **l** -> land;
- **t** -> take-off;
- **z** -> zero (as specified in Odometry node section);
- **Arrows** -> moving the drone over X and Y axes;
- **w-s** -> moving the drone over Z axe (adjust the altitude);
- **a-d** -> yaw.

####Publish & Subscribe

This node publish on:
- cmd_vel_topic -> topic where are published messages with the command velocity;
- command_topic -> topic where are published messages with the commands for the drone.



## Conclusioni

The project works very well in fact the drone reaches properly all positions commanded. However during the development we have encountered many problems, the biggest are:
- Wi-Fi connection:
    - The Wi-Fi connection between the PC with ROS and the drone is very unstable overall for the interference with other Wi-Fi networks;
    - Obviously we could solve this problem using the drone in an interference-free place.
- Odometry:
    - As we know the data from the odometry still good only for small periods of time;
    - To obviate this problem we introduce the zero function (as described in the odometry node section).
