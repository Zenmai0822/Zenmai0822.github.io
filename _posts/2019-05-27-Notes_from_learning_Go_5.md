---
layout: post
title: "Notes from Learning Go, Part 5"
categories: go
---

Relevant code and its output is located at [`maps.go`](https://github.com/Zenmai0822/goplayground/blob/master/maps.go)

## Maps

- `map[keyType]valueType`, like `map[int]string`
  - Literal (constructing by hand): `map[int]string{12: "value1", 23: "value2"}`
  - Recommended to do one pair per line, final line extra comma is permitted
- Zero Value is `nil`. Cannot be accessed
- Use `myMap := make(map[keyType]valueType)` to init a map. At this point all keys not present will produce a 0 value of the `valueType` when accessing.
  - Add second boolean variable (typically `ok`) to check if key in map.
- Use `for k, v := range mymap {}` to loop through
- *Order not guaranteed, will be different each time*
  - Create a slice (or array) containing the keys, sort the slice / array, then for loop said slice / array to get a stable order of keys.