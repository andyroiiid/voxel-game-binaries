# voxel-game-binaries

Binaries for a voxel game (engine?).

# version 5

The current version of the game (engine).

[The physics engine is slightly modified from version 3](#physics).

# version 4

Players are flying in this version.

```
horizontal movement: WASD
ascend: space
descend: left shift
left click: destroy block
quit: alt+F4
```

Tried to implement some mining gameplay.

This version will randomly flip the textures to reduce repetition. I learned this technique from some minecraft mod
(I think it's OptiFine?).

Integrated the [FMOD](https://fmod.com/) sound engine.

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

The earliest version I could find on my computers.

![screenshot](https://raw.githubusercontent.com/andyroiiid/voxel-game-binaries/master/screenshots/version-3-0.png)

```
move: WASD
jump: space
left click: destroy block
right click: create block
quit: alt+F4
```

You can jump at any time (even in the air).

I implemented a [fake ambient occlusion effect](https://iquilezles.org/www/articles/voxellines/voxellines.htm) in this
version. This technique, however, is verbose to set up and introduces tons of problems when generating chunks with
multi-threading, so I didn't migrate it to the next version.

## physics

The physics engine is a simple custom one written from scratch.

It doesn't contain any broad-phase or narrow phase, and it only supports collision detection and ray-casting.

The player has an AABB collider instead of a capsule.

# version 2

Version 2 is a rewrite of version 1 using raw OpenGL.

I still have source code for it, but the compiled executable seems to have some problems with Intel integrated GPUs.

I wrote a OpenGL api wrapper called `directgl` for it. It's just a wrapper for the
OpenGL [Direct State Access(DSA)](https://www.khronos.org/opengl/wiki/Direct_State_Access) apis.

# version 1

I couldn't find any file for this version.

I think they were on my self-hosted git server, but that server was long gone.

Version 1 is just some basic voxel world rendering written with the [magnum](https://github.com/mosra/magnum) framework.
