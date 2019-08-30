---
title: Route Admin Panel
id: route-admin-panel
---


## About

The **Route admin panel** is a web user interface for managing routes of ROS based mobile robots.

It allows to:
- Define destination points
- Save robot position as destination point
- Send destination point to `move_base`
- Upload custom map
- Set a sequence of destination points

The **Route admin panel** is built as a [Node.js](https://nodejs.org/) application. On one side it is interfacing with ROS topics, while on another side it presents a frontend for managing robot destinations.

## Installation

Install [Node.js](https://nodejs.org/):

```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt install -y nodejs
```

Clone and build Husarion fork of `rosnodejs` repository:

```bash
mkdir ~/husarion_rosnodejs
cd ~/husarion_rosnodejs
git clone https://github.com/RethinkRobotics-opensource/rosnodejs.git
cd ~/husarion_rosnodejs/rosnodejs
npm install
npm pack
```

Create workspace and clone dependency repositories, it may happen that you already have it done, in that case, skip this step:

```bash
mkdir ~/ros_workspace
mkdir ~/ros_workspace/src
cd ~/ros_workspace/src
catkin_init_workspace 
echo '. ~/ros_workspace/devel/setup.sh' >> ~/.bashrc

git clone https://github.com/husarion/husarion_ros.git
git clone https://github.com/husarion/rosbot_description.git
```

Clone `route_admin_panel` repository:

```bash
cd ~/ros_workspace/src
git clone https://github.com/husarion/route_admin_panel.git
```

Install rosnodejs and dependencies:

```bash 
cp ~/husarion_rosnodejs/rosnodejs/rosnodejs-3.0.0.tgz ~/ros_workspace/src/route_admin_panel/nodejs
cd ~/ros_workspace/src/route_admin_panel/nodejs/
npm install rosnodejs-3.0.0.tgz
npm install express socket.io quaternion-to-euler math3d multer
npm install
mkdir user_maps
echo '{"targetList": {"targets": []}}' > user_maps/config.json
```

Build workspace:

```bash
cd ~/ros_workspace
catkin_make
. ~/ros_workspace/devel/setup.sh
```

## How to use

Panel comes with prepared launch files for `move_base`, `gmapping`, `node.js` server and all other required components.
Depending on your ROSbot version, you can start it with:

- for ROSbot 2.0:

    ```bash
    roslaunch route_admin_panel demo_rosbot.launch
    ```

- for ROSbot 2.0 PRO:

    ```bash
    roslaunch route_admin_panel demo_rosbot_pro.launch
    ```
- for Gazebo simulator:

    ```bash
    roslaunch route_admin_panel demo_gazebo.launch
    ```

- for ROSbot 2.0 with [Mbed firmware](https://github.com/husarion/rosbot-firmware-new):

    ```bash
    roslaunch route_admin_panel demo_rosbot_mbed_fw.launch
    ```

- for ROSbot 2.0 PRO with [Mbed firmware](https://github.com/husarion/rosbot-firmware-new):

    ```bash
    roslaunch route_admin_panel demo_rosbot_pro_mbed_fw.launch
    ```

Once all nodes are running, go to web browser and type in address bar:

```bash
ROSBOT_IP_ADDRESS:8000
```
You need to substitute phrase `ROSBOT_IP_ADDRESS` with IP address of your device.

You should see interface like below:

![RouteAdminPanelScreenshot](/docs/assets/img/software/route-admin-panel.png)

## Using panel from any network

In case you would like to manage robot destinations outside of local network, you could use [Husarnet](https://husarnet.com/) for secure connection with your robot.

All Husarion devices comes with Husarnet preisntalled, if you are using your own device, install Husarnet according to [installation guide](https://docs.husarnet.com/install/).

If you do not have a Husarnet account, create it and log in to [Husarnet dashboard](https://app.husarnet.com/).

In Husarnet dashboard, click **Create network** button, you will get a dialog:

![create network](/docs/assets/img/software/husarnet_01_create-network.png)

Type `route_admin_demo` as network name then click **Create** button.

Go to your device and register it in Husarnet network by executing in terminal:

```
sudo husarnet websetup
```

You will get a registration link as a response, open it in web browser:

![add device](/docs/assets/img/software/husarnet_02_husarnet.png)

- In **Name for this device** provide `my-rosbot`
- In **Add to network** dropdown menu choose `route_admin_demo`
- Check **Change device hostname** checkbox
- Click **Add device to your account** button

You will be redirected to network summary view:

![network summary](/docs/assets/img/software/husarnet_03_network.png)

Click device name to open its configuration:

![network member](/docs/assets/img/software/husarnet_04_network_member.png)

Check **ROS master** checkbox.

Optionally you can also check **Make the Web UI public** if you want to make panel accessible for anyone knowing device address.

Go back to your device and start panel with the same launch file as for local network.

Once the panel is running, you will notice new button **WebUI** next to your device address in Husarnet dashboard, use this button to view panel.

![web ui accessible](/docs/assets/img/software/husarnet_05_network.png)

***Wait! But what about real peer-to-peer connection?***

To get access without need to log into any server, you will have to install Husarnet client also on your laptop, procedure is the same as for any other device.

Then register your laptop in Husarnet network the same way as you did with robot.

On laptop open browser and in address bar type: `[ROSBOT_HUSARNET_ADDRESS]:8000`
`ROSBOT_HUSARNET_ADDRESS` ia a value that you can find in Husarnet dashboard in device settings.

In the end you will be able to access `route_admin_panel` from any network using a secure peer-to-peer connection:

![panel accessed through husarnet](/docs/assets/img/software/panel_at_husarnet.png)