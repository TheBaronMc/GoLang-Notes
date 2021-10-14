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

stopped here : https://tour.golang.org/basics/16



