# AWS Sample Applications for the Waveshare JetBot
![Waveshare JetBot](/images/jetbot-aws.jpg)

A collection of ROS sample applications for the Waveshare JetBot to run in AWS RoboMaker. This repository includes the following sample applications:

- **Line Following:** This simple navigation application parses frames from the camera feed using OpenCV and instructs the Jetbot to follow a line.
- **SLAM and GMapping:** This sample application will run move_base, gmapping and SLAM - enabling you to generate maps using AWS provided open source 3D worlds.
- **Simple Motion:** This sample application performs basic rotation and forward motion commands.

## Setup

To run these sample applications, you can use Cloud9 Enviroment. For this we can use a cloudformation template that provides the infrastructure needed or follow https://aws.amazon.com/blogs/robotics/robotics-development-in-aws-cloud9/ Blog and use ROS NOETIC.

For the steps using cloudformation

1. Have your account sign in and enter to cloudformation.
2. Download the following file https://djzvpds7r9hcp.cloudfront.net/c9-ros-nice-cfn.yaml
3. Create New Stack, choose upload from a template file and choose the previous downloaded file, Click Next
4. Choose a Stack Name, write the Cloud9AccessRoleName **DO NOT LEAVE AS DEAFULT**, Choose ROS Version ROS1NOETIC, and leave simulator as Gazebo
5. Click Next and leave everythin as default,
6. Click Next and accept that you acknowledge that AWS CloudFormation might create IAM resources.
7. Wait for the stack to deploy 
8. When ready go into Cloud9 and open Cloud9 IDE 
9. In the terminal of the IDE (bottom pane), run the following commands:

    ```bash
        cd ~/environment
        git clone https://github.com/cataherrera/Cloud4kids-Robo4kids.git robo4kids
        cd robo4kids
        chmod +x ./setup.sh
        ./setup.sh
    ```

## Build (and modify) the sample applications using the AWS RoboMaker IDE.  

The sample applications in this repository can be run on a Gazebo simulation.

1. Open a **Virtual Desktop** by clicking the virtual desktop button in the top navigation tool bar. This will open a pop-up window, if you are prompted, allow your browser to open the pop-up window.
2. Move the new window adjacent to your IDE browser window. In the terminal of the IDE, set the display to the virtual desktop:

    ```bash
    export DISPLAY=:0
    ```

3. Next, we will build the ROS application to run in simulation. **Note: every time you make a code change to the ROS packages in this sample repository, you will need to run this command to re-build the application.**

    ```bash
    cd ~/environment/robo4kids
    colcon build
    ```

## Running the sample applications
### Robo4Kids - Control your robot with blockly

Now that the ROS application is built, we can source the application and run the simulation. To run the sample applications, simply source the built workspace then invoke the **roslaunch** file.

To start, ensure you are in the base workspace directory and the application has been sourced.

```bash
cd ~/environment/robo4kids
source install/setup.sh
```

You will be able to control de jetbot movement through some blockly code in your computer.

#### 1. In order to perform some operations for this workshop, it is necessary to assign an IAM machine role to your Cloud9 environment.

* Create a IAM role:
    * Select AWS Service - EC2 use case
    * Attache following policies:
        * AWSRoboMaker_FullAccess
        * AWSIoTFullAccess
        * IAMFullAccess
        * AmazonCognitoPowerUser
    * leave everything else as deffault and create role
* Attach role to the EC2 intance
    * go to the EC2 intance and selec the isntance with your robomaker enveriment name
    * Click Actions > Security > Modify IAM role.
    * Choose the role you recently created

* Disable Cloud9's normal management of your credential
    * On the Robomaker enviroment click the settings gear icon in the top-right corner of your Cloud9 environment.
    * Scroll down and open the section for AWS Settings. Disable AWS managed temporary credentials. Close the preferences pane.


#### 2. Configure AWS region


Run the following command in your Robomaker enviroment, change the `<Region you are Using>` for the region you are deploying this sample

 ```bash
    aws configure set default.region $<Region you are Using>
```

#### 3. Install and create dependencies
Run the following command in your Robomaker enviroment, this script will create the necesary requirments to connect through IoT to the jebtot as well as a cognito identity pool.

 ```bash
    cd ~/environment/robo4kidss/src/aws_example_apps/robo4kids/assets/scripts
    chmod +x  install_deps.sh
    source install_deps.sh

```
#### 4. Next, we will re-build the ROS application to run in simulation.

```bash
    cd ~/environment/robo4kids
    colcon build
```   
To start, ensure you are in the base workspace directory and the application has been sourced.
```bash
    cd ~/environment/robo4kids
    source install/setup.sh
```   

#### 5. Launch the aplication

```bash
roslaunch jetbot_sim_app teleop.launch
```

#### 6. Run the Blockly Interface

* Download the src/aws_example_apps/robo4kids/robo4kids_webapp on to your ***own computer*** folder.

* the aws-iot.js and aws-exports.js robo4kids_webapp/src directory are the files that store the aws credentials

* Run the web aplication, on the terminal in your computer inside the roboblockly directory run the following commands
```bash
    yarn install
    yarn start
```

A window in your browser should open with the roboblockly interface, select the empty level and start moving your robot!
Open both widows side by side. The Jetbot simulation and the blockly interface.
Drag and drop the blocks and conected to each other to control your robot. Star by selecting the foward and move left block like in the picture, press Run Code and see what happends with the jebot simulation!

![Blockly JetBot](/images/jetbot-blockly.png)



TroubleShooting
If the Blockly interface is not conecting with the simulation, check if the app is able to connect to the iot endpoint. One posible place to check is if the cognito identity pool, has  the  unauthrol-robo4kids attached to the Unauthenticated Rol. It should be atacched but if a problem happend when running the install_deps.sh it could be you are using a pool that doesnt have the role attached.


## Deploy and run with an NVidia Jetbot Kit

Coming Soon!
