# CSCI 522 Assignment 3 — Bounding Volumes & Frustum Culling
# Demo Video
https://youtu.be/wTbKF90_vTw
# What’s Implemented
# 1) AABB generation (load time, once per mesh)

- Where: `MeshCPU::ReadMesh()` and `MeshManager::getAsset()`.

- What:

  - Traverse the position buffer to find per-axis min/max and build a local-space AABB.

  - Store both:

    - `Vector3 m_AABBPoints[8]` (8 corners; easy world transform & testing).

    - `Matrix4x4 m_AABB[4]` (four “corner + basis vectors” helpers for wire drawing).

  - Enable culling per mesh: m_performBoundingVolumeCulling = true;

# 2) Frustum plane construction (every frame)

- Where: `CameraSceneNode::do_CALCULATE_TRANSFORMATIONS()`.

- What:

  - Build view & projection; extract 6 frustum planes (L/R/T/B/N/F) into `m_frustumPlanes[6]`.

  - Use a slightly smaller FOV (`verticalFov2`) to form the culling projection so meshes disappear earlier than the on-screen FOV—easy to see culling correctness.

# 3) Runtime culling & debug rendering

- Where: `SingleHandler_Draw::do_GATHER_DRAWCALLS()` in `SH_DRAW.cpp`.

- What:

  - For each `MeshInstance`, get its parent `SceneNode` world transform.

  - Transform `m_AABBPoints[8]` to world space; test against the 6 planes; set `m_culledOut`; update `m_numVisibleInstances`.

  - For visible instances, call `DebugRenderer::createAABBLineMesh(...)` to draw a green AABB.
