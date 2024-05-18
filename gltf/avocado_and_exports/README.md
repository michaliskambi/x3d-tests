Fun with avocado glTF model from [glTF-Sample-Assets (Models/Avocado/)](https://github.com/KhronosGroup/glTF-Sample-Assets).

The glTF model of avocado is 

- inlined in X3D file using `Inline`, but under `Switch`. So the glTF contents are not visible, but you can "cherry-pick" a subset of them and reuse from X3D.

- and then parts of the avocado are imported and used in X3D.

To know what is EXPORTed from Avocado, load it to [Castle Model Viewer](https://castle-engine.io/castle-model-viewer) and save as X3D. At the bottom, you will notice lines like this:

```
EXPORT Avocado_2
EXPORT CastleEncoded_2256_Avocado_d
EXPORT Avocado
```

See [Castle Game Engine documentation about glTF and X3D interoperability](https://castle-engine.io/gltf) for various details how this works.
