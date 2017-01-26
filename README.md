# CURC Autonomous Drone Project #1 Workspace

Installation options:

Drone team Google Drive: https://drive.google.com/open?id=0BxN1-ObwqraBMHNTQ29nd285MEU

#### VMWare
Download the CURC-VMWare.zip folder from the Drone team Google Drive, extract it, File > Open CURC-VMWare.ovf in VMWare, and run the VM. Feel free to adjust VM settings (RAM, storage, etc.) as appropriate for your personal computer. You may need to relax OVF settings for the import to work. 

#### VirtualBox (Recommended)
Download the CURC.vdi file from the Drone team Google Drive, File > Open the .vdi in VirtualBox, and run the VM. Feel free to adjust VM settings as appropriate for your personal computer.
Full instructions are available in the README document where the virtual machine images can be found.

#### NOTE
A Linux system is NOT required to develop, just to simulate your applications. We also have a VirtualBox instance of the image running in the MakerSpace, so you can write in your favorite text editor, clone your git repo to the clean image, and then test in the simulator running on the iMac there.

### OR

The following installation process is for those who have dual-booted Linux, want to install the tum_ardrone packages onto their CLIC lab accounts, or who want to keep their existing VM setup.

Follow the instructions below in your own VM. Copying and pasting the commands into your terminal is fine; there will be times where your terminal prompts you to press Enter, enter a password, etc.

Open up a Terminal window. If there is an existing ```drone_ws``` directory in your home directory, delete it using ```rm -rf ~/drone_ws```

Workspace setup:
```
cd ~
mkdir drone_ws
cd drone_ws
mkdir src
cd src
catkin_init_workspace
cd ~/drone_ws
catkin_make
echo "source ~/drone_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

 * [ardrone_autonomy] (https://github.com/AutonomyLab/ardrone_autonomy)
  - Docs: http://ardrone-autonomy.readthedocs.io/en/latest/installation.html
 ```
 cd ~/drone_ws/src
 git clone https://github.com/AutonomyLab/ardrone_autonomy.git -b indigo-devel
 cd ~/drone_ws
 sudo rosdep install --from-paths src --rosdistro indigo 
 catkin_make
 ```
 * [tum_ardrone] (https://github.com/tum-vision/tum_ardrone)
 
 ```
 sudo apt-get install freeglut3 freeglut3-dev
 cd ~/drone_ws/src
 git clone https://github.com/tum-vision/tum_ardrone.git -b indigo-devel
 cd ~/drone_ws
 sudo rosdep install --from-paths src --rosdistro indigo 
 catkin_make
 ```
  - Note: there will be a period of time where it seems like the compilation is hanging on ```Makefile:406: .deps: No such file or directory```. Not to worry--tum_ardrone is still compiling; give it five minutes or so!
 * [tum_simulator] (https://github.cohttps://github.com/dougvk/tum_simulator)
 
 ```
 cd ~/drone_ws/src
 git clone https://github.com/occomco/tum_simulator.git
 cd ..
 rosdep install --from-paths src --ignore-src --rosdistro indigo -y
 catkin_make
 ```
 
PLEASE read http://wiki.ros.org/tum_ardrone/, as well as the documentation for the nodes in ```tum_ardrone``` to understand their inputs and outputs (see "Used Topics"), and the role they play within the larger drone software architecture:

Nodes to read about:
 * [drone_stateestimation] (http://wiki.ros.org/tum_ardrone/drone_stateestimation)
 * [drone_autopilot] (http://wiki.ros.org/tum_ardrone/drone_autopilot)
 * [drone_gui] (http://wiki.ros.org/tum_ardrone/drone_gui)

### Run (in separate terminals)

If running simulation:

Terminal 1:
```
roscore
```
Terminal 2:
```
roslaunch cvg_sim_gazebo ardrone_testworld.launch
```
Terminal 3:
```
roslaunch tum_ardrone tum_ardrone.launch
```

If running actual drone:

Terminal 1:
```
roscore
```
Terminal 2:
```
roslaunch tum_ardrone ardrone_driver.launch
```
Terminal 3:
```
roslaunch tum_ardrone tum_ardrone.launch
```

Key assignments for keyboard control: 
http://wiki.ros.org/tum_ardrone/drone_gui#Keyboard_Control

> KB control requires the GUI window to have focus, but NOT the upper-left text field (press ECS)

    q,a: fly up & down.
    i,j,k,l: fly horizontally.
    u,o: rotate yaw.
    F1: toggle emergency
    s: takeoff
    d: land 

> Focus the GUI window and press ESC to immediately enable KB control and set focus such that KB control works properly. This can be used for safety (autopilot does wired stuff -> press ESC and immediately take over, disabling the autopilot and enabling manual control).
