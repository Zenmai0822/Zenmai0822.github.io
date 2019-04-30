---
layout: post
title: "Notes from Learning Go, Part 2"
categories: go
---

Relevant code is located at [`sqrt.go`](https://github.com/Zenmai0822/goplayground/blob/master/sqrt.go)

## Structure

Apparently O'Reilly's book did not teach project structure early on (Nothing as of Location 1707[^1]). A go project should vaguely look something like this in terms of folder structure. The following path tree is the `$GOPATH`. You might not have it defined in your environment variables, and a default of `~/go` is used in this case. However, 

[^1]: **Update (2019-04-29 22:52):** It's in Chapter 4. Location 2015 if you are using Kindle.

```
.
├── bin
├── pkg
└── src
    └── github.com
        └── Zenmai0822
                └── goplayground
                    ├── guessNum.go
                    ├── notes1.go
                    └── sqrt.go
```

- Import is done by `import "github.com/Zenmai0822/goplayground"`, local `$GOPATH/src` will be checked before actual code from `github.com/Zenmai0822/goplayground` is being accessed
- The `goplayground` package is not designed to be executable, my previous playground became the test field for the code I write in the structure above. 
- Dash in package name will make compiler unhappy

### Package Names

- Use `main` if the package is designed to be executable
  - A `main()` is thus needed
- Use any other package name if the package is a library of functions
- File names does not matter to outside sources

### Comments / Documentation

- All exported functions (capitalized) should have a comment directly above them explaining what's the function's job
  - Should be full sentences, does not have to be 1 sentence per line.
- Begins with "$FuncName ..."
- `package` declaration is the same, it's ok to just do one file

```go
// Package goplayground is a playground for trying out the Go language
package goplayground

...

// Sqrt calculates the square root of the given float.
// It will return 0 and error if given value is <0.
func Sqrt(f float64) (float64, error) {
	if f < 0.0 {
		// Error message should not end in a punctuation
		return 0, fmt.Errorf("Input Val is < 0")
	}
	return math.Sqrt(f), nil
}

func useSqrt() {
	//root, err := sqrt(-3.0)
	root, err := Sqrt(3.0)
	if err != nil {
		log.Fatal(err)
	} else {
		fmt.Printf("%.5f\n", root)
	}
}
```

## Errors

See code example above.

- Type `error` is built in
- `fmt.Errorf()` does not want punctuation or `\n` proceeding the error message
- Has to return `nil` if no error, and `error` if error