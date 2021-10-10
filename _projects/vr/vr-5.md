---
name: VR Development Diary Entry Five
description: Clowns and Anvils
layout: default
date: 2021-05-26
---

## Skeeball: Done, for now

Officially moving on from skeeball for now. While there's plenty of work I could do on the model to make this much more interesting, the game technically works and I want some space from it in order to think more about how I'd improve it. Here's a glimpse at the final result!

<video controls width='700'>
    <source src="/assets/vr/skeeball-1.mp4">
</video>

## Creepy Clowns

Back to the arcade. While I mostly just really wanted to make skeeball, an arcade is not complete with only a single machine! I considered a few games, but have a lot of nostalgia for "Down the Clown". Perhaps there is a German word for nostalia of being creeped out, because the clowns that you hit are certainly a creepfest.

![clowns](/assets/vr/clowns.jpg)

This felt like a good target game because it's still oriented around throwing a ball, and gives me a bit of a modeling and texturing challenge with the clown. I had to research a couple major things - how to put an image texture on the front for the face, and how to do his hair. 

For the hair, I followed a technique called Hair Cards - my implementation is fairly simple but the overall idea is just to create planes that are textured with a picture of hair. I had to massively fiddle with my image to make sure the blacks turned into alpha in unity, stuff like that. 

The clown image was a bit more straightforward, just import and line it up on the UV in blender, then it (mostly, I had to fiddle with it) just imports into unity. At first I used the default brush in blender to make a clown that only a mother could love...

![my crappy clown](/assets/vr/my-clowns.png)

Which I then used as leverage to bug my artistic wife to give me a better version that she digitally painted, ha. 

I tried to match the style & ambiance of the current arcade. It isn't quite right, but the real life machine is very cartoony so I did my best to tone that down. I want to move on from it, but I may circle back at some point to update the machine model to be a little slicker. 

Finally, I wired up some scripts to run the interactions. The clowns turn different colors (using a volumetric lighting plugin in order to maintain performance on the headset). Green scores higher, red docks points. Here is a vid of the game in action - 

<video controls width='700'>
    <source src="/assets/vr/clown-game.mp4">
</video>

## Fancy Anvils

With that game under my belt I turned back to blender. Overall I felt like I could model most simple things eventually but needed more upfront instruction on more complicated builds. With that in mind, I started on Andrew Price aka Blender Guru's Intermediate Modeling series - the anvil. This was a good tutorial where primarily I think I understand a bit more about UV unwrapping, how to model especially how the boolean modifier works, how to create nice simple curves, and baking normals and textures. I'm thinking I may try Substance Painter ultimately as far as texturing goes, but it was good to know how it's done in Blender.

Well I won't leave you hanging, here's my personal finished anvil! I'm pretty happy with how it turned out, especially how the high poly details look on the low poly mesh.

![anvil](/assets/vr/anvil.png)

## What's next??

More games! I want to try a claw game - pretty challenging so far. I'm messing with joints and tweens right now to try to create the claw open <> close motion. It's been good unity practice, I only wish there was an Andrew Price for Unity tutorials!

