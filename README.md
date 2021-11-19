# GoLang - Notes

All the informations below come from the [Go Tour](https://tour.golang.org/list).

## Basics

### Hello World

``` Go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello World!")
}
```

### Packages

Every program start running in package `main`.

You can import package in the `import` clause.  For example, if you want to import the random module, you can do it like this :

``` go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```

You can factorize the imports, like on the example above, or you can do it like that :

```go
import "fmt"
import "math"
```

The things which are exported from a module has to to start with a capital letter, like `Pi` in `math.Pi`. Otherwise, their will be not.

### Functions

``` go
func add(x int, y int) int {
	return x + y
}
```

*Note that the type comes after the variable name. More informations on the syntax [here](https://go.dev/blog/declaration-syntax).*

You can "factorize" the type declaration `x int, y int` -> `x, y int`.

You can return multiple things :

``` go
func swap(x, y string) (string, string) {
	return y, x
}
```

You can use the **naked function syntax** :

``` go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```

Be careful, only use it in short functions.

### Variables

``` go
var c, python, java bool // init with 'false' by default
var i int // init with 0 by default

/*
If an initializer is present, the type can be omitted
The variable will take the type of the initializer.
*/
var c, python, java = true, false, "no!"

// Short initialisation syntax
k := 3
c, python, java := true, false, "no!"
```

``` go
// ALL TYPES
bool // init with 'false'

string // init with ""

// init with 0
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

``` go
i := 42
f := float64(i)
u := uint(f)
```

``` go
const Pi = 3.14 // Constants cannot be declared using the := syntax.
```

```go
// Create a huge number by shifting a 1 bit left 100 places.
// In other words, the binary number that is 1 followed by 100 zeroes.
Big = 1 << 100
// Shift it right again 99 places, so we end up with 1<<1, or 2.
Small = Big >> 99
```

## Statement

### For

```go
for i := 0; i < 10; i++ {
    /* CODE */
}
```

The init statement will often be a short variable declaration, and the variables declared there are visible only in the scope of the `for` statement.

Unlike other languages like C, Java, or JavaScript there are no parentheses surrounding the three components of the `for` statement and the braces `{ }` are always required.

The init statement is optional:

```go
sum := 1
for ; sum < 1000; {
    sum += sum
}
```

### For is Go's "while"

```go
sum := 1
for sum < 1000 {
    sum += sum
}
```

The infinite loop:

```go
for {
}
```

### If

```go
x := 5
if x < 0 {
    return sqrt(-x) + "i"
}
```

If with a short statement

```go
if v := math.Pow(x, n); v < lim {
    return v
}
```

if - else

```go
if v := math.Pow(x, n); v < lim {
    return v
} else {
    fmt.Printf("%g >= %g\n", v, lim)
}

if condition {
    /* ... */
} else if condition {
    /* ... */
} else {
    /* ... */
}
```

### Switch

```go
switch os := runtime.GOOS; os {
    case "darwin":
    fmt.Println("OS X.")
    case "linux":
    fmt.Println("Linux.")
    default:
    // freebsd, openbsd,
    // plan9, windows...
    fmt.Printf("%s.\n", os)
}
```

Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

```go
switch i {
    case 0:
    case f():
}
```

does not call `f` if `i==0`.)

The condition of a switch statement is optional.

```go
switch {
    case t.Hour() < 12:
    fmt.Println("Good morning!")
    case t.Hour() < 17:
    fmt.Println("Good afternoon.")
    default:
    fmt.Println("Good evening.")
}
```

It can be a clean way to write long if-then-else chains.

### Defer

A `defer` statement defers the execution of a function until the surrounding function returns.

```go
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}

/*
OUTPUT:
hello
world
*/
```

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

The defer calls are stored in a queue. The strategy of this queue is last-in-first-out.

More info [here](https://go.dev/blog/defer-panic-and-recover)

## More types

### Pointers

The type `*T` is a pointer to a `T` value. Its zero value is `nil`.

The `&` operator generates a pointer to its operand.

The `*` operator denotes the pointer's underlying value.

```go
i, j := 42, 2701

p := &i         // point to i
fmt.Println(*p) // read i through the pointer
*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i

p = &j         // point to j
*p = *p / 37   // divide j through the pointer
fmt.Println(j) // see the new value of j
```

### Structs

A struct is a collection of fields

```go
type Vertex struct {
	X int
	Y int
	Z int
}

func main() {
    v := Vertex{1, 3, 3}
    fmt.Println(v) // {1, 3, 3}
    v.X = 4
	fmt.Println(v.X) // 4
}
```

Struct fields can be accessed through a struct pointer.

To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, that notation is cumbersome, so the language permits us instead to write just `p.X`, without the explicit dereference.

```go
var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```

### Arrays

The type `[n]T` is an array of `n` values of type `T`.

```go
var a [10]int
```

> An array's length is part of its type, so arrays cannot be resized.

#### Slicing

An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

The type `[]T` is a slice with elements of type `T`.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:

```go
a[low : high]
```

This selects a half-open range which includes the first element, but excludes the last one.

The following expression creates a slice which includes elements 1 through 3 of `a`:

```go
a[1:4]
```

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4]
```



stopped [here](https://tour.golang.org/moretypes/8)


## Methods and interfaces



## Concurrency

