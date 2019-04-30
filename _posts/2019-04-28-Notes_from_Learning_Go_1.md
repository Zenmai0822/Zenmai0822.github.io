---
layout: post
title: "Notes from Learning Go, Part 1"
categories: go 
---

Here are my notes for learning `go`. I am following along with O'Reilly's [Head First Go](https://www.amazon.com/dp/1491969555/). They did a good job making the book, as well as making it looks like it's from 2006.

Relevant code is located at [`notes1.go`](https://github.com/Zenmai0822/goplayground/blob/master/notes1.go) and [`guessNum.go`](https://github.com/Zenmai0822/goplayground/blob/master/guessNum.go).

## Painful Things

- ~~There can't be two files in the same folder that has `package main` and `main()` declared in each file. Thus, my playground files are organized in one file with all the functions defined, and a `main()` that runs them all (for now).~~
  - Only packages that is designed to be executable should use `main` as package name, thus it is impossible to have two `main()` defined in two separate files. 

- The [Go Plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode.Go) of VSCode uses `goreturns` as its formatting tool as default, and it will remove any unused `import` statements upon save. 

## Types

- Integer is `int`, but float has to be `float32` or `float64`
  - Value of a rune (char literal?) is `int32`
  - Casting float to int drops decimal values
- `"Double quote"` is string, `'s'`ingle quote is a rune
- Use `:=` to declare and assign variable (`currentHour := 2`, `aloha := "hello"`), or use `var $varName ($type) = value`
  - If the complier can infer the type, it will warn you and the type declaration should be omitted
- Cast by `newType(varOfAnotherType)`
- String to int uses `strconv.Atoi(string)`
- String to float uses `strconv.ParseFloat(string, precision)`. The precision field can be either `64` or `32`

## Functions

```go
func camelCaseFunctionName(varName varType) returnType {
    // do things
}
```

- It will be camel case if you don't want to export it (i.e. letting others to use it). **C**apitalize all function names you want to export.

- Functions can return multiple things, and an equal number of variables are needed for assignment

`reader.ReadString()` returns `input` (the text user has keyed in) and `err` (if any, `nil` if not). By assigning `err` as `_` the *Variable Not Used* error can be avoided. However, the `err` will be no longer reachable. 

```go
func readInput() {
    fmt.Print("Input Prompt:\n>")
    reader := bufio.NewReader(os.Stdin)
    input, _ := reader.ReadString('\n') // returns input and error
    fmt.Println(input)
}

// What should be done 
func readInputWithLog() {
    fmt.Print("Input Prompt:\n>")
    reader := bufio.NewReader(os.Stdin)
    input, err := reader.ReadString('\n') // returns input and error
    if err != nil {
        log.Fatal(err)
    }
}
```

- `for` uses semicolons (`for i:=0; i < 10; i++ {}`) and no parenthesis
- It can be used as a while loop (`for true {}`)

## Scopes

- Variables declared in a block only works in that block
- `if` and `for` are considered blocks, including the init statement of `for`

## Random

- Set a seed first. Otherwise the random vales generated will be the same across runs. (My code and the sample on the book produced the same random int repeatedly)

```go
// Use current epoch time to set random seed
func printRandomInt() {
    epoch := time.Now().Unix()
    rand.Seed(epoch)
    fmt.Println(rand.Intn(100)) // 0 to 99 inclusive
}
```