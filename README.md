# voxel-game-binaries

Binaries for a voxel game (engine?).

# Playthrough video (YouTube)

You can watch a sample playthrough of the game [here](https://youtu.be/lNVkgavvEAs) on YouTube.

# Download and instructions for the latest version

You can download the whole repository [here](https://github.com/andyroiiid/voxel-game-binaries/archive/master.zip).

The zip file size is about 50MB. The latest version is 5.2.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-5.2.png" width=50% height=50%>

```
return to main menu: esc

FreeFall mode:

horizontal movement: WASD
jump: space
interact: left click
orange blocks are save points
green blocks unlocks the red blocks
blue/pink blocks can switch on/off blocks of corresponding color

Playground mode:

horizontal movement: WASD
ascend: Q
descend: E
destroy block: left click
create block: right click
```

**IMPORTANT!!!**

**Version 5
requires [AVX2 instruction set support](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2).**

**This game uses [OpenGL 4.6 core profile](https://en.wikipedia.org/wiki/OpenGL#OpenGL_4.6)
and [Direct State Access(DSA)](https://www.khronos.org/opengl/wiki/Direct_State_Access).**

**It's completely possible to migrate the api to use OpenGL 3.3 core profile and DSA extensions. But for now please use
up-to-date GPUs to run this game. Here is a driver compatibility list from Wikipedia:**

```
NVIDIA GeForce 397.31 Graphics Driver on Windows 7, 8, 10 x86-64 bit only, no 32-bit support. Released April 2018
AMD Adrenalin 18.4.1 Graphics Driver on Windows 7 SP1, 10 version 1803 (April 2018 update) for AMD Radeon™ HD 7700+, HD 8500+ and newer. Released April 2018.
Intel 26.20.100.6861 graphics driver on Windows 10. Released May 2019.
```

**To put it simply, if you ever received any graphics driver update these two years, you are fine.**

**I didn't implement non-DSA workarounds, so there might
be [some driver bugs](https://blog.magnum.graphics/announcements/2020.06/#certain-gl-drivers-continue-to-be-a-hot-mess)
if you run it on Intel GPUs.**

# version 5 (the current version)

I rewrote the chunk system to improve thread-safety, and added fine-grained mutexes to ensure performance.

In previous versions, the chunk updates were triggered manually when neighbouring chunks were modified. In this version,
however, I found that this method triggered many redundant updates, so I decided to use a timestamp-based update, and it
worked well.

[The physics engine is slightly modified from version 3](#physics). It will push players out if they are stuck in a
newly generated block. Correctly resolving such conflicts is complicated, however.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/magicavoxel.png" width=50% height=50%>

I use [MagicaVoxel](https://ephtracy.github.io/) for level editing. I wrote a parser to
parse [`.vox` files](https://github.com/ephtracy/voxel-model/blob/master/MagicaVoxel-file-format-vox.txt) generated by
MagicaVoxel and to convert them into my own level file format.

## version 5.2

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-5.2.png" width=50% height=50%>

```
return to main menu: esc

FreeFall mode:

horizontal movement: WASD
jump: space
interact: left click
orange blocks are save points
green blocks unlocks the red blocks
blue/pink blocks can switch on/off blocks of corresponding color

Playground mode:

horizontal movement: WASD
ascend: Q
descend: E
destroy block: left click
create block: right click
```

## version 5.1

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-5.1.png" width=50% height=50%>

```
FreeFall mode:

horizontal movement: WASD
jump: space
interact: left click
orange blocks are save points
green blocks unlocks the red blocks

Playground mode:

horizontal movement: WASD
ascend: Q
descend: E
destroy block: left click
create block: right click

return to main menu: esc
```

I used a new set of textures in this version because the Kenney textures seemed too dark
after [distance attenuation](#distance-attenuation).

This version also contained a Playground mode, which is a Minecraft-like mode. In fact, it had always been in the game.
But I didn't include the main menu in version 5.0, so no one could access it.

### Bullet time and why it didn't work

The jumping section was still very hard. A friend suggested me to add an ability: bullet time.

I added bullet time so that players could press right mouse button to slow down the time when airborne. But then I
realized that giving them such abilities simply nullified all jumping challenges. There isn't any time limit for the
game, so players can slowly walk through the level at ease. Eventually, I removed the bullet time.

### Coyote time

In games with coyote time, players can still jump shortly after they walk off a platform. This greatly improves the
experience and makes the physics system more forgiving. I decided not to implement such system in version 5.0 because I
wanted the jumping challenges to be really hard. Later I found out that adding coyote time didn't simplify the jumping
challenges, and some players didn't like to look down when they are jumping. Without coyote time, the physics engine
behaved strangely for these players.

## version 5.0

The textures and sound effects are from [Kenney](https://www.kenney.nl/). Thanks, Kenney!

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-5.0.png" width=50% height=50%>

```
horizontal movement: WASD
jump: space
interact: left click
dirt(brown) blocks are save points
grass(green) blocks unlocks the brick(red) blocks

quit: alt+F4
```

### The skybox

The skybox is a procedural one adopted from [glsl-atmosphere](https://github.com/wwwtyro/glsl-atmosphere).

### Lighting

I decided to make a lighting system that could smoothly switch between day and night. It was easy to implement a daytime
lighting system and [half lambert](https://developer.valvesoftware.com/wiki/Half_Lambert) worked well. But when the sun
was below the horizon, the directional lighting seemed weird. So I made another nighttime lighting system, making the
player the only light source. Then I interpolated between these two systems with the time of the day.

### Distance attenuation

After the map grew big, I realized that it was hard to recognize the spatial relations between blocks, thus I made
blocks darker when they are far away.

#### before attenuation

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/before-attenuation.png" width=50% height=50%>

#### after attenuation

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/after-attenuation.png" width=50% height=50%>

# version 4

The sound effects are from [Adobe](https://www.adobe.com/products/audition/offers/audition_dlc.html). The BGM is
Afterglow from [Tungerman](https://tungerman.itch.io/).

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-0.png" width=50% height=50%>

Players are flying in this version.

```
horizontal movement: WASD
ascend: space
descend: left shift
left click: destroy block
quit: alt+F4
```

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-1.png" width=50% height=50%>

Tried to implement some mining gameplay.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-2.png" width=50% height=50%>

This version will randomly flip the textures to reduce repetition. I learned this technique from some minecraft mod
(I think it's OptiFine?).

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-3.png" width=50% height=50%>

Integrated the [FMOD](https://fmod.com/) sound engine.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-4.png" width=50% height=50%>

The particles are GPU controlled.

```
1. set particles base location in uniform

2. glEnable(GL_PROGRAM_POINT_SIZE);

3. vertex shader: generate random movement with the gl_VertexID or gl_InstanceID
   and set gl_PointSize depending on the distance to the camera

4. fragment shader: use gl_PointCoord as texture uv

5. gl_DrawArrays(GL_POINTS, 0, particleNum);
   or glDrawArraysInstanced(GL_POINTS, 0, particleNum, instanceNum);

6. it's possible to batch render many particle systems with different textures
   using sampler2DArray and instance buffer
```

[The physics engine is from version 3](#physics).

# version 3

The earliest version I could find binaries on my computers.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-3-0.png" width=50% height=50%>

```
move: WASD
jump: space
left click: destroy block
right click: create block
quit: alt+F4
```

You can jump at any time (even in the air).

## Ambient occlusion

I implemented a [fake ambient occlusion effect](https://iquilezles.org/www/articles/voxellines/voxellines.htm) in this
version. This technique, however, is verbose to set up and introduces tons of problems when generating chunks with
multi-threading, so I didn't migrate it to the next version.

I have to admit that it looks great and I really want to re-implement it in the newer versions.

## Physics

The physics engine is a simple custom one written from scratch.

I didn't have an experience implementing a physics engine before. So it doesn't contain any broad-phase or narrow-phase,
and it only supports collision detection and ray-casting. The collider on the player supports a behaviour similar
to [`KinematicBody.move_and_slide`](https://docs.godotengine.org/en/3.2/classes/class_kinematicbody2d.html#class-kinematicbody2d-method-move-and-slide)
of the Godot engine.

Erin Catto has [a great talk](https://www.youtube.com/watch?v=7_nKOET6zwI) about how to implement physics engines. There
are also [articles](https://tavianator.com/2011/ray_box.html) about how to implement branchless AABB ray-casting.
Developers from Respawn shared [how to write SIMD version of it](https://www.youtube.com/watch?v=6BIfqfC1i7U). But I
only used some really basic techniques in this engine.

The player has an AABB collider instead of a capsule.

## Tracy profiler

I used the [Tracy profiler](https://github.com/wolfpld/tracy) to profile my game.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/tracy.png" width=50% height=50%>

# version 2

Version 2 is a rewrite of version 1 using raw OpenGL.

I still have source code for it, but the compiled executable seems to have some problems relating to dependencies.

I wrote a OpenGL api wrapper called `directgl` for it. It's
an [OOP (Object-oriented Programming)](https://en.wikipedia.org/wiki/Object-oriented_programming) wrapper for the
OpenGL [DSA (Direct State Access)](https://www.khronos.org/opengl/wiki/Direct_State_Access) apis.

It was an over-engineered version with tons of dependencies. I tried to use [EnTT](https://github.com/skypjack/entt),
which is a C++ [ECS (Entity Component System)](https://en.wikipedia.org/wiki/Entity_component_system) framework. Then I
realized that I didn't need it at all. I also used [spdlog](https://github.com/gabime/spdlog) for logging, and it
greatly slowed down the compilation speed. Logging with [SDL log](https://wiki.libsdl.org/CategoryLog) is enough for
most games.

## Chunk system

Minecraft has an 2D chunk system. It has a chunk size of 16 * 16 * 256, so there is a limit to where you can place
blocks. This system works fine for Minecraft, but I wanted to support infinite building in three directions. So, I made
a chunk system with 32 * 32 * 32 chunk size. I chose this size because you can index into any chunk with a 16 bit
integer (32 = 2 ^ 5, 32 * 32 * 32 = 2 ^ 15).

At first, I tried to manage chunks with a 3D bidirectional linked list. It turned out to be a complete mess. I had to
handle tons of edge conditions and if I wanted to access a diagonal chunk, I had to dereference multiple pointers,
hoping that none of them is a `nullptr`. So, I just abandoned linked lists and used `std::unordered_map`. It worked
great.

Generating a chunk's render data took about a millisecond, and I didn't have that much time for it on the main CPU
thread (for a 60fps game, each frame has 16 milliseconds). Therefore, I had to setup some multi-threaded processing.
Then there was another problem: OpenGL is hard to work with when you are programming a multi-threaded program. It's much
safer to only call OpenGL functions on a single thread (usually the main thread). My solution was to prepare the render
data on worker threads, and to upload them on the main thread when it is idle.

## Perlin noise world generation

I used [a Perlin noise library](https://github.com/Reputeless/PerlinNoise) for the world generation. But when I tested
it on multiple platforms, the same seed generated different worlds. MinGW GCC and linux GCC produced the same result,
while MSVC gave a completely different terrain. So I realized that there might be some problems with the standard
library. At first I thought maybe `std::mersenne_twister_engine` would behave differently on different platforms, but
that wasn't the case. The real bug is
from [`BasicPerlinNoise::reseed()`](https://github.com/Reputeless/PerlinNoise/blob/master/PerlinNoise.hpp#L125).

```c++
std::shuffle(std::begin(p), std::begin(p) + 256, std::default_random_engine(seed));
```

This is the standard way to generate the `permutation` array
from [the original Perlin noise implementation](https://mrl.nyu.edu/~perlin/noise/), but `std::shuffle` is implemented
differently in GCC and MSVC! After reading their standard libraries source code, I found that they both use
[the Knuth shuffle algorithm](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle). But there
are [two ways](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle#The_modern_algorithm) (iterating from two
directions) to shuffle an array, and they do not produce the same results. The solution was simple: I wrote my
own `shuffle()`.

# version 1

I couldn't find any file for this version.

I think they were on my self-hosted git server, but that server was long gone.

Version 1 is just some basic voxel world rendering written with the [magnum](https://github.com/mosra/magnum) framework.

## RenderDoc

I used the [RenderDoc](https://renderdoc.org/) to debug my graphics.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/renderdoc.png" width=50% height=50%>

This is a screenshot for version 3 of the game, but I've been using RenderDoc since the first version.

