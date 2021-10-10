---
name: VR Development Diary Entry Six
description: THE CLAAAAW
layout: default
date: 2021-07-01
---

## In the beginning, there was an idea

I began my claw journey with a bet from a friend. He took the VR arcade for a spin and was disappointed that the claw machine was a static asset! So I went to work. 

## The claw itself

Modeling the claw wasn't too bad, mainly simple geometry that needed to be rotated and duplicated a few times to create the claw fingers. I learned something very important though - the origin point in blender is very important, as it becomes the pivot point in Unity. I made sure to be very mindful of my origins in blender so that my claw fingers later on rotated around the joint on the machine.

![claw](/assets/vr/claw.png)

Modeling the rest of the machine wasn't too exciting, and currently suffers from some of the same problems the other machines do. I need to learn a bit more hard surface modeling techniques to spice them up and add details. I also need to understand surface textures more, they've all got a painted metal texture I applied from a bundle in Unity - ideally I'd texture them all separately in Surface Painter. So many things to learn!

## The claw mechanics

The next step was tying the claw to inputs. I needed a way to control left/right/backwards/forwards and up/down. I opted for the mechanic where you press and hold the button for down, then when you release, the claw closes and returns to the starting position. I worked on the inputs initially in the unity editor so I'd have quicker turnaround - a quick turnaround for changes is very important!

<video controls width='700'>
    <source src="/assets/vr/claw-mechanics.mp4">
</video>

## The joystick

Almost there! Next step was to wire the inputs to a VR world joystick rather than my current keyboard "wasd" setup for debugging. 

This was CHALLENGING!

My first blush at the wiring was to determine the rotation of the joystick - how far off of vertical it was, and which direction, determined velocity and direction of the resulting claw movement. This turned into a total disaster that I'm still not sure I've recovered from! Euler angles, quaternions, oh my. What I learned is that there are (mostly) internal/obfuscated rotation descriptions that the engine uses to determine how a 3d object is rotated in space, and it's extra complicated to prevent something called "gimbal lock". Euler angles are the more human readable rotation descriptors, but in practice I couldn't rely on them (maybe because of this gimbal lock mechanic?). 

My solution (as a clever/lazy programmer!) was to reframe the problem to remove the rotation problem completely. I added invisible game objects on 4 corners of the joystick, north, east, west, south, and a fifth on the top of the joystick. Basically think of them like little sensors. As the two sensors get closer to one another (distance measurement, very easy and understandable vs rotation) - this is how I know the direction and velocity the claw should be going.

Here's a video of me rambling (oculus lets me record voice on these vids now, for better or worse) about this whole rotation problem.

<video controls width='700'>
    <source src="/assets/vr/claw-ramble-one.mp4">
</video>

Anyway! Once I sorted that out, I had a little more work to do on the joystick, and eventually a lot of little cleanup to do on the claw machine, but the bones are there. More rambling ensues.

<video controls width='700'>
    <source src="/assets/vr/claw-ramble-two.mp4">
</video>