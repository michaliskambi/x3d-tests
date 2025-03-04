Question 1:

The family name `my custom family name in parent` is defined
by `FontLibrary` in the `inline_parent.x3dv`.

It *referred to* by the `FontStyle` node in the
`inline_child.x3dv`.

Should it have an effect when rendering `inline_child.x3dv`?
That is, should `inline_child.x3dv` display "my custom family name in parent"
with `DejaVuSans-BoldOblique.ttf`?

Note that answering yes, while it may seem expected, means:

- it means that rendering of `inline_child.x3dv` when it is standalone, opened directly, is different. Maybe that's OK (one can argue it's similar to how `Fog` affects inlined content), but this needs to be clearly documented in spec.

- and if the answer is yes, what is the order when multiple `FontLibrary` nodes define the same family name?
