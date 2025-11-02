# GitHub Issues for wgpu Graphics Framework

This document contains all the issues to create for progressive development of the realtime graphics framework.

---

## Phase 1 - Essential Components

### Issue 1: Implement Vertex trait and VertexBuffer abstraction

**Labels:** `enhancement`, `phase-1`, `geometry`

**Description:**
Create a flexible vertex system that supports common vertex attributes for realtime graphics.

**Components:**
- `Vertex` trait with required attributes (position, normal, UV, color)
- `VertexBuffer` wrapper for managing vertex data with wgpu
- Helper methods for vertex buffer creation from raw data
- Proper memory layout with `bytemuck` derives

**Acceptance Criteria:**
- [ ] `Vertex` trait defined with standard attributes
- [ ] `VertexBuffer` struct wraps wgpu buffer creation
- [ ] Support for interleaved vertex data
- [ ] Example usage in documentation
- [ ] Unit tests for buffer creation

**Dependencies:**
- Uses `bytemuck` for safe casting

**Files:**
- `src/geometry/vertex.rs`
- `src/geometry/mod.rs`

---

### Issue 2: Implement Mesh abstraction with IndexBuffer support

**Labels:** `enhancement`, `phase-1`, `geometry`

**Description:**
Create a `Mesh` struct that combines vertex and index buffers for efficient rendering.

**Components:**
- `IndexBuffer` wrapper for index data
- `Mesh` struct combining vertex + index buffers
- Support for both indexed and non-indexed rendering
- Mesh validation (bounds checking, etc.)

**Acceptance Criteria:**
- [ ] `IndexBuffer` struct created
- [ ] `Mesh` struct combines vertex and index buffers
- [ ] Support for 16-bit and 32-bit indices
- [ ] Draw method abstractions
- [ ] Unit tests

**Dependencies:**
- Issue #1 (Vertex system)

**Files:**
- `src/geometry/mesh.rs`
- `src/geometry/mod.rs`

---

### Issue 3: Add primitive geometry generators

**Labels:** `enhancement`, `phase-1`, `geometry`

**Description:**
Provide helper functions to generate common geometric primitives for testing and prototyping.

**Components:**
- Cube/Box generator
- Sphere generator (UV sphere or icosphere)
- Plane/Quad generator
- Triangle generator
- Configurable detail levels

**Acceptance Criteria:**
- [ ] `create_cube()` function
- [ ] `create_sphere()` function with subdivision parameter
- [ ] `create_plane()` function
- [ ] `create_quad()` function (screen-space helper)
- [ ] Proper normals and UVs generated
- [ ] Visual tests/examples

**Dependencies:**
- Issue #2 (Mesh system)

**Files:**
- `src/geometry/primitives.rs`

---

### Issue 4: Implement Shader loading and management

**Labels:** `enhancement`, `phase-1`, `graphics`

**Description:**
Create abstractions for loading and managing WGSL shaders with error handling.

**Components:**
- `Shader` struct wrapping `ShaderModule`
- File-based shader loading
- Error reporting for shader compilation
- Shader source caching

**Acceptance Criteria:**
- [ ] `Shader` struct created
- [ ] Load shaders from files
- [ ] Load shaders from string/embedded sources
- [ ] Proper error handling and reporting
- [ ] Examples of usage

**Dependencies:**
None

**Files:**
- `src/graphics/shader.rs`
- `src/graphics/mod.rs`
- `shaders/` directory

---

### Issue 5: Implement RenderPipeline abstraction

**Labels:** `enhancement`, `phase-1`, `graphics`

**Description:**
Create a builder-pattern abstraction for wgpu render pipelines to reduce boilerplate.

**Components:**
- `PipelineBuilder` for fluent pipeline creation
- Support for common configurations (blend modes, culling, etc.)
- Pipeline caching by configuration
- Vertex layout integration with Vertex trait

**Acceptance Criteria:**
- [ ] `PipelineBuilder` with fluent API
- [ ] Common presets (opaque, transparent, wireframe)
- [ ] Automatic vertex layout from Vertex trait
- [ ] Pipeline caching
- [ ] Documentation with examples

**Dependencies:**
- Issue #1 (Vertex trait)
- Issue #4 (Shader system)

**Files:**
- `src/graphics/pipeline.rs`

---

### Issue 6: Implement Transform and model matrix system

