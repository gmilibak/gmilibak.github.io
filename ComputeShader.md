

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
- ComputeShader::Dispatch(int kernelIndex, int threadGroupsX, int threadGroupsY, int threadGroupsZ);
- [numthreads(x,y,z)]
- example:
  - shaderObject.Dispatch(0, 1, 1, 1); 
  - [numthreads(256,1,1)]
  - shaderObject.Dispatch(0, 10, 10, 2); 
  - [numthreads(5,5,1)]

ComputeBuffer:
- Unity class to load data into GPU
- can hold an array
- best to define 'struct' as an element of the array
 - 'struct':
   - lightweight object or the precursor of objects used in OOP
   - can be defined in both C# and HLSL
- so how to distribute calculations on ComputeBuffer elements on the GPU cores?
  - by defining threads and threadgroups in one dimension

Cellular Automata
Emergent Behavior
Boids (Swarm Intelligence)
