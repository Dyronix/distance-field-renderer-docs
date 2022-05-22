# Chapter 4: Details of the Experiment
---

In this section, we will elaborate on specific details of the experiment. Details on the used hardware and how a single iteration in the prototype will be measured can be found in this chapter. These measurements will test the performance of the proposed algorithm. The algorithm includes sphere tracing against a distance field of a complex structure and rendering them in a real-time environment. We will compare these measurements to the performance of a rasterised complex structure rendered in a real-time environment. As stated in Chapter 3, we consider a lattice to be a complex structure for the sake of our demonstration. The focus during the iterations lies mainly on the comparison between the two rendering approaches, not on improving them. Both topics are well-established, and expanding on them would necessitate further research, which would be impossible to conduct in a year’s time. This does not, however, imply that we can sacrifice the visual component of each final render.

These measurements will be assembled by the instrumentor provided with the code-base and exported to a JSON file. The metrics within the JSON file will include the number of microseconds particular functions took to execute. Using these metrics, we can compute the theoretical framerate of an application. The exported measurements can be visualised using the Chrome Tracing Tool. This tool can visually demonstrate the timings of each function of a single iteration. However, when a comparison had to be made between different iterations, values from the JSON files are extracted and pushed into a CSV file to visualise them in a single table.

The parameters that will be measured are as follows: 

-	The input geometry that needs to be visualised. Two types of geometry will be measured against each other. These geometry types consist of a mesh represented with vertices and faces in an .obj format and a distance field with the distance evaluations towards our geometry. Each distance field has a different set of parameters that can be evaluated individually
    - Max iteration steps a sphere tracer can take
    - The minimum surface distance we consider a hit
    - The maximum travel distance along a ray
-	Resolution of the rendered scene. The picked resolutions are 1920x1080 (or High Definition (HD)) and 3440x1440 (or 4K)
-	The amount of lights within the scene. Light calculations ask a heavy toll on the GPU due to our deferred rendering system the effects of these calculations should be reduced to a minimum.
-	The iterations steps of our sphere tracer. Note that this setting only influences the distance field renderer.
-	The loading time for each type of geometry. This should not affect the application’s runtime; however, when we cannot load a specific piece of geometry or when hardware constraints are reached, no measurements can be made.
-	Procedural unit cell generation. A technique specifically designed for the visualisation of lattified geometry, which would eliminate the upload of an already lattified volume as the unit cells are determined on the fly within the shader. 
For our prototype we set out to reach 60 FPS. The human eye perceives the illusion of motion at a refresh rate of 24 FPS, however industry standards of real-time 3D applications have settled on 60 FPS. In a 60 FPS environment, one frame should be calculated in at least 1 / 60 seconds or 16.667 milliseconds.

[Appendix A.2] explains the core framework setup we used. This section will go over the core API of the prototype and has little to no impact on how we measured our findings. The API is considered an implementation detail. 