**Labels:** `enhancement`, `phase-1`, `math`

**Description:**
Create a Transform struct for managing object position, rotation, and scale.

**Components:**
- `Transform` struct with position, rotation (quaternion), scale
- Model matrix computation
- Transform hierarchy support (parent/child)
- Helper methods (look_at, translate, rotate, scale)

**Acceptance Criteria:**
- [ ] `Transform` struct with all fields
- [ ] `model_matrix()` method returns Mat4
- [ ] Transform composition (parent * child)
- [ ] Helper constructors and methods
- [ ] Unit tests for matrix correctness

**Dependencies:**
- Add `glam` crate for math

**Files:**
- `src/math/transform.rs`
- `src/math/mod.rs`
- Update `Cargo.toml` with glam dependency

---

### Issue 7: Implement PerspectiveCamera

**Labels:** `enhancement`, `phase-1`, `camera`

**Description:**
Create a perspective camera with configurable FOV, aspect ratio, and near/far planes.

**Components:**
- `PerspectiveCamera` struct
- View matrix computation
- Projection matrix computation
- Camera configuration (FOV, aspect, near, far)

**Acceptance Criteria:**
- [ ] `PerspectiveCamera` struct created
- [ ] `view_matrix()` method
- [ ] `projection_matrix()` method
- [ ] `view_projection_matrix()` convenience method
- [ ] Proper aspect ratio handling
- [ ] Unit tests

**Dependencies:**
- Issue #6 (Transform system) - cameras use transforms

**Files:**
- `src/camera/perspective.rs`
- `src/camera/mod.rs`

---

### Issue 8: Implement OrthographicCamera

**Labels:** `enhancement`, `phase-1`, `camera`

**Description:**
Create an orthographic camera for 2D rendering and UI.

**Components:**
- `OrthographicCamera` struct
- Configurable view volume (left, right, top, bottom, near, far)
- Projection matrix computation

**Acceptance Criteria:**
- [ ] `OrthographicCamera` struct created
- [ ] `projection_matrix()` method
- [ ] Helper for screen-space 2D projection
- [ ] Unit tests

**Dependencies:**
- Issue #6 (Transform system)

**Files:**
- `src/camera/orthographic.rs`
- `src/camera/mod.rs`

---

### Issue 9: Add depth buffer support to State

**Labels:** `enhancement`, `phase-1`, `rendering`

**Description:**
Add automatic depth buffer creation and management to the State struct.

**Components:**
- Depth texture creation on init and resize
- Depth format selection (prefer Depth24Plus)
- Integration with render pass
- Depth testing configuration

**Acceptance Criteria:**
- [ ] Depth texture created in `State::new()`
- [ ] Depth texture resized in `State::resize()`
- [ ] Depth attachment added to render pass
- [ ] Z-testing enabled by default
- [ ] Visual test showing depth working

**Dependencies:**
None - modifies existing State

**Files:**
- `src/app.rs` (formerly lib.rs)

---

### Issue 10: Implement UniformBuffer abstraction

**Labels:** `enhancement`, `phase-1`, `graphics`

**Description:**
Create a type-safe uniform buffer abstraction for passing data to shaders.

**Components:**
- `UniformBuffer<T>` generic struct
- Automatic buffer creation and updates
- Bind group integration
- Support for dynamic updates per frame

**Acceptance Criteria:**
- [ ] `UniformBuffer<T>` struct with generic type
- [ ] `update()` method for writing new data
- [ ] Bind group helper methods
- [ ] Example with camera matrices
- [ ] Unit tests

**Dependencies:**
- Requires `bytemuck` for safe casting

**Files:**
- `src/graphics/uniform.rs`

---

### Issue 11: Implement basic Material system

**Labels:** `enhancement`, `phase-1`, `graphics`

**Description:**
Create a Material abstraction that combines shaders, uniforms, and bind groups.

**Components:**
- `Material` struct with shader reference
- Bind group management
- Uniform data storage
- Material builder pattern

**Acceptance Criteria:**
- [ ] `Material` struct created
- [ ] Integrates with shader system
- [ ] Manages bind groups automatically
- [ ] Support for uniforms
- [ ] Example material creation

**Dependencies:**
- Issue #4 (Shader system)
- Issue #10 (Uniform buffers)

**Files:**
- `src/graphics/material.rs`

