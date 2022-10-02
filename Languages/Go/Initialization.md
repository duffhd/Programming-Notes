In Go we have some simple ways to initialize a variable, structs or built-in types.

For variables we have:

```go
func main() {
    var typed string = "typed"
    var varButNoType = "var but no type"
	untyped := "untyped"
}
```

For structs:

```go
type User struct {
    name     string
    password string
}

func main() {
    emptyUser := NewEmptyUser()
    // Populating the fields of the empty struct.
    emptyUser.name = "empty name"
    emptyUser.password = "1234"

    user := NewUser()
    // Name is not zero initialized.
    fmt.Println(user.name)
}

func NewEmptyUser() *User {
    // The new function receives a type and returns a pointer to it
    // zero initialized.
    return new(User)
}

func NewUser(name string, password string) *User {
    // Similarly with new, we can just return the address of the newly
    // created user, in this case we are creating and populating its
    // fields.
    return &User {
        name: name,
        password: password,
    }
}
```

For maps, channels and slices:

```go
func main() {
    // make uses special syntax since it's built-in by the compiler
    // and not specified inside the language itself.
    ch := make(chan string)
    sl := make([]byte, 5)
	mp := make(map[string]int)
}
```