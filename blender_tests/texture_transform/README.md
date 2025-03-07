# Texture transformations in Blender, to implement in Blender -> X3D exporter

## Overview

Tests of texture transformations in Blender, to make sure that X3D exporter exports them correctly.

- The most important Blender feature here is the ["Mapping"](https://docs.blender.org/manual/en/latest/render/shader_nodes/vector/mapping.html) node combined with _"UV Map"_.

- Usually (when used to make typical transformations meaningful for 2D texture coordinates), it should result in X3D node `TextureTransform` with proper settings. Usually `TextureTransform` can be placed directly in `Appearance.textureTransform`, though in some hard cases `TextureTransform` has to be one of items within `MultiTextureTransform`, and then `MultiTextureTransform` goes to `Appearance.textureTransform`.

- In complex cases, we may need to use `TextureTransform3D` node from X3D _Texturing3D_ component. This is capable of expressing all the settings of Blender's "Mapping" node. It makes most sense for 3D textures (more on them in later section).

Note that it is practically impossible to support all the features of Blender nodes in X3D (or glTF, for that matter). So we don't really hope to support everything -- just the most common node setups. I tried to demonstrate them here.

## Testcases

Tests, in the order that (seems to Michalis) to reflect the complexity of the problem:

### Transform the "base" texture in various ways (scale, rotate, translate)

`texture_transforms_on_base_texture.blend`: various transformations of the _base_ texture.

I suggest to start working (on the X3D exporter support for texture transforms) from this testcase, as it's the simplest one.

TODO: The rendering of the corresponding glTF version (in `texture_transforms_on_base_texture.glb`) by [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) is wrong at _"Texture rotate"_ test. Is this a bug of [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) , or Blender -> glTF exporter? Unknown. Strangely, https://gltf-viewer.donmccurdy.com/ and https://github.khronos.org/glTF-Sample-Viewer-Release/ fail to open textures inside, so we cannot compare. This is a TODO for Michalis to investigate. Don't let this stop Blender -> X3D development: naturally, for Blender -> X3D exporter, the "target" is what Blender displays, and it doesn't really matter what glTF / Castle Model Viewer do.

### Real-life example of using texture trasformations in a game level

`castle_fps_game_level/level.blend`: Real-life example of using texture trasformations in a game level.

This is a subset of the real model of the level used in [examples/fps_game/](https://github.com/castle-engine/castle-engine/tree/master/examples/fps_game) example from _Castle Game Engine_, we removed most of the level except 2 "floating terrains" and a bridge. We also removed (from the test here) a [special X3D effect we used there to blend 2 textures on a terrain together](https://github.com/castle-engine/castle-engine/blob/master/examples/fps_game/data/level/terrain_multi_texture.x3dv).

After the cuts, the "feature" tested by this is actually really simple, it just uses Blender "Mapping" node to scale texture coordinates 200x for both base and normalmap textures.

So this testcase is still rather simple. The usage presented here is just in a more "real-life" context than `texture_transforms_on_base_texture.blend`.

### Transform base, normal, metallic and roughness textures in same or different ways

`multiple_textures.blend`: multiple textures (base, normalmap, metallic/roughness) used and transformed.

Shows transforming all textures  (base, normalmap, metallic/roughness) in the same way.

Shows what happens when some textures are transformed differently than others. (This will require `MultiTextureTransform` in X3D and managing which transform affects what with `mapping`).

This is a more complex testcase. Especially the case when we have different transformations for different textures is hard. It's also a really rare use-case. So support for this is absolutely optional.

TODO: [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) rendering of this is not good, so I don't attach it here. One bug is wrong rotations (see also above). Another issue is inability to account for different transformations of different texture slots (this is known, simply not implemented in _Castle Game Engine_ glTF -> X3D conversion, and makes warnings _"Textures within material have different texture transformation, not supported now"_). I am also not 100% sure whether the generated glTF is OK, [glTF Sample Viewer](https://github.khronos.org/glTF-Sample-Viewer-Release/) reports problems with it.

## Files accompanying each testcase

Each Blender sample has an accompanying:

- glTF file produced by Blender's glTF exporter, in `xxx.glb` file.

    [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) renders them OK (except TODOs mentioned above...), showing that Blender, glTF exporter and CGE work correctly together. They are exported to self-contained `.glb` files, so you can view them in any glTF viewer, including online https://gltf-viewer.donmccurdy.com/ and https://github.khronos.org/glTF-Sample-Viewer-Release/ (except they don't load some glTF files, see above...).

- TODO: A "desired" X3D file in `xxx_desired.x3d`.

    This is is crafted by hand, in a text editor (search "Michalis" to find my modifications in X3D file done by hand). This is how it should look like in X3D, to achieve the same effect as we see in Blender and glTF. The goal is to make sure that our Blender->X3D exporter produces the equivalent X3D file as the "desired" one.

- Screenshot from Blender (rendering and node setup) in `xxx_blender.png`.

- Screenshot from [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) showing the glTF file, in `xxx_gltf_castle.png`. This should be equivalent to the "desired" X3D file rendering.

We use a few textures to test this. See [textures/](textures/) subdirectory and the [texture authors](textures/AUTHORS.md).

## TODO: 3D textures

Blender and X3D (unlike glTF) support also 3D textures. They can be defined e.g. in [KTX](https://castle-engine.io/ktx) or [DDS](https://castle-engine.io/dds) texture files.

We should test capabilities of Blender to export 3D textures, and test transforming them too. This will also make all the settings of ["Mapping"](https://docs.blender.org/manual/en/latest/render/shader_nodes/vector/mapping.html) node useful.

## What does Blender->glTF exporter support?

IMHO Blender->X3D exporter should support _at least_ similar Blender nodes setup as the Blender->glTF exporter. Because that's what I would expect as a user -- Khronos already did the hard work of consulting what artists want, and what is possible to export with reasonable effort (for glTF, but it should be mostly similar for X3D). Ideally, X3D exporter can follow it, or support even more (see "3D textures" notes).

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

## Copyright and license

Made by [Michalis Kamburelis](https://michalis.xyz/). License: public domain.

Except:

- Note that some textures have different authors and licenses: [textures/AUTHORS.md](textures/AUTHORS.md).

- `castle_fps_game_level/` stuff follows the [CGE license](https://castle-engine.io/license) for examples, which is permissive "modified BSD (3-clause)".
