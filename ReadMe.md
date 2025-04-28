# Gesture Tower: Embedded Multiplayer Gesture-Controlled Game

This project implements a gesture-based multiplayer gaming platform using BeagleY-AI embedded boards, MediaPipe hand tracking, and a live React web client. Players use real-world gestures to battle each other, with state synchronization maintained through a Node.js WebSocket server.

---

## Overview

- Two (or more) BeagleY-AI boards connect to a centralized Node.js server via WebSockets.
- Each board captures hand gestures through a USB webcam, interpreted using MediaPipe Hand Tracking models.
- Players use joysticks and rotary encoders to control when gestures are captured and submitted.
- LCD displays, audio feedback, and live animations provide real-time player status.
- ReactJS + Framer Motion web client shows synchronized game state to users.

- For more info please refer to the following YouTube Overview: https://www.youtube.com/watch?v=HzwpM-fz1Ek 

---

## The Web Client is Hosted as Shown Below: 

Web Client: https://433-project.vercel.app/ 

Note that in order to fully run an active game, 2 BeagleY-AI boards need to be connected to the same room and be in the "ready" state.

However the gesture-recogniton can still be observed using a single BYAI.

---

## Tech Stack


### Web Client (React)

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Framer Motion](https://img.shields.io/badge/Framer%20Motion-0055FF?style=for-the-badge&logo=framer&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

### Web Server (Node.js)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=for-the-badge&logo=websocket&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

### Embedded System (BeagleY-AI + C++)

![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-4285F4?style=for-the-badge&logo=mediapipe&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![Bazel](https://img.shields.io/badge/Bazel-76D275?style=for-the-badge&logo=bazel&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

---

## Prerequisites

Install these on the BeagleY-AI (ARM64 environment):

```bash
# Install Bazel
sudo apt install curl gnupg -y
curl -fsSL https://bazel.build/bazel-release.pub.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/bazel-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/bazel-archive-keyring.gpg] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
sudo apt update && sudo apt install bazel

# Install OpenCV
sudo apt install libopencv-dev:arm64 python3-opencv

# Install JSON parsing library
sudo apt-get install nlohmann-json3-dev:arm64
```

Make sure you have Node.js v18+ and npm installed on your development machine.

---

## How to Clone

```bash
git clone https://github.com/GurmukhSKharod/Gesture-Tower.git
cd gesture-tower
```

---

## Setting up the Web Client

```bash
cd client
npm install
npm start
```
Opens at http://localhost:3000

Displays real-time animations of the game state.

---

## Setting up the Web Server

```bash
cd gesture-tower-server
npm install
node server.js
```
Runs WebSocket server on port 3001.

Manages all connections to players and web client.

---

## Setting up Embedded Gesture Recognition (BeagleY-AI)

Step 1 — Copy the Mediapipe Project
```bash
cp -r mediapipe ~/mounted_folder/public/
```
Step 2 — Build the C++ Executable
```bash
cd ~/mounted_folder/work/project/mediapipe
bazel build -c opt --crosstool_top=@crosstool//:toolchains --compiler=gcc --cpu=aarch64 --define MEDIAPIPE_DISABLE_GPU=1 //bazel_project_build:gesture_game
```
Step 3 — Move Executable into Public Folder
```bash
cp bazel-bin/bazel_project_build/gesture_game ~/mounted_folder/public/mediapipe/
```
Step 4 — Run on BeagleY-AI
```bash
cd /mnt/remote/mediapipe
./gesture_game
```

 - Joystick starts gesture capture
 - Rotary encoder stops capture and submits gesture
 - LCD displays recognized gestures and game state
 - Audio feedback plays after gesture submission
 - Terminal and Web Client can be used to also interact with the system, such as users leaving and joining active game sessions or starting/stopping gesture recognition.

---

## Testing the System

Component	            Test Instructions

Web Client	            - Open localhost:3000, check live updates

Web Server	            - Console logs connection events

BeagleY-AI Hardware	    - Start/Stop gestures, verify LCD updates

Multiplayer Sync	    - Use two BeagleY-AI boards to confirm live synchronized gameplay

---

## Notes

 - USB Webcam must be available at /dev/video3 for gesture detection to work.

 - Ensure all hardware peripherals (joystick, rotary encoder, LCD, audio output) are connected properly.

 - Player states are synchronized every round automatically across all clients.