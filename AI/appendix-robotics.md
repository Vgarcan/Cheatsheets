# Appendix — Robotics (Optional)

**Not part of the Django + AI expert track.** Use this when you want a **mental map** of how ML relates to robots; your web stack stays **[AI-STUDY-GUIDE.md](AI-STUDY-GUIDE.md)** + Ollama.

---

## Table of Contents

- [1. How this differs from Django + Ollama](#1-how-this-differs-from-django--ollama)
- [2. ROS 2 (Robot Operating System 2)](#2-ros-2-robot-operating-system-2)
- [3. Simulation first](#3-simulation-first)
- [4. Where ML fits](#4-where-ml-fits)
- [5. Where classic control fits](#5-where-classic-control-fits)
- [6. GPU on your PC vs edge on the robot](#6-gpu-on-your-pc-vs-edge-on-the-robot)
- [7. If you still want Django here](#7-if-you-still-want-django-here)

---

## 1. How this differs from Django + Ollama

- **Django + Ollama:** HTTP request/response, users, databases, **language** tasks.
- **Robotics:** **time-critical** loops (sensors, motors), safety, kinematics, often **ROS 2** message graphs—not the same failure modes as a chat app.

---

## 2. ROS 2 (Robot Operating System 2)

- Industry-standard **middleware**: nodes publish/subscribe topics, call services, share parameters.
- You typically write **Python or C++** nodes; ML may run as **one node** (e.g. object detection) feeding others (planner, controller).

---

## 3. Simulation first

- **Gazebo**, **Isaac Sim**, or vendor simulators let you break software **without** breaking hardware.
- Learn navigation, transforms (TF), and sensor models before deploying on a real arm or mobile base.

---

## 4. Where ML fits

- **Perception:** object detection, segmentation, pose estimation (often CNNs or vision transformers).
- **Policy / imitation:** learning behaviors from data (research-heavy; not your first week).
- **High-level planning:** LLMs can suggest **plans** or parse commands—but **low-level** motor control still needs traditional robotics stacks and safety review.

---

## 5. Where classic control fits

- PID, trajectory planning, inverse kinematics, state machines—these remain central. ML rarely replaces **all** of this on real hardware.

---

## 6. GPU on your PC vs edge on the robot

- **Powerful GPU on a workstation:** heavy perception models, simulation, offline dataset work.
- **Edge device on robot:** Jetson-class hardware for lighter models; latency and power constraints.
- Your **12GB server** is closer to **offline / lab** inference than to a battery-powered drone unless you network carefully (latency, drops).

---

## 7. If you still want Django here

Possible but niche: **dashboard**, **fleet management**, **logging**, **remote operator UI** that calls the same **HTTP** inference or ROS bridges. The **real-time** loop stays out of Django’s request cycle.

---

*Come back here after you are comfortable with Django + Ollama; robotics is a second specialization.*
