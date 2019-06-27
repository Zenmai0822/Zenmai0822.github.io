---
layout: post
title: "Notes from Learning Go, Part 4"
categories: go
---

Relevant code is located at [`threeSum.go`](https://github.com/Zenmai0822/goplayground/blob/master/threeSum.go)

## Package Naming Conventions

- All lowercase, 
- Abbreviations if obvious

## Constants

- Declared using `const $Name [$Type] = $Value`, must have a value assigned
- No `:=`

## Make Executables

```bash
go install path/to/src
```

### Make Executables Smaller

```bash
# removes debug information for linker
go build --ldflags="-s -w"
# compress further using upx, install upx first
upx ./[BIN_NAME]
```

## Make Documentation

```bash
$ go doc goplayground
package goplayground // import "github.com/Zenmai0822/goplayground"

Package goplayground is a playground for trying out the Go language

func FibOne(n int) int
func FibTwo(n int) int
func Guess()
func MainPartThree()
func Sqrt(f float64) (float64, error) 
```

## Hosting HTML Documentation by Yourself

```bash
godoc -http=:6060 #Port Number
```

Then go to `localhost:6060` or the port number assigned to view the docs.

## Arrays and Slices

- No type mixing
- Array has fixed sizes (zero-value of type if no value assigned upon definition)
  - Literal Representation: `[3]int{0,1,2}`, `[5]string{"h","e","l","l","o"}`
- Slice does not have fixed sizes, use `s = append(s, newVal)` to append
  - Empty slices are `nil`
  - Literal Representation: `[]int{0,1,2}`, `[]string{"h","e","l","l","o"}`
  - Indexing arrays (`arr[1:3]`) creates slices, **referencing that array**
    - i.e. If slice is changed then array is modified, vice versa.

We can finally do some [leetcode](https://leetcode.com/problems/3sum/)...

```go
package goplayground

import (
	"sort"
)

// ThreeSum is a function that returns a slice of integer slices of 3 nums that adds to 0.
// This is a direct translation of my python code so that I can get my hands dirty.
// Lec 0015
func ThreeSum(nums []int) [][]int {
	var res [][]int
	sort.Ints(nums)
	for i, n := range nums {
		if i > 0 && nums[i-1] == nums[i] {
			continue
		}
		li := i + 1
		ri := len(nums) - 1
		for li < ri {
			sum := n + nums[li] + nums[ri]
			if sum == 0 {
				res = append(res, []int{n, nums[li], nums[ri]})
				li++
				ri--
				// we have incremented already, thus we are comparing li with li-1
				for li < ri && nums[li] == nums[li-1] {
					li++
				}
				for li < ri && nums[ri] == nums[ri+1] {
					ri--
				}
			} else if sum > 0 {
				ri--
			} else {
				li++
			}
		}
	}
	return res
}
```