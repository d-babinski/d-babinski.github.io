---
title: "Audio Propagation System with FMOD"
excerpt: "Geometry based audio propagation system for set of rooms and exteriors."
---

References:
- [Hitman](https://ubm-twvideo01.s3.amazonaws.com/o1/vault/gdceurope2015/Boev_Stepan_Sound Propagation in.pdf)
- [Video form of hitman presentation](https://www.gdcvault.com/play/1022774/Sound-Propagation-in)
- [Example spatial audio implementation](https://drive.google.com/file/d/140Nbs2cJ5GJeMNf_92SEu92Z4GDIi1Wi/view)
- [Quantum Break & control spatial audio talk](https://www.youtube.com/watch?v=WsXHcDyMhWM)
- [Wwise spatial audio sdk](https://www.audiokinetic.com/fr/library/edge/?source=SDK&id=spatial_audio.html)
- FMOD Documentation


# Abstract

Usually when considering three dimensional levels with complicated geometry and exteriors combined with interiors, there's a demand for more complex solution to how audio from different sources is heard.

In this post I will explain the technological approach and concepts of audio propagation system made for Unity game, based on how it was done in several AAA titles.

# Definition, requirements and structure

To get full grasp of the system I'll introduce several terms and concepts of what was required of said system.

## Terms and requirements

### Terms

- Obstruction - obstruction is a situation when sound is blocked by elements that are not part of global geometry (everything that isnt a room or portal) ie. pillar/box

- Propagation - movement of sound through rooms and portals

- Propagation cost - value between 0 and 1 that describes how hard it is for sound to reach the listener(player) through portals and rooms

- Occlusion - situation when emmiter and listener are in different and accousticly seperated areas, leads to a muted sound and usually means parameter of obstruction or propagation cost being 1 (in range of [0..1] )

- Room - a node of a graph with a static weight that describes an area within which the sound is perfectly hearable, it has its own propagation cost

- Portal - an edge of a graph with dynamic weight that is a connection between rooms, it can change the state to closed, open and partially closed changing its propagation cost


### Functional requirements

- Rooms and portals form a graph with listener(player) having assigned a room that he’s currently in.

- If we’re currently obstructed but line of sight is getting restored the transition between muffled and not muffled sound should be smooth.

- Sound that emanate from different room  with portal being open or partially open should appear to be heard from that opening (ignoring the fact where exactly the speaker/sound is).

- Sound should get clearer in a cone simulating sound diffracted on the edges of opening. Sound direction should be appearing to be normal (front) of the portal but even with crystal clear sound unless player is in the room that the speaker is direction should stay the same. When listener is directly in front of opening sound should stop being occluded at all (keeping the direction till player enters the room).

### Nonfunctional requirements

- Immersive
- Consistent
- Computionally Inexpensive
- Support for dynamic geometry
- Reasonable implementation time


## Structure

### Propagation geometry

It is not worth to try to read the data about level geometry, the effects are much better when used manually as a seperate system made of aforementioned rooms and portals where rooms represent both indoor and outdoor enviroments and portals are openings/connections between them.
    
Rooms need to keep track of sounds played in them. Portals are mostly used for calculating the propagation path and cost of sound propagation.

Rooms can be 3d primitives (polygons are possible too although usually theres no need for them). These primitives can be connected to create a more complex shapes.

Portals are 2d primitives rotated and positioned in 3d space.

Rooms can be nested, meaning we can have one shape for whole building and nested roomsinside.

Every room and portal consist of occlusion value (propagation cost) - it definesabsorption of the sound by walls (different for wooden or concrete walls) and openings.These values can be dynamic and gameplay driven ie. doors opening and closing.

There is no documented way for automatic creation of geometry for sound so it all needs to be placed manually. The increase in number of rooms and portals doesn’t impact performance in a significant way.

![As in presentation](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eaf1e053-1075-4807-b720-de98d3fe4c4d/Untitled.png)

Occlusion - the way occlusion is counted is by finding the closest point to listener on the portal and then checking the angle between vector to listener and portal normal which defines interpolation value between portal and room occlusion values.

![How its done in hitman](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16777faa-212a-4100-84d0-a65b68cf7375/Untitled.png)

Depending on interpolation value occlussion will gradually transition between room’s and portal’s occlusion value(propagation cost). This means as player is approaching a door sound will have the cost as the player was directly in the place of portal instead of room he’s in. (basically sounds, as said previously, get more hearable)

Counting the angle between vectors [speaker → portal] and [portal → player] is less immersive.

Best practice is to compute the occlusion (propagation cost) for portals individually and this way theres no need to calculate propagation cost of all sounds. It is pratically faster and sounds empirically better.

Consider a situation where there’s more than one portal connecting two same rooms as below:

![Propagation graph](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80ad98b9-3e91-4724-bfad-29045995ddac/Untitled.png)

Approach is to compute the propagation cost of both portals and choose the one with lower value. (by no means playing sound from both openings) In this situation considering P1 and P2 have the same cost (same open kind of door) the preferable choice is P2. If P2 was a closed door and P1 an open door/window P1 is preferred.

### Propagation paths

Diffraction through portals should result in longer path and therefore attentuation falloff.++