---

## Phase 2 - Enhanced Features

### Issue 12: Implement Texture loading from files

**Labels:** `enhancement`, `phase-2`, `texture`

**Description:**
Add support for loading textures from PNG/JPG files.

**Components:**
- `Texture` struct wrapping wgpu texture
- Load from file using `image` crate
- Mipmap generation
- Common texture formats support

**Acceptance Criteria:**
- [ ] `Texture::from_file()` method
- [ ] Support PNG and JPG formats
- [ ] Proper format conversion
- [ ] Mipmap generation option
- [ ] Example loading and displaying texture

**Dependencies:**
- Add `image` crate to Cargo.toml
- Add `anyhow` for error handling

**Files:**
- `src/graphics/texture.rs`
- `src/graphics/mod.rs`

---

### Issue 13: Implement Sampler presets

**Labels:** `enhancement`, `phase-2`, `texture`

**Description:**
Provide common sampler configurations for texture filtering.

**Components:**
- Sampler preset functions
- Linear/Nearest filtering
- Mipmap modes
- Wrap modes (repeat, clamp, mirror)

**Acceptance Criteria:**
- [ ] `Sampler::linear()` preset
- [ ] `Sampler::nearest()` preset
- [ ] `Sampler::linear_clamp()` preset
- [ ] Custom sampler builder
- [ ] Documentation

**Dependencies:**
- Issue #12 (Texture system)

**Files:**
- `src/graphics/texture.rs`

---

### Issue 14: Implement comprehensive InputState

**Labels:** `enhancement`, `phase-2`, `input`

**Description:**
Create a robust input state tracker for keyboard and mouse.

**Components:**
- Key state tracking (pressed, just_pressed, just_released)
- Mouse button state
- Mouse position and delta tracking
- Update system for clearing transient states

**Acceptance Criteria:**
- [ ] `InputState` struct
- [ ] `is_key_pressed()`, `is_key_just_pressed()`, `is_key_just_released()`
- [ ] Mouse button methods
- [ ] Mouse position and delta
- [ ] `update()` method called per frame
- [ ] Integration example

**Dependencies:**
None

**Files:**
- `src/input/state.rs`
- `src/input/mod.rs`

---

### Issue 15: Implement Time and delta time tracking

**Labels:** `enhancement`, `phase-2`, `infrastructure`

**Description:**
Add time management for frame-rate independent updates.

**Components:**
- `Time` struct with delta time
- FPS calculation
- Total elapsed time
- Frame counting

**Acceptance Criteria:**
- [ ] `Time` struct created
- [ ] `delta_time()` method (seconds as f32)
- [ ] `fps()` method with smoothing
- [ ] `elapsed()` method for total time
- [ ] Integration into State update loop

**Dependencies:**
None

**Files:**
- `src/time/mod.rs`

---

### Issue 16: Implement FPS camera controller

**Labels:** `enhancement`, `phase-2`, `camera`

**Description:**
Create a first-person camera controller with keyboard and mouse input.

**Components:**
- `FpsCameraController` struct
- WASD movement
- Mouse look controls
- Configurable speed and sensitivity

**Acceptance Criteria:**
- [ ] `FpsCameraController` created
- [ ] Updates camera transform from input
- [ ] Configurable movement speed
- [ ] Mouse sensitivity control
- [ ] Example demo

**Dependencies:**
- Issue #7 (PerspectiveCamera)
- Issue #14 (InputState)
- Issue #15 (Time for delta time)

**Files:**
- `src/camera/controller.rs`

---

### Issue 17: Implement orbit camera controller

**Labels:** `enhancement`, `phase-2`, `camera`

**Description:**
Create an orbit/arcball camera controller for object inspection.

**Components:**
- `OrbitCameraController` struct
- Mouse drag to orbit
- Scroll to zoom
- Pan with middle mouse button
- Focus point configuration

**Acceptance Criteria:**
- [ ] `OrbitCameraController` created
- [ ] Orbit around target point
- [ ] Zoom in/out
- [ ] Pan support
- [ ] Example demo

**Dependencies:**
- Issue #7 (PerspectiveCamera)
- Issue #14 (InputState)

**Files:**
- `src/camera/controller.rs`

---

### Issue 18: Implement multiple render pipeline support

**Labels:** `enhancement`, `phase-2`, `rendering`

