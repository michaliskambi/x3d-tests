# Texture transformations in Blender, to test Blender -> X3D exporter

## Overview

Testing texture transformations in Blender, to make sure that X3D exporter exports them correctly. The most important Blender feature here is the ["Mapping"](https://docs.blender.org/manual/en/latest/render/shader_nodes/vector/mapping.html) node combined with _"UV Map"_. When used with 2D textures, it should result in X3D node `TextureTransform` with proper settings (sometimes it can be placed directly in `Appearance.textureTransform`, sometimes it has to be one of items within `MultiTextureTransform`).

Note that it is practically impossible to support all the features of Blender in X3D or glTF, we don't really hope to support everything -- just the most common node setups.

Tests, in the order that (seems to Michalis) to reflect the complexity of the problem:

- `texture_transforms_on_base_texture.blend`: various transformations of the _base_ texture.

    I suggest to start working (on the X3D exporter support for texture transforms) from making sure / fixing this testcase, as it's the simplest one.

- `castle_fps_game_level/level.blend`: Real-life example of using texture trasformations in a game level.

    This is a subset of the real model of the level used in [examples/fps_game/](https://github.com/castle-engine/castle-engine/tree/master/examples/fps_game) example from _Castle Game Engine_, we removed most of the level except 2 "floating terrains" and a bridge. We also removed (from the test here) a [special X3D effect we used there to blend 2 textures on a terrain together](https://github.com/castle-engine/castle-engine/blob/master/examples/fps_game/data/level/terrain_multi_texture.x3dv).

    After the cuts, the "feature" tested by this is actually really simple, it just uses Blender "Mapping" node to scale texture coordinates 200x for both base and normalmap textures. The usage just in a more "real-life" context.

- TODO: multiple_textures: multiple textures (base, normalmap, metallic/roughness) used and transformed.

Each Blender sample has an accompanying:

- TODO: glTF file produced by Blender's glTF exporter. [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) renders them OK, showing that Blender, glTF exporter and CGE work correctly together.

- TODO: A "desired" X3D file. This is is crafted by hand, in a text editor (search "Michalis" to find my modifications in X3D file done by hand). This is how it should look like in X3D, to achieve the same effect as we see in Blender and glTF. The goal is to make sure that our Blender->X3D exporter produces the equivalent X3D file as the "desired" one.

- TODO: Screenshots, from Blender (rendering and node setup) and _Castle Model Viewer_ (which should show equivalent rendering for glTF and "desired" X3D file).

We use a few textures to test this. See [textures/](textures/) subdirectory and the [texture authors](textures/AUTHORS.md).

## TODO: 3D textures

- Blender and X3D (unlike glTF) support also 3D textures. They can be defined e.g. in [KTX](https://castle-engine.io/ktx) or [DDS](https://castle-engine.io/dds) texture files.

    We should test capabilities of Blender to export 3D textures, and test transforming them too. This will also make all the settings of ["Mapping"](https://docs.blender.org/manual/en/latest/render/shader_nodes/vector/mapping.html) node useful.

## Note: What does Blender->glTF exporter support?

IMHO (Michalis) Blender->X3D exporter should support _at least_ similar Blender nodes setup as the Blender->glTF exporter. Because that's what I would expect as a user -- Khronos already did the hard work of consulting what artists want, and what is possible to export with reasonable effort. Ideally, X3D exporter can follow it, or support even more (see "3D textures" notes).

It's documented what Blender->glTF exporter supports, see [glTF exporter docs, section "UV Mapping"](https://docs.blender.org/manual/en/dev/addons/import_export/scene_gltf2.html#uv-mapping):

> Settings from the Mapping node are exported using a glTF extension named `KHR_texture_transform`. There is a mapping type selector across the top. _Point_ is the recommended type for export. _Texture_ and _Vector_ are also supported. The supported offsets are:
>
> - Location - X and Y
> - Rotation - Z only
> - Scale - X and Y

Note that glTF only supports 2D textures, so their choice of above settings makes sense (no way to support e.g. rotation in X or Y). The same applies to X3D when we use only `TextureTransform` node.

But in X3D, we could express full power of Blender's "Mapping" node by the `TextureTransform3D` node (from X3D _Texturing3D_ component).

See also glTF testcases (note: I didn't find Blender sources for them, I don't know if they were created in Blender at all):

- https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/TextureTransformTest

- https://github.com/KhronosGroup/glTF-Sample-Assets/tree/main/Models/TextureTransformMultiTest
