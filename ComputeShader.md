## Emergent behaviours on compute shaders

CPU vs GPU:
- both are computing processors
- built with different architecture and for different purposes

CPU:
- few cores
- executes a wide variety of processes on the computer

GPU:
- many cores (up to the hundreds or thousands)
- executes more specialized tasks
- best for processes that can be divided across these cores

Compute shader:
- a tool to utilize the advantages of the GPU's parallel processing
- written in HLSL (High-Level Shader Language)
- useful for simulating complex systems or systems consisting 

Defining threadcount and indexing:
- C# code in Unity to define threadgroups in three dimension <br>
```ComputeShader::Dispatch(int kernelIndex, int threadGroupsX, int threadGroupsY, int threadGroupsZ);``` <br>
- HLSL code defining the number of threads in each threadgroup for the three dimension <br>
```
#pragma kernel FillWithRed

RWTexture2D<float4> res;

[numthreads(1,1,1)] void FillWithRed (uint3 id : SV_DispatchThreadID)
{
  res[id.xy] = float4(1,0,0,1);
}
```
- example:
  - shaderObject.Dispatch(0, 1, 1, 1);
  - [numthreads(256,1,1)]
  - shaderObject.Dispatch(0, 1920 / 8, 1080 / 8, 2); 
  - [numthreads(8,8,1)]

ComputeBuffer:
- Unity class to load data into GPU
- can hold an array of arbitrary structured data
- best to define 'struct' as an element of the array
 - 'struct':
   - lightweight object or the precursor of objects used in OOP
   - can be defined in both C# and HLSL
- so how to distribute calculations on ComputeBuffer elements on the GPU cores?
  - by defining threads and threadgroups in one dimension

Cellular Automata
- grid based, each cell has its own state
- evolving in discrete steps based on a set of rules
- Conway's game of life

Emergent Behavior
- the result of the relationships and interactions between individual parts
- not explicitly described by the behavior of the components, it's a "side effect"

Boids
- algorithm to simulate flocking behaviour
- originally developed by Craig Reynolds in 1986
- three simple rules
  - cohesion
  - separation
  - alignment

Hiccups and solutions:
- race conditions on the GPU
  - using separate read and write texture: readTx and writeTx, swapped between frames
  - modify only non-read buffer attributes: velocity and newVelocity
