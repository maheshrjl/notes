# 🦍 Golang

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

