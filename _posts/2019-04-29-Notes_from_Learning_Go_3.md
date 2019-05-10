---
layout: post
title: "Notes from Learning Go, Part 3"
categories: go
---

Relevant code is located at [`pointers.go`](https://github.com/Zenmai0822/goplayground/blob/master/pointers.go)

## Pointers

\*Python user sweats furiously\*

- Go is a **pass-by-value** language. It does not mutate variables.
- Pointers are memory addresses, use them to reference variables across scopes

### Syntax of Pointers

- `&varName` is the pointer of variable `varName`, the pointer has a type shown as `*varType`
- To access the value the pointer is pointing to, use `*pointer`.

### Pointer Scope

- Pointers work across scopes -- variables defined in one function can get its value mutated by passing the pointer of that variable to other functions. Those other mutating functions can take the pointer and mutate the value of the pointer.
- Pointers can be returned by a function, or not.

```go
// mutatePointers mutates a pointer of int using two manipulation functions.
// One mutates the pointer in place, and one returns the modified pointer
func mutatePointers() {
	three := 3
	pointerOfThree := &three
	fmt.Printf("Value of variable 'three': %d\n", three)
	plusOnePointer(pointerOfThree)
	fmt.Printf("Value of variable 'three', after mutation by pointer: %d\n", three)
	mutatedPointer := plusOneThenReturnPointer(pointerOfThree)
	fmt.Printf("Value of variable 'three', after mutation again using plusOneThenReturnPointer(): %d\n", *mutatedPointer)
}

// plusOnePointer mutates the incoming pointer and returns nothing
func plusOnePointer(numPointer *int) {
	*numPointer++
}

// plusOneThenReturnPointer mutates and then changes the incoming pointer
func plusOneThenReturnPointer(numPointer *int) *int {
	*numPointer++
	return numPointer
}

```

## Warnings

I am using `go vet` through the VSCode Go plugin. It will warn you in the following cases:

### `Printf` does not have a format string

```go
fmt.Printf("Value of variable 'three', after mutation by pointer: $d\n", three)
```
yields:

```bash
$ go vet pointers.go
# command-line-arguments
./pointers.go:40:12: Printf call has arguments but no formatting directives
```

and at runtime:

```bash
Value of variable 'three', after mutation by pointer: $d
%!(EXTRA int=4)%
```