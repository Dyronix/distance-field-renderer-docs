# Introduction

Within modern society, many media devices exist. Having a responsive software product on any device would benefit different industries, such as individualising three-dimensional (3D) printed products, medical visualisations representing a diagnosis in a 3D environment, and even games to support a more extensive player base. 

We will explore an example from the Additive Manufacturing (AM) industry. One of the complex structures AM recently took interest in are lattice structures. Lattices are a geometrical arrangement of crisscrossed patterns that can generate all types of geometry without losing structural integrity. However, visualisations of lattice structures using computer graphics can become hard to render on sub-optimal hardware. Depending on the size of the lattice, polycount can reach millions of triangles which is not feasible to visualise optimally on consumer hardware in real-time. Additionally a representation of such geometry within a modelling tools such as MAYA or Blender necessitates a high level of knowledge, patience, foresight and meticulous preparations to ensure that models have adequate control vertices where details is desired. 

In this article, we propose a solution for developers to create a 3D renderer that uses a volumetric approach to visualise these structures. Our target platform will be the web, focusing on chromium browsers. We will use a tool called "emscripten" to convert our C++ code into a webassembly that is able to execute WebGL instruction within the context of a webbrowser. By taking this approach our application is compatible with the web and any desktop platform that supports OpenGL (looking at you Apple). This also means that state of the art technology such as mesh-shaders or raytracing is not available. However, this will make sure that our solution is compatible with all kinds of platforms as a more generic approach is utilized to achieve the desired outcome. Volumes have shown promising results in visualising high fidelity geometry without the cost of uploading the required surface-based data to the GPU. An added benefit of volumes is that they can perform Boolean operations more efficiently.

# Preliminaries

Before we can talk about the process of creating a volumetric rendering pipeline. Some key mathematics and programming ideas involved within this article have to be explained. 

## Distance Fields 

A distance field is a scalar field that specifies the minimum distance to the surface of a shape. If we examine this in more detail, a distance field can be represented by a function <b>F</b>, such that any given point <b>P</b> will return a distance <b>d</b> from the object represented by the function. We store the distances returned by such a function as 3D matrices or, more commonly known within graphics programming, a 3D texture. Each texture cell stands for the closest distance from the grid element to the nearest surface. Therefore, a grid element containing a value of 0 represents the surface of a shape. 

## Sphere Tracing

Visualising a distance field can be achieved by using an algorithm called sphere tracing. Sphere tracing is a technique for rendering implicit surfaces using a geometric distance. Which is exaclty what we stored within our 3D texture. To find the distance towards a shape, we need to define a distance function for it or have a generated volume available to trace against. For example, a sphere with center (x_0,y_0,z_0) situated at the world origin (<b>P</b>) and radius <b>r</b> can be representated as followed:

<p align="center">
<i>P^2-r^2=0</i>
</p>

This equation is what is called an implicit function. A sphere represented in this form is also called an implicit shape. An implicit equation only tells us if a particular point is inside a shape (negative values), outside a shape (positive values), or precisely on the surface (value of 0). The collection of points where the implicit function equals x is called an iso-surface of value x (or iso-contour in 2 dimensions). Sphere tracing is a method of drawing a surface solely based on this data. For more information about sphere tracing, click [here](../sphere_tracing.md)

## Boolean Operations

Volumetric data can easily represent shapes defined via Boolean operations. These operations are often used in CAD software in collaboration with a technique called Constructive Solid Geometry (CSG), which consists of the same operations only based on surface-data not on geometry, which makes this algorithm a lot more CPU intesive as new geometry has to be constructed on the fly. Modelling complex shapes by assembling simple shapes such as spheres, cubes, planes might be hard to achieve if we modelled our geometry by hand. Being able to blend implicit shapes is a quality that parametric surfaces lack and thus one of the main motivations for using them. For more information about Boolean operations, click [here](../boolean_operations.md)

## Deferred Shading

Deferred rendering or deferred shading is based on the idea that we defer most heavy calculations (such as light calculations) to a later stage. We can achieve deferred shading with one geometry pass and one light pass. The geometry pass renders the scene once and stores distinct data about the displayed geometry in different textures, commonly known as the G-buffer. Position vectors, color vectors, normal vectors, and/or specular values make up the majority of this data. In the second pass, we render a full-screen quad and calculate the final render using the provided G-buffer. We only need to do our light calculations once when deferring them to a later stage because the G-buffer contains all of the data from the topmost fragment. If deferred rendering is a little fuzzy I highly recommend reading the following article: [Learn OpenGL: Deferred Shading](https://learnopengl.com/Advanced-Lighting/Deferred-Shading) from Joey De Vries.

# Strategy

Now that we are all caught up we can start creating a volume renderer. As stated a deferred rendering solution was used to integrate our volumetric renderer within a proper rendering pipeline. This means that the outcome of our volumetric renderer will be a G-Buffer. We created this G-Buffer by utilizing our sphere-tracer within the fragment shader of our render pass. Some pseudocode for creating this renderpass could look as followed:

```cpp:
    /*
        color attachment 00: stores position of the surface that was hit
        color attachment 01: stores normal of the surface that was hit
        color attachment_02: stores albedo (RGB) and specular (A) of the surface that was hit
    */
    color_attachments.add(create_color_attachment(Format::RGB_32_FLOAT));
    color_attachments.add(create_color_attachment(Format::RGB_32_FLOAT));
    color_attachments.add(create_color_attachment(Format::RGBA_32_FLOAT));

    FrameBuffer framebuffer = ResourceFactory::create_frame_buffer(color_attachments);

    RenderPassDescription renderpass_desc;

    /*
        We can add this frame buffer to our Pipeline
    */
    renderpass_desc.framebuffer = framebuffer;
    renderpass_desc.clear_color = { 0.0f, 0.0f, 0.0f, 1.0f };
    renderpass_desc.clear_flags = COLOR_ONLY;

    sbt::PipelineDescription pipeline_desc;

    pipeline_desc.layout = { {sbt::DataType::VEC3, "a_Position"}, {sbt::DataType::VEC2, "a_TexCoord"} };
    pipeline_desc.renderpass = ResourceFactory::create_render_pass(renderpass_desc);
    pipeline_desc.depth_test_state = { sbt::DepthTestEnabled::Type::NO };
    pipeline_desc.facecull_state = { sbt::FaceCullingEnabled::Type::NO };

    create_pipeline(pipeline_desc);
```

