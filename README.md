# CG2111A-Alex

## Aim of Project
Natural disasters kill on average 45,000 people per year, globally. They are also responsible for 0.1% of  global deaths over the past decade. The first 72 hours after a disaster are the most critical, as the actions and response of the search and rescue team during this period can determine whether the survivors live or die. 
Therefore our goal is to design a search and rescue robotic vehicle ‘Alex’ to map out the location it is placed in. It will be remotely operated, with the operator issuing Alex commands to navigate the terrain based on the map information it provides in real time. 

## Alex Functionalities
A Lidar (Light Detection and Ranging) sensor is mounted on top of Alex to map out its surroundings. The Lidar sensor spins at a predetermined frequency to provide a 360- degree view of the bot’s surroundings. These signals are fed back to the Raspberry Pi (RPi), which is then displayed to the operator for him to deduce the mapping of the area around the robot and navigate the robot through the course. 

A pair of Hall Effect Sensors, one on each wheel encoder, detects the turning motion of the wheels and sends these pulses to the Arduino to determine the distance travelled. 

An Arduino Uno and Raspberry Pi are used to send information between each other to make the robot move according to the user’s inputs. User inputs the commands for direction of movement of the robot as well as the distance and speed (PWM of the motor) to the RPi, which will send a data packet to the Arduino. Once the commands are sent, the Arduino will send back an acknowledgement packet. Depending on whether the received packet has error, bad checksum or is valid, the Arduino will send a corresponding packet to RPi to inform it. If the command sent is valid, the Arduino will execute the code and move the robot and send a sendOK() function to let the RPi know that it is done. When the Get() function is used, the Arduino will send back the relevant information of the robot, such as the number of left/right ticks, distance travelled etc.

## System Architecture
### Hardware components:
- Motors: Responsible for the actual movement of the robot which are powered by the 4 double A batteries
- Arduino and LiDar which are both connected to the RPi and communicate through UART communication. The RPi is powered by a 5V power bank.
- LEDs which are powered by the Arduino. Green LED switches on when Alex is moving and red LED switches on when Alex stops.
### Software components:
- RPi and operator laptop are connected through the same wireless network and using Transport Layer Security (TLS), the operator can send commands to the RPi to move the robot or retrieve data. 
- On a separate laptop, through dual booting or using a virtual machine to run Linux OS, the operator can run the RViz application to see the map of the robot’s surroundings more efficiently as compared to VNC from the RPi.

There are two main components connected to the RPi, the Lidar and the Arduino Uno. The Lidar is powered by the RPi and receives data from the surrounding environment and sends this data back to the RPi through the USB to UART conversion module, which is then sent back to the Robot Operating Software (ROS) master set to the operator’s laptop IP address through the local network. This data is then processed by the Hector SLAM algorithm on the laptop and depicted in the ROS Visualisation (RVIZ), a 3D visualisation tool, for the operator to see. 

For the movement of the robot, the operator inputs the direction followed by the distance and power, which is then transmitted through the RPi to the Arduino and translated into the corresponding values to be subsequently relayed to the motor driver. The motor driver, which is powered by 4 AA batteries, drives the wheels to spin in specific directions. The movement inputs also trigger the LEDs, green when Alex is moving and red when Alex stops. Data such as the distance travelled can be calculated from the number of revolutions the wheel has spun, which is captured by the motor encoder. The data can be seen by using the ‘g’ command.
