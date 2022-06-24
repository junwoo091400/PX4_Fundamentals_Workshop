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

## Prerequisites
In order to follow the Workshop without delay, you must have the following prerequisites satisfied:
Click the links on the right of each condition that you don't satisfy

1. Have Git installed: https://git-scm.com/downloads
2. Have Docker installed: https://docs.docker.com/get-docker/
3. Have the Docker image for PX4 development ready
   - Do: `docker pull px4io/px4-dev-ros-melodic:latest`
4. Have QGC installed: https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html
5. 

> Workshop content start here!
---

## What is PX4?

## High Level Overview of PX4 Architecture

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


---

## Development Environment setup

> We will be following [PX4 Docker Documentation](https://docs.px4.io/master/en/test_and_ci/docker.html)

### First, install Docker

Follow the instructions here: https://docs.docker.com/get-docker/

### Second, clone the PX4-Autopilot repository
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
cd PX4-Autopilot
```

- It is important that you `cd` into the PX4-Autopilot directory as we will map the current directory where Command prompt is at as the PX4-Autopilot directory in the Docker container!

### Third, get the Docker Image 'px4io/px4-dev-ros-melodic:latest' 

Here, you can either pull it from the docker hub or load it from the USB file that is supplied during the workshop.

How to pull from internet: `docker pull px4io/px4-dev-ros-melodic:latest`

How to load from a USB drive tar file: `docker load -i .\px4-dev-ros-melodic_latest.tar`.
Note, the path you supply should be where the Docker Image TAR file is located in your device.


### Fourth, run the Docker Container

You first need to create a running Docker container

```bash
docker run -it --privileged \
--env=LOCAL_USER_ID="$(id -u)" \
-v "$(pwd):/src/PX4-Autopilot/:rw" \
-v /tmp/.X11-unix:/tmp/.X11-unix:ro \
-e DISPLAY=:0 \
px4io/px4-dev-ros-melodic:latest bash

# Or, if you prefer a continuous one-line command:
docker run -it --privileged -v "$(pwd):/src/PX4-Autopilot/:rw" -v /tmp/.X11-unix:/tmp/.X11-unix:ro -e DISPLAY=:0 px4io/px4-dev-ros-melodic:latest bash
```

- If you don't have the "" around the volume mapping of the PX4-Autopilot directory, it may include a space character, which can cause error messages such as "invalid reference format: repository name must be lowercase".
- Check out the [Stack Overflow answer](https://stackoverflow.com/questions/48522615/docker-error-invalid-reference-format-repository-name-must-be-lowercase).

### Fifth, build a sample target
Then navigate into the directory `src/PX4-Autopilot` and build the px4 target `px4_sitl` as an example
```bash
cd /src/PX4-Autopilot
make px4_sitl_default
```

This should produce a file `build/px4_sitl_default/bin/px4`.
Now you have your first target built!

### Docker resources
- All the PX4IO Docker [Images](https://hub.docker.com/u/px4io/)
- [Ubuntu PX4 Build Toolchain setup Documentation](https://docs.px4.io/master/en/dev_setup/dev_env_linux_ubuntu.html#gazebo-jmavsim-and-nuttx-pixhawk-targets)
- [PX4 Docker container usage Documentation](https://docs.px4.io/master/en/test_and_ci/docker.html#use-the-docker-container)

---

## Software In The Loop (SITL) in Docker

```bash
cd /src/PX4-Autopilot
make px4_sitl_default gazebo HEADLESS=1
make px4_sitl_default jmavsim HEADLESS=1 # In case Gazebo is taking up too much resource you can try this!
```

- If the build process in the Docker is taking too much resource in Windows, checkout [this article](https://dev.to/tallesl/vmmen-process-consuming-too-much-memory-docker-desktop-273p) for a fix (for limiting the RAM usage of WSL process)

### Then connect QGC with the SITL

Running the docker container should automatically allow the QGC to connect!

### Try taking off the drone

You can either do one of the following:
1. In QGC, select the 'Takeoff' button on the left panel in the main map screen, and slide to confirm takeoff command
2. In PX4 terminal, type `commander takeoff`

This should have your drone flying in the air! And you can see how the simulation actually works :)

---

## Simple Module Development: MAD-MAX

> Let's develop a simple module to get an understanding of how to create one of the most fundamental blocks of PX4!

[This](https://github.com/junwoo091400/PX4-Autopilot/tree/devsummit2022_px4_fundamentals_workshop_simple_module) is where the custom branch for the simple module is located!

### Fetching the custom branch from a repository

First, add the remote repository
```bash
git remote add junwoo https://github.com/junwoo091400/PX4-Autopilot.git
git remote add junwoo git@github.com:junwoo091400/PX4-Autopilot.git # In case you are using a SSH credentials for github interface
```

Second, fetch the branch `devsummit2022_px4_fundamentals_workshop_simple_module` from the repository
```bash
git fetch junwoo devsummit2022_px4_fundamentals_workshop_simple_module
git checkout junwoo/devsummit2022_px4_fundamentals_workshop_simple_module # Checkout the branch, but you will be in a detached HEAD state!
git switch -c devsummit2022_px4_fundamentals_workshop_simple_module # Create a new local branch off of the remote branch
```

### Updating the submodules (may or may not be needed)
PX4 uses a lot of different submodules and it may become outdated once you switch to another branch and so on. Therefore it is important to update the submodules when you switch the branch!
```bash
git submodule update --init --recursive # `--init` is for initializing a submodule in case it doesn't exist at all (wasn't fetched)
```

### Build the SITL
```bash
make px4_sitl_default jmavsim HEADLESS=1 # Lightweight simulator!
```

### Uploading the plan

Upload the [sample mission plan file](Assets/mad_max_sample.plan) via QGC.

---

## Hardware Demo
> Does all of this run in Hardware? If so, how do we do that?

I will be using [Pixhawk 4](https://docs.px4.io/master/en/flight_controller/pixhawk4.html), but you can use any hardware you want that is supported by PX4! Here is [the list of all of them](https://docs.px4.io/master/en/flight_controller/).

### Figure out which target name you need to type from the Docs

For Pixhawk 4, `px4_fmu-v5` is the target name!

### Uploading the program to the Hardware

Uploading can be done in 2 different ways:

#### 1. Uploading with the native upload command

You can refer to the Docs [here](https://docs.px4.io/master/en/dev_setup/building_px4.html#uploading-firmware-flashing-the-board), but basically you need to append ` upload` at the end of the make command of building the target.

`make px4_fmu-v5_default upload`

> This method is the most convenient one, but it requires the Docker container to be able to connect to the board.

#### 2. Uploading using QGC

Select 'Custom' Firmware selection in the Firmware update tab, and upload! :P

### Select the Simulation-In-Hardware Airframe

'SIH Quadcopter X' from the Airframes Tab, to run simple simulation inside the hardware!

### Check the mad_max module's output via MAVLink Shell

You can have the output through QGC's MAVLink Shell (Anaylize Tools tab > MAVLink Shell > Breakout panel)