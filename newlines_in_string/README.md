This was done to check what happens if you put literal newlines in a single item on `Text.string`, like this in X3D classic:

```
# INVALID EXAMPLE
geometry Text {
  string "One line
Another line
Yet another line"
}
```

instead of doing it properly, like this:

```
# THIS IS VALID AND YOU SHOULD USE THIS.
geometry Text {
  string [
    "One line"
    "Another line
    "Yet another line"
  ]
}
```

As expected, the 1st approach fails,
- with CGE/view3dscene,
- with FreeWRL.

I.e. newline inside a string doesn't make a new line of text. Indeed, supporting it would be cumbersome in implementations (need to split strings on newlines in addition to having alreday a list of strings, FdString.Count no longer implies the number of lines).
