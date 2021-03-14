---
name: VR Development Diary Entry Three
description: Back to Unity
layout: default
date: 2021-01-01
---

# VR Development Diary Entry #3
### More Unity

### How it started:

<video controls width='700'>
    <source src="/assets/vr/old-whack-a-mole.mp4">
</video>

### How it's going:

<video controls width='700'>
    <source src="/assets/vr/kinetic-arcade.mp4">
</video>

Now that I'm dangerous in Blender, I created an arcade interior to upgrade my crappy non-uv-unwrapped cube. Massive project cleanup time!

## Lighting

Lighting, unsurprisingly enough, is a deep topic in Unity. While I don't understand all the ins and outs yet, here's my take so far.  I'm using three different types of lights -

* Emissive Materials
* Baked Lighting
* Realtime Lighting

Initially emissive materials were what I used for everything, since Unity's offered light strategies (point, spot, etc) seem to look best if they actually have a model (think lamp shade) attached. The idea with emissive materials is - it's just a model with a material that can glow, basically. The drawback is that it only contributes to baked (static) lighting. 

I use emissive materials on the skeeball machines, punctuated with some spot lights where I can find places to sneak them in. The problem is finding ways to incorporate them and have them look natural. I'm planning on fooling with their intensity to accentuate starting/finishing the game, high scores, etc.

Baked lighting is the act of recoloring materials based on the properties of the materials (reflectiveness, etc) and the lights and light colors around them.  It seems like a great fit for VR because it can create a mood without using a lot of resources in real time. I used baked lighting extensively in the arcade.

As mentioned earlier, I added some realtime lighting to the skeeball machines as well. The main purpose for these is to add some visual interest to the objects that do need to move - the player hands and the balls. Otherwise they wouldn't get any light sources and end up looking very dull and out of place. 

One tip I figured out too late - Window -> Rendering -> Lighting pops up a lighting window, from there you can drag the tab back onto the main Unity window.  I like having Lighting up as a tab to periodically bake a low quality lightmap as I adjust things.

## Gameplay

Working on the "skeeball game loop" as I putz with everything else.  Right now the core ball roll feels decent but could be improved, you press the button to start and the balls roll down the chute, and the balls go through the holes in the goal model and become reclaimed by the ball return. I still need to keep track of the scores and display that to the player.

## Materials and fluff

The arcade is coming together with the help of a few asset store purchases. It has a bar/food area, some arcade cabinets, bathrooms, and air hockey (I have some VR ideas for this...). Materials - I found a nice asset for "realistic" painted metal which I'm using for the cabinet for the skeeball machine, and most of the rest of the materials for the floor, brick, skeeball roller area I'm using a great site named [cc0textures](http://cc0textures.com).

## Next Steps

I like the mood so far that I've got for the arcade, however, I do need to add a little bit of animation, since it feels pretty stiff. I'm playing with tweening the realtime lights to add feedback, as well as looking into the particle system to add some effects although I'm not sold on it - since I'm keeping things very "realistic" right now I don't know if I'll end up liking it or not. 

<video controls width='700'>
    <source src="/assets/vr/kinetic-arcade-particles.mp4">
</video>