# voxel-game-binaries

Binaries for a voxel game (engine?).

You can download the whole repository [here](https://github.com/andyroiiid/voxel-game-binaries/archive/master.zip).

The zip file size is about 50MB.

**IMPORTANT!!!**

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

TODO: add TOC

TODO: fix license problem

TODO: remove decade sound

# version 5 (the current version)

The current version of the game (engine). This version
requires [AVX2 instruction set support](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2).

TODO: provide SSE build

TODO: Tracy profiler

TODO: RenderDoc

TODO: chunk system: thread-safety; migrating from manually triggered update to timestamp-based update

[The physics engine is slightly modified from version 3](#physics). It will push players out if they are stuck in a
newly generated block. Correctly resolving such conflicts is complicated, however.

## version 5.2

TODO: level rework

TODO: A-B switch and global switch

TODO: bullet-time and why it doesn't work, only kept the zoom effect; coyote-time

TODO: self-emission texture

## version 5.1

TODO: level rework

TODO: changed texture

## version 5.0

TODO: level design

TODO: daylight cycle

TODO: modify the lighting system so that player can easily recognize the distance (mist effect)

# version 4

Players are flying in this version.

<img src="https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-4-0.png" width=50% height=50%>

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

[The physics engine is slightly modified from version 3](#physics).

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

### Binary search for Time of Impact

TODO: illustration and when to terminate

### A simple way to implement ray-casting for voxel worlds

TODO: illustration and time profiling

The player has an AABB collider instead of a capsule.

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
