# Actuated 3-Fingered Robotic Claw

**Project Lead:** Jensen George  
**Organization:** Open Droids  
**Role:** Mechanical Engineering Intern  
**Project Type:** Robotics Development Project

Led the development of a **3-finger dexterous robotic gripper** at Open Droids, collaborating with two fellow engineering interns throughout system integration, controls development, prototyping, and testing.

I independently designed the **complete mechanical CAD assembly and every custom component within it**, including a custom **tri-axis rack-and-pinion actuation mechanism** developed to translate a single actuation input into coordinated motion across all three fingers.

The project combined mechanical design, electromechanical integration, embedded control, and ROS 2 communication to develop and validate a functional prototype. The system evolved through two control architectures: an initial **ROS 2 interface using RS485 serial communication** and a final **standalone IR remote-control implementation**.

<p align="center">
  <img src="Media/gripper_demo.gif" width="175" alt="3-Finger Dexterous Gripper Demonstration">
</p>

## My Contributions

- Served as **project lead** for the three-intern engineering team through design, integration, prototyping, and testing
- Independently designed the **entire mechanical CAD assembly**, including every custom-designed component used within the gripper mechanism
- Conceived and developed a custom **tri-axis rack-and-pinion mechanism** to coordinate actuation across the gripper's three fingers
- Iterated the mechanical architecture based on actuation requirements, packaging constraints, prototype behavior, and system testing
- Produced CAD models and engineering documentation to support fabrication, assembly, and design iteration
- Supported integration of the mechanical system with servo actuation and Arduino-based embedded control
- Contributed to integration of **ROS 2 control over RS485 serial communication**
- Performed prototype assembly, troubleshooting, testing, and validation alongside the engineering team

---

# Mechanical Design

## Tri-Axis Rack-and-Pinion Actuation

A central mechanical challenge was developing an actuation architecture capable of coordinating **three fingers arranged around the gripper** while maintaining a compact assembly.

To address this, I designed a custom **tri-axis rack-and-pinion mechanism** in which the gripper's three finger actuation paths are mechanically coordinated through the central drive architecture. The mechanism converts actuator input into coordinated motion across the three fingers while consolidating the transmission within the gripper assembly.

I developed the mechanism from concept through CAD implementation as part of the complete gripper assembly, including the individual custom components, interfaces, and supporting geometry required for integration.

### CAD Design Ownership

The complete gripper CAD assembly was designed independently by **Jensen George**. This included the overall assembly architecture as well as each custom mechanical component incorporated into the design.

Design development included:

- Overall gripper architecture and packaging
- Tri-axis rack-and-pinion transmission
- Finger mechanisms and associated geometry
- Mechanical interfaces between components
- Servo integration and transmission geometry
- Assembly constraints and component fit
- Iterative redesign based on prototype and testing results


1. **ROS 2 Serial Control (RS485)**:
An earlier version of the project enabled **ROS 2** control over **RS485 serial communication**. A custom ROS 2 Python node published open and close commands to the Arduino, allowing collaboration with larger robotic systems and automation frameworks

2. **IR Remote Control**:
The final design uses an **infrared** remote to open and close the gripper directly. The Arduino decodes the IR signals and drives the servo to open or close the gripper. This approach provides a simple, standalone solution without requiring a PC or ROS environment

Both control methods are documented to showcase the flexibility of the system and the development process from PC-based to fully embedded control

The steps for the method using ROS 2 Serial Control will be presented first with hardware, firmware, and software, then followed by the IR Remote Control approach

# ROS 2 SERIAL CONTROL GRIPPER (RS485)
---
This project enables ROS2 control of a 3-finger adaptive gripper using an Arduino microcontroller and RS485 serial communication. The goal is to design, build, and test working system to receive open and close commands from a ROS 2 node, parsing them on the Arduino, and driving a servo motor accordingly
# Overview
The system bridges a gap in robotics research by providing a gripper that:
- Uses standardized communication (RS485),
- Integrates cleanly with ROS 2-based workflows,
- And is built from affordable components
  
