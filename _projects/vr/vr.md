---
name: VR Development Diary Entry One
description: Messing around with Oculus and Unity
layout: default
date: 2020-10-01
---

## Intro

I bought an Oculus Quest 2 a month ago or so when they were released. I was very, very late to this whole VR scene but I was instantly hooked upon seeing the possibilities for immersive experiences. While I'm still getting used to the Unity editor, I do enjoy the ease of use and pure magic of having gravity, physics, colliders, lighting, etc all set up and ready to be used with a few simple radio toggles. 

I figured I'd document my experiences as I go - the tutorials I'm using, what works, what doesn't, bumps and snags, all that fun stuff. I'm a few weeks in already so I'll have to take a moment here to rewind and remember what I've done.

## Week One

My first few tutorials were focused on Unity:

### [John Lemon's Haunted Jaunt](https://learn.unity.com/project/john-lemon-s-haunted-jaunt-3d-beginner)

This was a nice introduction to Unity 3D. The class goes over prefabs, animations, manipulating the scene, rays and raycasting, and more. It was a bit of a slog since it wasn't directly VR, but I wasn't very comfortable in Unity yet so it was a great way to practice.

### [VR Beginner: The Escape Room](https://learn.unity.com/project/vr-beginner-the-escape-room)

This was fairly short and focused. It is focused on the Unity XR tooling which has some overlap with the Oculus Unity SDK. I'm still not 100% on the differences or why you'd use one or the other (cross platform compatibility aside).

There are two scenes, one is the escape room (ooh, cool!) and the other is just a prototype room with a few objects. The escape room would have been fun to put together but it's not the subject of the tutorial, sadly. I should come back to this now, but at the time I wasn't savvy enough in Unity to really appreciate the scene without any learning context. 

## Week Two

Next steps were focused on VR - two Unity VR specific tutorials on Udemy that I found that I believed would compliment each other nicely. 

### [Oculus Quest Development With Unity](https://www.udemy.com/course/oculus-quest-development-with-unity/)

Doesn't get more focused than that! The VR Beginner Unity Learn course didn't have much meat on the bones, so this was a perfect follow up. This class used the Oculus Unity SDK instead which I've stuck with since simply due to being more familiar with it. 

This class primarily has you build a scene by importing Quidditch assets, adding some props and grabbing to interact with them, the finishes off with UI and explaining how to use the debug console.

### Rabbit hole alert!

I came back to this debug console idea much later and spent half a day futzing with it and completely breaking my project. The otherwise simple 20 minute tutorial dove into manipulating code deep in the bowels of a plugin off of the asset store, which wouldn't compile for me, and one thing lead to another till my project wouldn't build even after removing all the crap. I ended up upgrading Unity a couple bugfix versions ahead and doing various rain dance project cleans until things started building again. 

Surely there is an asset in the store that does this out of the box for VR, but it was a nice to have rather than a need to have so I haven't circled back.

There's a second half to this tutorial where you put together a game that looks a lot like beat saber, but it seems to give you much of the code rather than walk you through it, instead focusing on the VR specifics. I may circle back to this, but it wasn't exactly what I wanted next so I put it on the shelf.

### Week Three

### [Build 30 Mini VR Games in Unity From Scratch](https://www.udemy.com/course/build-30-mini-virtual-reality-games-in-unity-3d-from-scratch/learn/lecture/6230108#overview)

Alright! So after going through the previous Quidditch tutorial, I could blow through some of these mini games to keep practicing different ideas in VR right? Well, kinda. Turns out this class is focused on Google Cardboard. 

I improvised at this point - I know enough about Oculus VR dev to be dangerous, why not try to adapt the minigames to Oculus. So I started on the first tutorial (Whack a Mole). I gave myself the challenge of attempting to finish the game myself after designing some of the initial 3d items.

### Interlude: Whack a Mole

Assets used: 

* [Free casual game SFX pack](https://assetstore.unity.com/packages/audio/sound-fx/free-casual-game-sfx-pack-54116)
* [DOTween](https://assetstore.unity.com/packages/tools/animation/dotween-hotween-v2-27676)
* [Punch and Fighting Sounds](https://assetstore.unity.com/packages/audio/sound-fx/foley/punch-and-fighting-sounds-132898)
* [Stylized Hammer](https://assetstore.unity.com/packages/3d/props/tools/stylized-hammer-162874)
* [Arcade Room Interior](https://assetstore.unity.com/packages/3d/environments/arcade-room-interior-165950)

My first completed VR mini game! Revelations and breakthroughs while working on this one - 

* Right click + WASD[Q/E] is __HUGE__ working in the scene. I instantly became way faster navigating around.
* DOTween is nice. I'm unfamiliar with the Unity primitives for moving but familiar with tweens both from Phaser.js and CSS. I know someday I'll have to bite the bullet but for now, having something nice and simple gets me to the next step.
* The version of .net I was using didn't support async/await which was a bummer - coroutines aren't nearly as nice. Instead of using async I set timers when I needed them (probably better anyway) - since I can rely on the update loop being constantly called, I can check if a certain amount of time has elapsed then fire the delayed action.

1. [Proud of the fact I created the scene and grabbable axe here](https://photos.app.goo.gl/WUmye8jKBc5a2pu2A)
2. [More progress](https://photos.app.goo.gl/ZZQ2jmXgKfdhG4uH6) - now the "moles" can be mashed with satisfying sound effects, but I had a bug that caused them to periodically not push back up! 
3. [They move on their own!](https://photos.app.goo.gl/GtNnPioCpYspz91w8) 
4. [Finished prototype](https://photos.app.goo.gl/kqgwvVH9xaxUP2Uk8) Good enough to move on

## Next Steps

Blender. I need at least a working knowledge in order to keep going with prototypes. It'd be nice to be a pro at Blender too but it's been rough getting started. If at least I could make a few more complicated shapes than you can create via Unity, that'd keep me moving forward.

More mini games. I'm thinking Skeeball for sure, maybe that arcade game where you had to shoot the water gun, maybe others (down the clown??) Then I'll work on the atmosphere for the arcade, throw in some more retro cabinets, music, lighting, etc. 