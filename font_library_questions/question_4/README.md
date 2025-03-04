Question 4:

The family name `my custom family name` is defined
by `FontLibrary` in the `inline_child.x3dv`.

It *referred to* by the `FontStyle` node in the
`inline_parent.x3dv`.

Should it have an effect when rendering "Hello World from inline parent!"?

I think *no*, following how `Fog` affects inlined content. `Fog` in the parent affects the inlined content, but `Fog` in child does not affect parent. But then, `Fog` is a "bindable node" so it has its own mechanism altogether, it's not that simple analogy.