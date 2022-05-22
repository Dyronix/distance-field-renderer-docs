# Hardware Setup
---

We will use the following hardware setup to measure our findings:

-	Microsoft Windows 10 Home edition ( 10.0.19043 Build 19043 )
-	Intel Core i9-9900KF CPU @ 3600 MHz, 8 Cores, 16 Logical Processors
-	64GB DDR4 Memory @ 3200 MHz
-	Nvidia GeForce RTX 2070 @ 8GB Dedicated GPU memory

A quick note about hardware implementations: a hardware implementation means that we execute a job on a physical device or electronic circuit instead of being done by a computer program. Within the implementation of our prototype, we will make use of some hardware implementations that could be limited to a specific hardware setup. These hardware implementations will influence performance.

Images presented using the Chrome Tracing Tool are generated using version v24.2-cb50f  [95]. All data visualised within the Chrome Tracing Tool is generated from the prototype. RenderDoc version 1.18 build from 3996d037 is used to extract framebuffers from the prototype to illustrate visual comparisons between a deferred rasterisation renderer and distance field renderer. Finally, every heatmap shown in this chapter was created using the prototype and taken with the version of RenderDoc indicated above.
