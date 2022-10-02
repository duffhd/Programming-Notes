# Go Cheatsheet

##### Table of contents

- [Variables](#variables)
- [Comments](#comments)
- [Pointers](#pointers)
- [Types](#types)
- [Imports](#imports)
- [Functions](#functions)
- [Control flow](#control-flow)
- [Structs](#structs)
- [Visibility](#visibility)
- [Goroutines](#goroutines)
- [Interfaces](#interfaces)
- [Json](#json)
- [Generics](#generics)

# Variables

```go
var name string = "hello"
name := "hello" // inferred type

var (
    name string = "hello"
    number int = 5
)
```

## Constants

```go
const name string = "hello"

const (
    name string = "hello"
    number int = 5
)
```

# Comments

```go
// Single line comment

/*
    Multiline Comment
*/

i := 12 // Inline comments
```

# Pointers

```go
import "fmt"

a := 50
var b *int      // Points to an int
b = &a          // b access a value field
fmt.Println(b)  // Prints the memory value like 0xc000065
fmt.Println(*b) // Pointer to access value, prints 50
```

# Types

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

# Imports

```go
package main

import "fmt"

// Massive imports
import (
    "fmt"
    "os"
    "encoding/json"
)
```

# Functions

```go
func main() {
    add(1, 1)
}

// Receives two ints and return an int
func add(n1, n2 int) int {
    return n1 + n2
}

// varargs - Receive multiple values
func add (nums ...int) {

}
```

## Entry point

```go
// main.go file

package main // Entry package

func main() { // Entry function
    // code
}
```

## Multiple returns

Go can work with multiple returns out-of-the-box.

```go
func accepted(id int) (bool, int) {
    if something {
        return true
    } else {
        return 10
    }
}

// To capture multiple values use the syntax
// b will the attached to the first return type, which is bool
// i will be attached to the second return type, which is int
b, i := accepted(10)

// If you don't want to capture some value just use _
b, _ := accepted(10)
```

# Control flow

## If

```go
if value != nil {
    // Do something.
} else if value == 0 {
    // Do other things.
} else {
    // Do other stuff.
}
```

## For

```go
// For statement
// For is the only way to loop on Go
for i := 0; i < 10; i++ {
    // code
}

for { // infinite loop

}

// Iterate and get values and index
for i, v := range something {
    // i = index
    // v = value
}
```

## Switch

```go
// Control flow keywords can have value assignement 
// to them after the keyword.
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

# Structs

```go
package main

import "fmt"

type Person struct {
    name   string,
    age    int,
    height int
}

func main() {
    // Create a Person
    var person Person = Person{"alex", 21, 180}
    person2 := Person{"person", 42, 170}

    // Access struct values
    fmt.Println(person.name)
}
```

## Methods on structs

```go
func main() {
    var person Person = Person{"alex", 21, 180}
    // Methods are called on instatiaded structs using . notation
    person.sayName()
}

func (p Person) sayName() {
    fmt.Println(p.name)
}

// To change the value on a struct, the use of 
// * pointer is needed.
func (p *Person) incrementAge(age int) {
    p.age = age
}
```

## Anonymous Structs

```go
strt := struct {
    name string
}{
    name: "Alex",
}
```

# Visibility

```go
// The public visibility can be set by making the first letter
// uppercase, structs, interfaces and functions all behave the same.
func PublicFunc() { }

// This function will only be visible by the file it's in.
func privateFunc() { }
```

# Goroutines

Goroutines are like liteweight threads, every function can be dispatched to run on a goroutine simply by calling it with the go keyword
before the function's name.

```go
func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")
    say("hello")

    // In place goroutine call
    go func() { 
    }()
}
```

## Concurrency

Channels are a way of sending values from multiple goroutines to a single data structure.

```go
// Channels are used to retrieve and group data from multiple goroutines.
func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // send sum to c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c // receive values from c

    fmt.Println(x, y, x+y)
}
```

## Select

Select is the same as the switch statement but for channels.

```go
import "fmt"

func main() {
    tick := time.Tick(100 * time.Millisecond)
    boom := time.After(500 * time.Millisecond)
    for {
        select {
            case <-tick:
                fmt.Println("tick.")
            case <-boom:
                fmt.Println("BOOM!")
                return
            default: // Triggered when no other case is available.
                fmt.Println("    .")
                time.Sleep(50 * time.Millisecond)
        }
    }
}
```

## Mutex

```go
// Mutex prevent race conditions when accessing non-synchronized
// data structures.

import "sync"

var mutex = sync.Mutex{}

func mutexExample() {
    // Lock the mutext for the current scope
    mutex.Lock()

    // defer makes sure that even if something goes wrong after
    // this line the expression will be executed, in this case
    // the mutex will be unlocked preventing errors.
    defer mutex.Unlock()
    // Do something.
}
```

# Interfaces

```go
// Interfaces describe methods to be implemented

import "fmt"

type Human interface {
    Think()
}

type Person struct () {
    name string
}

func (p Person) Think() {
    fmt.Println(p.name, "is thinking")
}

func main() {
    p := Person{"alex"}
    p.Think()
    // Here p == Human interface because it implements
    // all of its methods.
}
```

## Empty Interfaces

```go
package main

import (  
    "fmt"
)

func describe(i interface{}) {
    // Access the underlying interface value.
    fmt.Printf("Type = %T, value = %v\n", i, i)
}

func main() {  
    s := "Hello World"
    describe(s) // Type = string, value = Hello World

    i := 55
    describe(i) // Type = int, value = 55

    strt := struct {
        name string
    }{
        name: "Alex",
    }
    describe(strt) // Type = struct { name string }, value = {Naveen R}
}
```

# Json

```go
package main

import (
    "encoding/json"
    "fmt"
    "os"
)

// To make the conversion of structs to json the struct
// field needs to be public.
// After the type we can set the filed name, which will be 
// the name present on the json once we read from the json or
// convert to a json.
type response2 struct {
    Page   int      `json:"page"`
    Fruits []string `json:"fruits"`
}
```

# Generics

Go uses [] before function parameters to declare generics. You can create a generic by specifying its name followed by its type or constraint.

```go
// Here the V1 generic type can be of type int or float64
// Then we receive two values that have type V1 and return V1
func genericSum[V1 int | float64](value1 V1, value2 V1) V1 {
    return value1 + value2
}

func main() {
    var i int = genericSum(1, 1)
    var f float64 = genericSum(1.2, 1.5)

    // Here the types conflict and the addition cannot be made plus the return type is unknown 
    unknown := genericSum(1, 1.2) // compile error
}
```

## Constraints

You can constrain the generic values you receive in a function by specifying a constraint, which can be a type that implements an interface or just primitive types.

```go
type MyConstraint interface {
    Constrained()
}

type ConstrainedStruct struct {
    value string
}

func (c ConstrainedStruct) Constrained() {
    fmt.Println("I'm constrained to the MyConstraint interface")
}

// Here testConstraint will only receive types which implement the MyConstraint interface
func testConstraint[C MyConstraint](value C) {
    value.Constrained()
}
```

```go
type numbers interface {
    int64 | float64
}

// Here numbers can only be of type int64 or float64
func only64[V numbers](nums []V) V {
    var sum V

    for _, v := range nums {
        sum += v
    }

    return sum
}
```

## More Material

For examples of language features and library usage go to [Go by Example](https://gobyexample.com/)

For idiomatic use of the language refer to the official [Effective Go](go.dev/doc/effective_go)
