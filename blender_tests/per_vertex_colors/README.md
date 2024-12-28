# Test using per-vertex colors in Blender

`monkey_ambient_occlusion.blend` shows using per-vertex colors in Blender to multiply PBR and unlit materials. The per-vertex colors have been generated using "Bake" of "Ambient Occlusion" to "Color Attribute" (possible when Blender's renderer is set to "Cycles").

Desired goal: make Blender -> X3D exporter capable of producing `monkey_ambient_occlusion_desired.x3d` that looks exactly the same as in Blender.

Additional files:

- `monkey_ambient_occlusion.glb` shows output of Blender -> glTF exporter.

    It is mostly correct... except the _"Blue unlit, per-vertex colors"_ case (in Blender, blue is correctly multiplied with per-vertex colors; in glTF the blue is missing). This is most likely a bug in Blender -> glTF exporter.

- `monkey_ambient_occlusion_gltf_castle.png` is a screenshot from [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) showing the glTF file.

- `monkey_ambient_occlusion_gltf_sample_viewer.png` is a screenshot from [glTF Sample Viewer](https://github.khronos.org/glTF-Sample-Viewer-Release/) showing the glTF file.

    The _Castle Model Viewer_ and _glTF Sample Viewer_ show the same result, so the viewers are likely correct (and missing blue at "Blue unlit, per-vertex colors" case is a bug in Blender -> glTF exporter).