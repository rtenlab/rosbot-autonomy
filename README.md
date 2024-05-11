# rosbot-autonomy

Based on Husarion's rosbot-autonomy docker images and instructions (https://github.com/husarion/rosbot-autonomy). This forked repo is to run them in simulation on PC and AGX.


## 🤖 Run all (autonomy, visualization, simulation) in Host PC

> [!IMPORTANT]
> To run `Gazebo` or `Webots` Simulators you have to use computer with NVIDIA GPU and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) installed.

![Gazebo](https://github-readme-figures.s3.eu-central-1.amazonaws.com/rosbot/rosbot-autonomy/gazebo-rviz.png)

### Gazebo

To start Gazebo simulator run:

```bash
just start-gazebo-sim
```

### Webots

To start Webots simulator run:

```bash
just start-webots-sim
```

### 🚗 Control the ROSbot from RViz

To instruct the robot to autonomously explore new areas and create a map (in "slam" mode) of **[2D Goal Pose]** in RViz. When `SLAM` is off, you can indicate the robot's current position by **[2D Pose Estimate]** button.

![autonomy-result](https://github-readme-figures.s3.eu-central-1.amazonaws.com/rosbot/rosbot-autonomy/autonomy-result-rviz.gif)

------


## 🤖 Run autonomy in AGX and visualization & simulation in Host PC
Make sure the AGX and the host PC are connected to the same network and the devices are pingable. Check dmesg and use ufw to ensure the devices can communicate with one another. 

On the Simulation host PC: 

```bash
docker compose -f compose.sim.host.gazebo.yaml up
```

Then, on the client device (AGX), 
```bash
docker compose -f compose.sim.agx.yaml up
```

Then, once that is running, run rviz on the host: 

```bash
docker compose -f compose.sim.host.rviz.yaml up
```

### Troubleshooting 
Note: if AGX keeps printing tf error, check if ros2 multicast works.
```bash
ros2 multicast receive
```
Open another terminal:
```bash
docker ps
docker exec -it <container_id> bash
ros2 multicast send
```
If no message is delivered, try running the docker image in host network mode:
```bash
docker compose -f compose.sim.agx_netmode-host.yaml up
```
------


## 🔧 Verifying User Configuration

To ensure proper user configuration, review the content of the `.env` file and select the appropriate configuration (the default options should be suitable).

- **`LIDAR_BAUDRATE`** - depend on mounted LiDAR,
- **`MECANUM`** - wheel type,
- **`SLAM`** - choose between mapping and localization modes,
- **`SAVE_MAP_PERIOD`** - period of time for autosave map (set `0` to disable),
- **`CONTROLLER`** - choose the navigation controller type,
- **`ROBOT_NAMESPACE`** - type your ROSbot device name the same as in Husarnet.

> [!IMPORTANT]
> The value of the `ROBOT_NAMESPACE` parameter in the `.env` file should be the same as the name of the Husarnet device.


