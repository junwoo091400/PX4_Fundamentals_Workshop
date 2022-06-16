# "PX4 Fundamentals" Workshop Materials

This is the material that will be used during the [PX4 Developer Summit 2022](https://events.linuxfoundation.org/px4-developer-summit/).

This repository contains everything that the participant will need to follow & try out the demos presented during the workshop!

-  Friday June 24, 2022 10:50am - 12:30pm CDT 
-  Lone Star A, JW Mariott Austin Texas

## About the Workshop

 Have you struggled with getting started in PX4 Development? Did you ever get stuck after running the example in the user doc and couldn't develop what you wanted? This workshop explains all the essential concepts and knowledge you need to get started with PX4 Autopilot Development!

In this workshop, you will learn the core concepts in PX4 like NuttX, Modules, Drivers, uORB, and Parameters. We will show you how to set up a development environment using Docker and program a small custom module, compile it, and test using SITL (Software-In-The-Loop Simulation) to verify its behavior and intuitively understand how a module works.

We will also construct a more advanced module that will require more advanced concepts like Work Queue, Scheduling, MAVLink, and more. With this process, you will gain tangible insight into constructing a proper module that does more than one task. Finally, we will demonstrate uploading the custom modules into actual Hardware.

We will also share tips and tricks that will make your PX4 development experience more pleasant, and we will stick around to answer as many questions as possible in the time we have. 

> [Link to the Event information](https://sched.co/12d8c)

## What is PX4?


## 1. High Level Overview of PX4 Architecture

[Official PX4 Architecture Documentation](https://docs.px4.io/master/en/concept/px4_systems_architecture.html).

### NuttX OS

You can learn briefly about the concept of an **Operating System** [here](https://edu.gcfglobal.org/en/computerbasics/understanding-operating-systems/1/).
Or if you feel like it, [here's](https://www.geeksforgeeks.org/introduction-of-operating-system-set-1/) a more in-depth explanation on Operating System.


PX4-Autopilot holds the NuttX related files the `platform/nuttx` [folder](https://github.com/PX4/PX4-Autopilot/tree/master/platforms/nuttx)

In the `nuttx/src` folder, you can find commonly used OS-level px4 commands like: [`px4_task_spawn_cmd`](https://github.com/PX4/PX4-Autopilot/blob/master/platforms/nuttx/src/px4/common/tasks.cpp#L58-L87)

There are 2 submodules inside the `nuttx/nuttX` [folder]:

The `nuttx/NuttX/apps` submodule handles:
- Shells (MAVLink Shell utilizes this)

The `nuttx/NuttX/nuttx` submodule handles:
- Task Scheduling
- IO resource management
- Networking
- File System
- Hardware level abstraction (Drivers)

### Modules

All the PX4-Autopilot modules are located in the `src/modules` [folder](https://github.com/PX4/PX4-Autopilot/tree/master/src/modules).

[Official PX4 Modules Documentation](https://docs.px4.io/master/en/modules/modules_main.html)


### Drivers

### uORB
- Blog Post on PX4 uORB Concept: [Part 1](https://px4.io/px4-uorb-explained-part-1/), [Part 2](https://px4.io/px4-uorb-explained-part-2/), [Part 3](https://px4.io/px4-uorb-explained-part-3-the-deep-stuff/) and [Part 4 (Coming Soon)]!

![uORB Graph](Assets/uORB_Graph.gif)

You can check out the colorful and interactive uORB graph [here](https://docs.px4.io/master/en/middleware/uorb_graph.html)!


### Parameters
- Blog Post on PX4 Parameters Concept: [Part 1](https://px4.io/px4-parameters-part-1-overview/) and [Part 2](https://px4.io/px4-parameters-part-2-in-depth-guide/)


## Development Environment setup

> We will be following [PX4 Docker Documentation](https://docs.px4.io/master/en/test_and_ci/docker.html)

### First, install Docker

### Second, clone the PX4-Autopilot repository
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
cd PX4-Autopilot
```

### Third, execute an SITL through Docker

You first need to create a running Docker container

```bash
docker run -it --privileged \
--env=LOCAL_USER_ID="$(id -u)" \
-v $(pwd):/src/PX4-Autopilot/:rw \
-v /tmp/.X11-unix:/tmp/.X11-unix:ro \
-e DISPLAY=:0 \
px4io/px4-dev-ros-melodic:latest bash
```

Then navigate into the directory `src/PX4-Autopilot` and build the SITL

```
cd src/PX4-Autopilot
make px4_sitl_default gazebo HEADLESS=1
```

### Fifth, check the SITL by opening a QGC

- All the PX4IO Docker [Images](https://hub.docker.com/u/px4io/)
- 

- [Ubuntu PX4 Build Toolchain setup Documentation](https://docs.px4.io/master/en/dev_setup/dev_env_linux_ubuntu.html#gazebo-jmavsim-and-nuttx-pixhawk-targets)
- [PX4 Docker container usage Documentation](https://docs.px4.io/master/en/test_and_ci/docker.html#use-the-docker-container)

