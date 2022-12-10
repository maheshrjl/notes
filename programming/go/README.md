# 👨💻 Go

### Functions

{% hint style="info" %}
Functions are their own data type in go

* Functions can be assigned to variables
* Functions can be passed as paramters
{% endhint %}

#### Funtion with multiple parameters of same type

```go
func functionName(param1, param2 int) {}
// Funtion call
functionName(param1, param2)
```

#### Funtion with parameters of different types

```go
func functionName() (int, string) {
    return 22, "Maheshrjl"
}
// Function Call
var x, str = functionName()
```

#### A function with a return type

```go
func functionName(param1, param2 int) int{}

//Function Call
returnVariable := functionName(param1, param2)
```

#### A function with multiple return types

```go
func add(x,y int) (int, int){return x,y}
//Function call
varx, vary := add(1,1)
```

#### Labelling return types

```go
func add(x,y int) (variablex int, variabley int){
    variablex = x
    vairabley = y
    return
}
//Function call
varx, vary := add(1,1)
```

#### Defer execution until function return

```go
func add(x,y int) (variablex int, variabley int){
    defer fmt.Println("Adding x + y") // Execute this at function return
    variablex = x
    vairabley = y
    return
}
```

#### Assign a function to a variable & call it

```go
func test(){
    fmt.Println("Hello!")
}

func main(){
    x := test
    x() // This calls the test function
}
```

#### Create a function inside a function

```go
func main(){
    test := func() {//Test  is a variable storing a function
        fmt.Println("Hello!")
    }
    test()
}
```

#### Pass a function to another function

```go
// Work in Progress
```

#### Function returning a function / function closure

A closure is a function value that references variables from outside its body

```go
// Work in Progress
```

## Data Structures

### Array

An array type defiiniation specifies a length and an element type. Eg: `4[int]` represents an array of 4 integers.&#x20;

* Size of an array is fixed and it's length is a part of it's type (`[4]int` and `[5]int` are distinct, incompatible types.
* Arrays do not need to be initalized explicitly, an unassigned array will have a 0 value.

#### Initalizing Array:

```go
name := [2]string{"Mahesh","Rijal"}
```

or

```go
name := [...]string{"Mahesh","Rijal"}
```

### Slices

Array has a fixed size. But, slices are dynamically sized. [Slices](https://go.dev/blog/slices-intro) support `append` and `copy` functions

#### Create a empty slice with make

```go
s := make([]string, 3)
```

The `make` function allocates a zeroed array and returns a slice that refers to that array

### Struct

A struct is a sequence of named elements, called fields, each of which has a name & type.

```go
type Rectangle struct{
    Width float64
    Height float64
}
```
