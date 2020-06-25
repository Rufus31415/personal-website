---
layout: post
title: Introducing BallCuber - 4x4x4 Rubik's Cube solving robot
date:   2020-05-09
image:  '/images/introducing-ballcuber-4x4x4-rubik-s-cube-solving-robot.jpg'
tags:   BallCuber Arduino 3D .NET
hn_id: 22986398
hn_link: https://ballcuber.github.io/
reddit_id: 1589872168
reddit_link: https://www.reddit.com/r/Cubers/comments/g6k1j5/4x4x4_rubiks_cube_solving_robot/
repo: ballcuber.github.io
user: ballcuber
---

Official website : [https://ballcuber.github.io](https://ballcuber.github.io){:target="_blank"}

<p><iframe src="https://www.youtube.com/embed/5e3V5y5gV5E?rel=0" frameborder="0" allowfullscreen></iframe></p>

 The BallCuber is a robot able to solve the Revenge Rubik's Cube 4x4x4.
It has a patented, fast and innovative mechanics that allows each motor movement to turn a layer. The robot is fully 3D printed.

I've been working on this project with a friend for 2 years.

The Rubik's Cube 4x4x4 is located inside a sphere (hence the name BallCuber ;) ). Intermediate parts are located between the cube and the sphere. Stepper motors animate the machine via a gearbox and a geneva drive mechanism. When a motor rotates, it causes a row of intermediate parts to slide on the inner wall of the sphere, which spreads the movement to a layer of the cube.

The robot is controlled by PC-based supervision software as well as embedded software. The electronic cabinet is composed of Arduino boards, stepper motor drivers, motors, magnetic angle encoders and a lot of cables.

The initial state of the cube is obtained by a separate device equipped with a camera. The supervision software processes the images and an algorithm calculates the movements necessary for the resolution. Once the user puts the Rubik's Cube 4x4x4 into the robot and closes the hood, the cube is manipulated and comes out solved in about 3:20s!

In a future version, we hope to have 9 brushless motors as well as more precise mechanics to beat the world record which is 1:18s.
Feel free to browse the website or contact us for more information. 


| | | |
|:-------------------------:|:-------------------------:|:-------------------------:|
|<img width="1604" src="https://ballcuber.github.io/assets/kinematic/Sphere.png"> |  <img width="1604" src="https://ballcuber.github.io/assets/kinematic/Mounted_base.png">|<img width="1604" src="https://ballcuber.github.io/assets/kinematic/KinematicCrossSection.gif">|
|<img width="1604" src="https://ballcuber.github.io/assets/kinematic/KinematicCrossRows.gif">  |  <img width="1604" src="https://ballcuber.github.io/assets/kinematic/EpicyclicGear.gif">|<img width="1604" src="https://ballcuber.github.io/assets/patent/preview.png">|
