---
name: Advent of the Reaper Entry One
description: A tactics RPG about Reapers, Angels, and intrigue!
layout: default
date: 2021-09-01
---

## Advent of the Advent of the Reaper

While I was having fun on my own, I missed the camraderie of a team. With Delta surging the second wave of the pandemic was starting to make me stir crazy. I found a redditor looking for help with his tactics RPG and decided to try my hand at some Unity 2D work.

## Learning 2D

2D in Unity is interesting to say the least. Unity is a 3d engine first and foremost, so they implemented 2D as an extension of 3D. You can actually deselect an option to force 2D and fly around your 2d surfaces in a 3d view! The camera is set to an orthographic view (no perspective), and instead of stacking with a Z value, one uses Sorting layers and ordering in those layers to decide what gets rendered on top.

## Tutorials First

[Ruby's Adventure: 2D Beginner](https://learn.unity.com/project/ruby-s-2d-rpg) was my starting point. This was a perfect course. It took maybe 5-10 hours (I didn't do all of it, I skipped a few of the final chapters that weren't very applicable to what I wanted to build). I felt pretty comfortable in Unity already so I mostly sped through the chapters. 

I think the biggest roadblock here was learning how the animator tools work. I wouldn't say I'm completely an expert yet, but I'm learning as I go and for my purposes I'm controlling maybe 60% of it with code, 40% of it via the animator tools. It's just too easy to Tween things around, and you're already in a script when you need an animation, so it just seems to make sense to do it with code quite often.

## Pixel Perfect: Past Perfect, Into Insanity

Unity purportedly ships with a Pixel Perfect Camera. I had multiple issues with it (this was a whle ago, but if I remember correctly..)

1. Sprites would jitter around when panning the camera
2. I wouldn't get clean pixel perfect borders. Some elements were still blurry!
3. Canvas especially looked really bad

It might be worth a revisit now that I know a little more about what I'm doing, but I swapped out for [Ocias Pixel Camera](https://ocias.com/blog/unity-pixel-art-camera/) which has worked great so far. It nicely forces pixels onto elements that are non-pixel which was actually quite nice since this means I get "pixel art" elements for free, for example default UI buttons. We'll come back later and create fancy pixel buttons, but for now they're at least not an eyesore.

### Resolution...

![advent splash](/assets/advent/advent-splash.png)

The way this camera works (and I think is true for the unity shipped camera too, if it worked) - you don't have a fixed viewport size. Instead, you tell it what the optimal size in pixels is, and then the camera figures out how to scale depending on the viewport. In this way we can achieve anything from a mobile screen to a 4:3 (Switch-like) view to my large 4k monitor without any ill effects. It is important to set up canvases with this in mind, for example elements that should float towards the bottom, or top left/top right, etc. 

### Meets Units...

![grid cells](/assets/advent/grid-cells.png)

"Units" are Unity's internal representation of distance. They're related to pixels in the sense that you set pixel-per-unit on images you import. The default is 100. There are no hard and fast rules here, many suggest not to change the default, but I thought it'd be a smart idea to have 1 unit = 1 row or column in the tactics grid. This would make the tactics "game board" really easy to work with, since for example as I walk out the character's move possibilities I could simply count cells out.

This wasn't quite so simple in practice! As it turned out, I wanted *TWO* units per cell in order to achieve the view that I needed. It's a little complicated to explain, but the pixel art is 32x32, destined for a potential overall ideal resolution of 640x360. If one unit was 32x32, the camera ended up zoomed in too far and we didn't see enough of the map. But if we doubled the ideal resolution, I was worried that it'd be too big for the small Switch screen. So I elected to make each grid cell 2x2 units, and the PPU (pixel-per-unit) 16, so all of our 32x32 art would expand over 2 units. Complicated but what can you do! 

## More to come!

I knew if I could prove out enough of the tactics combat, I could write the rest easy enough. I was a little daunted by how I'd figure out how far a character can move, how to design the enemy AI, how to design the dialogue system (and how to trigger it), and how to write the tactics "sub-states" in a way that abstracts input (for when we build for console, switch, mobile!) away from the game logic. So I dove in!  

