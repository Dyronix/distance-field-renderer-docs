# Chapter 5: Discussion
---

This chapter should analyse the data and discuss our findings. Each section of this chapter goes over the individual parameters that have been measured.

## Rasterisation vs. Distance Field Rendering

This section should explain the difference between both rendering techniques. The rasteriser would increment the triangle count based on the voxel count within the volume.

-	Voxel Count 
    -	90x90x90
    -	300x300x300
    -	600x600x600
    -	900x900x900
-	Triangle equivalent is dependent on the used mesh below is an average of all used geometry
    - ~ 65’000 triangles
    - ~ 1’600’000 triangles
    - ~ 13’500’000 triangles
    - ~ 61’000’000 triangles

Each scenario should be rendered with different geometry, this geometry can be found in [Appendix A.5]

A graph should illustrate the loading times of the assets geometry/volume and shaders. A graph should illustrate the framerate of the run scenarios.

Within the volume renderer the max iterations steps, minimum surface threshold and maximum travel distance should be evaluated to measure if there is any impact on performance when these values are changed.

## Resolution of the renderer scene

This section should explain the difference between both the HD and the 4K version of both rendering techniques. 

A medium version of each asset type should be picked to put some stress on the hardware when rendering these scenes.

-	Voxel Count
    -	300x300x300
-	Triangle Count
    -	~ 1’600’000 triangles
Each scenario should be rendered with different geometry, this geometry can be found in [Appendix A.6]

A graph should illustrate the loading times of the assets geometry/volume and shaders. A graph should illustrate the framerate of the run scenarios.

Within the volume renderer the max iterations steps, minimum surface threshold and maximum travel distance should be evaluated to measure if there is any impact on performance when these values are changed.

## Lights

This section should explain the difference between the usage of a different amount of light sources. Due to the nature of a deferred rendering system the impact of this parameter should be minimal. Tests need to illustrate if this is the case.

A medium version of each asset type should be picked to put some stress on the hardware when rendering these scenes.

-	Voxel Count
    -	300x300x300
-	Triangle Count
    -	~ 1’600’000 triangles
Each scenario should be rendered with different geometry, this geometry can be found in [Appendix A.5]

A graph should illustrate the loading times of the assets geometry/volume and shaders. A graph should illustrate the framerate of the run scenarios.

Within the volume renderer the max iterations steps, minimum surface threshold and maximum travel distance should be evaluated to measure if there is any impact on performance when these values are changed.

## Procedural unit cell generation.

This section should explain that a volume with a lower memory footprint can be uploaded and be lattified on the fly within a shader. 

A medium version of each asset type should be picked to put some stress on the hardware when rendering these scenes.

-	Voxel Count
    -	300x300x300
-	Triangle Count
    -	~ 1’600’000 triangles
Each scenario should be rendered with different geometry, this geometry can be found in [Appendix A.5]

A graph should illustrate the loading times of the assets geometry/volume and shaders. A graph should illustrate the framerate of the run scenarios.

Within the volume renderer the max iterations steps, minimum surface threshold and maximum travel distance should be evaluated to measure if there is any impact on performance when these values are changed.