## Hardware Components: 
- Arduino Uno
- MAX485 Module
- USB-to-RS485 Adapter
- FT6335M Servo Motor 
- 5V 2A Power Supply w/ Barrel Jack

---
# How to Set Up

### 1. Wiring 
| Arduino Pin  | MAX485 Module |
| ------------- | ------------- |
| D11  | DI (TX)  |
| D10  | RO (RX)  |
| D2 | DE/RE (Soldered to one output)|
| GND  | GND  |
| 5V  | VCC  |

| MAX485 MODULE  | USB-TO-RS485 |
| ------------- | ------------- |
| A   | A  |
| B  | B  |
| GND (shared with Arduino GND ) | GND  |

| FeeTech 360 Core Motor  | Arduino Uno |
| ------------- | ------------- |
| Power  | External Power Source |
| GND  | GND  |
| Signal | D9  |

### 2. Arduino Firmware
1. Open Arduino IDE
2. Connect to Arduino Uno in 'COM4' port
3. Flash **RS485GripperControl** sketch to Arduino Uno

# ROS2 SOFTWARE
### Requirements: 
1. ROS2 (Humble recommended)
   **If using a virtual machine, ensure devices (Arduino Uno and USB-to-RS485 Adapter) are successfully recognized and given full permissions using
 ```bash
sudo chmod 666 /dev/ttyUSB0
 ```
   or
```bash
sudo chmod 666 /dev/ttyACM0
```


2. Python3.8+
### ROS2 Dependencies: 
- `rclpy`
- `std_msgs`
- `colcon` build system
- `rosdep` (dependency manager)
### Python Dependencies:
- `pyserial`
  ```bash
  pip install pyserial

# STEP 1: Create ROS2 Workspace
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws

colcon build
source install/setup.bash
```
# STEP 2: Create package inside ros2_ws/src
```bash
ros2 pkg create --build-type ament_python gripper_commander
```
# STEP 4: Inside gripper_commander add serial_node.py
```bash
code .
```
This will open Visual Studios where you can navigate packages and python scripts. Create python script and add code for **serial_node.py**
# STEP 5: Build Package inside ros2_ws
```bash
colcon build --packages-select gripper_commander
```
# STEP 6: Run Node
```bash
ros2 run gripper_commander serial_node
```
In second terminal run open and close commands:
```bash
ros2 topic pub /gripper_command std_msgs/String "data: 'open'"

ros2 topic pub /gripper_command std_msgs/String "data: 'close'"
```
# IR REMOTE-CONTROLLED GRIPPER
This section will explain how to control the gripper using an IR sensor with a paired IR Remote. Because of early challenges with integrating a pressure sensor, the remote was added as an alternative method to control the dexterity of the gripper
### Hardware Components: 
- Arduino Uno
- IR Reciever Module
- IR Remote
- FT6335M Servo Motor
- 5V 2A Power Supply w/ Barrel Jack (used as an External Power Source)
---
# How to Set Up

### 1. Wiring 
| Arduino Pin  | CONNECTION TO |
| ------------- | ------------- |
| IR RECIEVER SIGNAL  | D3  |
| IR RECIEVER GND  | GND  |
| IR RECIEVER VCC | 5V |
| SERVO SIGNAL PIN  | D9  |
| SERVO VCC  | External Power  |
| SERVO GND  | GND  |

### 2. Arduino IDE
#### ARDUINO LIBRARIES TO INSTALL:
IRremote by Armin Joachimsmeyer
# STEP 1: Find Remote Codes
1. Upload **remoteCodes.ino** sketch
2. Open Arduino Serial Monitor
3. Set Baud rate to 9600
4. Designate 2 buttons to trigger "Open" and "Close" and press
5. Write their Hex values and replace their values in Arduino Sketch **remoteControlGripper.ino**
# STEP 2: Upload Sketch
1. Upload finalized code with hex values to Arduino
2. Press "open" and "close" buttons to trigger servo and gripper accordingly


