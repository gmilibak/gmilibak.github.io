## Ray tracing, path tracing

### Physical light
- bouncing uncountable times until reaches the viewer's eye/camera
- transporting information about all previous encounters
- interaction with materials
- most rays does not perceived at all
- reflection
- diffusion <br>
<p align="center">
  <img src="/png/reflection-specular-diffuse.png"/>
</p>
- refraction <br>
<p align="center">
  <img src="/png/refraction.png"/>
</p>

### Ray
- origin
- direction
  
### Ray casting
- imitating light traveling along a path
- can be used for rendering, collision detection and more

### Classic ray tracing
- ray shot from the eye/camera (primary rays) through every pixel
- ray intersect objects
- find closest intersected object
- ray reflected or refracted (or both, secondary rays)
- shadow ray shot towards light source (secondary rays)) <br>
<p align="center">
  <img src="/png/Ray_trace_diagram.png"/>
</p> <br>

### Path tracing
- follow one ray over a series of reflection
- the reflection direction is only known for a mirror surface
 
### Bounding Volume Hierarchy (BVH)
- series of ever shrinking bounding volumes for which a hit or miss value is calculated and in the innermost bounding volume we only have to check a handful of triangles for ray-triangle intersection <br>
<p align="center">
  <img src="/png/BVH_schematics.png"/>
  <img src="/png/BVH_model.png"/>
</p>

### Rendering equation

### Material properties
- color
- roughness
- specular (shiny) or diffuse reflection (matte)
- specular and diffuse reflection (glossy) <br>
<p align="center">
  <img src="/png/reflection-light-angle-incidence.png"/>
</p>
 
### Hardware
- specialized hardware for ray tracing (RT Cores)
- RT Core performs
  - BVH traversal
  - ray-triangle intersection

### Effects
- hard shadows
- soft shadows
- global illumination (indirect lighting) <br>
<p align="center">
  <img src="/png/Different-shadows.jpg"/>
</p> <br>
- glossy reflections
- background/foreground blur (depth of field)
- motion blur

### Real time ray tracing
- optimized sampling
- denoising (AI-supported) <br>
<p align="center">
  <img src="/png/denoising.jpg"/>
</p> <br>
- sofwares are continuously getting optimized


### Interesting stuff
- [Cornell box](https://www.graphics.cornell.edu/online/box/compare.html)
- [Arago spot](https://en.wikipedia.org/wiki/Arago_spot) (light diffraction)
