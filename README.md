# voxel-game-binaries

Binaries for a voxel game (engine?).

# version 5

The current version of the game (engine).

# version 4

Tried to implement some mining gameplay.

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

# version 3

The earliest version I could find on my computers.

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

# version 2

Version 2 is a rewrite of version 1 using raw OpenGL.

I still have source code for it, but the compiled executable seems to have some problems with Intel integrated GPUs.

I wrote a OpenGL api wrapper called `directgl` for it. It's just a wrapper for the
OpenGL [Direct State Access(DSA)](https://www.khronos.org/opengl/wiki/Direct_State_Access) apis.

# version 1

I couldn't find any file for this version.

I think they were on my self-hosted git server, but that server was long gone.

Version 1 is just some basic voxel world rendering written with the [magnum](https://github.com/mosra/magnum) framework.
