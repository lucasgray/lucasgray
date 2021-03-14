---
name: VR Development Diary Entry Four
description: A dark mysterious library
layout: default
date: 2021-03-01
---

## Questionable Arts

My wife is working on a novel and I wanted to surprise her with a replica of one of her scenes she could actually put the headset on and walk through. Ideally I would have done a little puzzle or scavenger hunt as a cherry on top if I had enough time.  As it turned out, the scene was a bit more involved than I had bargained for but I still had a lot of fun putting it together and learned a few more things along the way!

## The Library

<video controls width='700'>
    <source src="/assets/vr/library-walkthrough.mp4">
</video>


The library building itself was supposed to have 7 floors, some grand staircases, and most importantly, a tree growing in the middle. I found some asset store purchases for library components, desks and the like, and a few different tree packages to try. 

The tree package was the most confusing, as it had things that only worked in URP or HDRP (there's a lot to unpack with the acronyms, but they're basically different project types, due to a newer way of being able to use code to write scripts in the render pipeline). I would import the asset and drag the prefab in, then basically corrupt the project due to compile errors that I couldn't resolve. I think the asset pulled in the same transitive dependency Unity was trying to offer as well - but a different version, and I couldn't resolve it. I stuck on the old fashioned 3d project since that seems best for the Quest anyway.

It's far from finished but I'm pretty happy with where it's headed anyway. It's missing lots of the little details - trim, windows, that sort of thing.

![library interior](/assets/vr/interior.png)

## The Doors

I built a set of large doors - they are supposed to be quite a bit more ornate than this, but again, time was not on my side! 

![library doors](/assets/vr/library-doors.png)

## The Lighting

It's a very dark scene for a few reasons - for starters I've been enjoying understanding and pushing the limits of the lighting system. In addition one of the first scenes in Leigh's book is set in the early morning, so it would make sense for it to still be dark out. Finally, a dark scene hides a lot of flaws :) 

## Next Steps

I still have this gap where - I've done a little bit of texture painting in the Donut course, but need to figure out how to add surface detail to models. I downloaded substance painter but don't understand it yet. The texture painting in Blender seems like it lacks some feature though, or maybe I just don't understand it well enough yet. So the best I can really do right now is make the 3d model, import it to Unity, and slap a repeating texture on it there, which works okay but doesn't really feel polished. I'll have to find a skillshare course that focuses on this step. 

Thinking I'll put the library on the shelf for now and get back to my VR arcade. I'm considering "Down the Clown" next to stay on the ball throwing wavelength.