**Description:**
Allow State to manage multiple render pipelines for different rendering techniques.

**Components:**
- Pipeline registry/cache
- Pipeline switching during render
- Common pipelines (wireframe, solid, transparent)

**Acceptance Criteria:**
- [ ] Pipeline storage in State
- [ ] Add/get pipeline methods
- [ ] Example using multiple pipelines
- [ ] Performance profiling

**Dependencies:**
- Issue #5 (Pipeline abstraction)

**Files:**
- `src/app.rs`

---

### Issue 19: Add render statistics and profiling

**Labels:** `enhancement`, `phase-2`, `infrastructure`

**Description:**
Track rendering statistics for performance monitoring.

**Components:**
- Draw call counter
- Triangle counter
- GPU time estimation
- Stats display/logging

**Acceptance Criteria:**
- [ ] `RenderStats` struct
- [ ] Track draw calls per frame
- [ ] Track primitives rendered
- [ ] Optional on-screen display
- [ ] Reset per frame

**Dependencies:**
- Issue #15 (Time system)

**Files:**
- `src/render/stats.rs`

---

## Phase 3 - Polish & Advanced Features

### Issue 20: Implement basic Scene management

**Labels:** `enhancement`, `phase-3`, `scene`

**Description:**
Create a simple scene graph for managing multiple renderable objects.

**Components:**
- `Renderable` trait
- `Scene` struct with object list
- Batch rendering support
- Simple culling (optional)

**Acceptance Criteria:**
- [ ] `Renderable` trait defined
- [ ] `Scene` struct to hold renderables
- [ ] `Scene::render()` method
- [ ] Add/remove objects
- [ ] Example scene with multiple objects

**Dependencies:**
- Issue #2 (Mesh)
- Issue #11 (Material)

**Files:**
- `src/scene/renderable.rs`
- `src/scene/scene.rs`
- `src/scene/mod.rs`

---

### Issue 21: Implement instanced rendering

**Labels:** `enhancement`, `phase-3`, `rendering`

**Description:**
Add support for efficient instanced rendering of repeated geometry.

**Components:**
- `InstanceBuffer` abstraction
- Per-instance data (transform, color, etc.)
- Modified shaders for instancing
- Example rendering many objects

**Acceptance Criteria:**
- [ ] `InstanceBuffer` struct
- [ ] Instance vertex attributes
- [ ] Modified pipeline for instancing
- [ ] Example: render 1000+ cubes
- [ ] Performance comparison vs individual draws

**Dependencies:**
- Issue #2 (Mesh)
- Issue #5 (Pipeline)

**Files:**
- `src/graphics/instance.rs`

---

### Issue 22: Implement render-to-texture support

**Labels:** `enhancement`, `phase-3`, `rendering`

**Description:**
Add ability to render to textures for post-processing and effects.

**Components:**
- `RenderTarget` abstraction
- Texture attachment creation
- Render pass to texture
- Copy texture to screen

**Acceptance Criteria:**
- [ ] `RenderTarget` struct
- [ ] Create render target with specified size
- [ ] Render scene to target
- [ ] Display target on screen
- [ ] Example demo

**Dependencies:**
- Issue #12 (Texture system)

**Files:**
- `src/graphics/render_target.rs`

---

### Issue 23: Implement post-processing framework

**Labels:** `enhancement`, `phase-3`, `rendering`

**Description:**
Create a framework for chaining post-process effects.

**Components:**
- `PostProcess` trait for effects
- Effect chain/stack
- Built-in effects (grayscale, blur, etc.)
- Ping-pong buffer management

**Acceptance Criteria:**
- [ ] `PostProcess` trait
- [ ] `EffectChain` for multiple effects
- [ ] Example: grayscale effect
- [ ] Example: gaussian blur
- [ ] Chained effects demo

**Dependencies:**
- Issue #22 (Render-to-texture)

**Files:**
- `src/graphics/post_process.rs`
- `shaders/post/` directory

---

### Issue 24: Add PBR material support

**Labels:** `enhancement`, `phase-3`, `graphics`

**Description:**
Implement physically-based rendering materials with metallic-roughness workflow.

**Components:**
- PBR shader (WGSL)
- Albedo, metallic, roughness, AO textures
- Image-based lighting setup
- Material builder for PBR

