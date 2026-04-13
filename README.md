# ASCamera ROS2 SDK — Setup Guide
### RA305 Robotics Programming

---

## Prerequisites

- ROS 2 Jazzy installed and sourced
- Ubuntu 24.04 on Raspberry Pi 5
- ASCamera depth camera and USB cable

---

## 1. Clone the Repository

Clone into your project directory (you know which one):

```bash
git clone https://github.com/KSU-Chad/ascamera.git
cd ascamera
```

---

## 2. Install Dependencies

Update apt first:

```bash
sudo apt update
```

Install required libraries:

```bash
sudo apt install libgflags-dev nlohmann-json3-dev libgoogle-glog-dev \
ros-$ROS_DISTRO-image-transport ros-$ROS_DISTRO-image-publisher ros-$ROS_DISTRO-camera-info-manager
```

---

## 3. Configure udev Rules

Navigate to the scripts directory:

```bash
cd ascamera/scripts/
```

Copy the udev rule file (grants the camera proper device permissions):

```bash
sudo cp angstrong-camera.rules /etc/udev/rules.d
```

Run the target detection script:

```bash
sudo ./gettarget.sh
```

Reload udev rules to apply changes:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

---

## 4. Configure the Launch File

Open the launch file to update the configuration path:

```bash
gedit ../launch/ascamera.launch.py
```

Find the configuration file path entry and update it to match your directory structure, replace <user> and <workspace>:

```
/home/<user>/<workspace>/ascamera/configurationfiles
```

Save and close.

---

## 5. Build the Workspace

Navigate back to the project root and build:

```bash
cd ..
./build.sh
```

Source the environment:

```bash
source install/setup.bash
```

---

## 6. Run the Camera Node

```bash
./run_ascamera_node.sh
```

Leave this terminal open — the camera service must stay running while viewing.

---

## 7. Visualize Camera Output

Open a **new terminal** for the viewer. Two options:

### Option A — rqt (2D images)

```bash
rqt
```

In the rqt window: **Plugins → Visualization → Image View**

- Depth map: select the depth topic,
 - change the Max depth value and see how the image changes

- RGB image: select the rgb0 topic from the dropdown

### Option B — RViz (3D / depth)

```bash
rviz2
```

**For color image:**
1. Click **Add → By topic → /ascamera → /camera_publisher → /rgb0 → /imgage → imgage → OK**

**For depth map:**
1. Set **Fixed Frame** to `camera_link`
2. Click **Add → By topic → /ascamera → /camera_publisher → /depth0 → /points → PointCloud2 → OK**

---