**Acceptance Criteria:**
- [ ] PBR shader implementation
- [ ] Support for texture maps
- [ ] Basic lighting model
- [ ] Example PBR materials
- [ ] Visual comparison with basic materials

**Dependencies:**
- Issue #11 (Material system)
- Issue #12 (Texture loading)

**Files:**
- `src/graphics/pbr.rs`
- `shaders/pbr.wgsl`

---

### Issue 25: Implement simple lighting system

**Labels:** `enhancement`, `phase-3`, `lighting`

**Description:**
Add basic lighting with directional and point lights.

**Components:**
- `Light` trait
- `DirectionalLight` implementation
- `PointLight` implementation
- Light uniform buffers
- Blinn-Phong shading

**Acceptance Criteria:**
- [ ] Light traits and structs
- [ ] Shader integration
- [ ] Multiple light support
- [ ] Example scene with lights
- [ ] Specular highlights

**Dependencies:**
- Issue #10 (Uniform buffers)

**Files:**
- `src/scene/light.rs`
- `shaders/lit.wgsl`

---

## Examples & Documentation

### Issue 26: Create "spinning cube" example

**Labels:** `example`, `documentation`

**Description:**
Classic first example showing a textured, lit cube rotating in 3D space.

**Acceptance Criteria:**
- [ ] `examples/cube.rs` created
- [ ] Uses camera, mesh, material systems
- [ ] Cube rotates smoothly
- [ ] Can run with `cargo run --example cube`
- [ ] README section for example

**Dependencies:**
- Issue #3 (Primitives)
- Issue #7 (Camera)
- Issue #11 (Material)

**Files:**
- `examples/cube.rs`

---

### Issue 27: Create "textured mesh" example

**Labels:** `example`, `documentation`

**Description:**
Demonstrate loading and displaying a textured 3D mesh.

**Acceptance Criteria:**
- [ ] `examples/textured.rs` created
- [ ] Load texture from file
- [ ] Apply to mesh
- [ ] Camera controls
- [ ] README documentation

**Dependencies:**
- Issue #12 (Texture loading)
- Issue #16 or #17 (Camera controller)

**Files:**
- `examples/textured.rs`

---

### Issue 28: Create "multiple objects with lighting" example

**Labels:** `example`, `documentation`

**Description:**
Show scene with multiple objects, camera controls, and dynamic lighting.

**Acceptance Criteria:**
- [ ] `examples/scene.rs` created
- [ ] Multiple objects with different materials
- [ ] Interactive camera
- [ ] Moving/animated light
- [ ] FPS display

**Dependencies:**
- Issue #20 (Scene management)
- Issue #25 (Lighting)

**Files:**
- `examples/scene.rs`

---

### Issue 29: Write comprehensive API documentation

**Labels:** `documentation`

**Description:**
Add doc comments to all public APIs with examples.

**Acceptance Criteria:**
- [ ] All public types documented
- [ ] All public methods documented
- [ ] Code examples in docs
- [ ] `cargo doc --open` generates clean docs
- [ ] README links to docs

**Dependencies:**
All implementation issues

**Files:**
All source files

---

### Issue 30: Create getting started tutorial

**Labels:** `documentation`

**Description:**
Write a step-by-step tutorial for creating a first application.

**Components:**
- Tutorial document
- Step-by-step code progression
- Explanation of concepts
- Troubleshooting section

**Acceptance Criteria:**
- [ ] `TUTORIAL.md` created
- [ ] Covers basic setup to rendering a triangle
- [ ] Explains camera and transform usage
- [ ] Includes common pitfalls
- [ ] Linked from README

**Dependencies:**
- Multiple implementation issues

**Files:**
- `TUTORIAL.md`

---

## Summary

**Phase 1 (Essential):** Issues #1-11 (Geometry, shaders, camera, transforms, materials)
**Phase 2 (Enhanced):** Issues #12-19 (Textures, input, time, controllers, multi-pipeline)
**Phase 3 (Polish):** Issues #20-25 (Scene, instancing, post-processing, PBR, lighting)
**Examples:** Issues #26-28
**Documentation:** Issues #29-30

Total: **30 issues** for progressive development.

## Recommended Order

1. Start with Phase 1 in order (#1-11)
2. Create basic examples as Phase 1 completes
3. Move to Phase 2 based on needs
4. Phase 3 can be done in any order
5. Documentation throughout
