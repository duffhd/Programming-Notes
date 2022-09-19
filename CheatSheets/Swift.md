# Swift Cheatsheet

##### Table of Contents

## Variables

```swift
// let declares an immutable variable that can be typed
// or untyped
let name = "Ale"
name = "" // compile error
let name: String = "Ale" // type declaration

// var declares a mutable variable
var name = "Ale"
name = "" // valid
var name: String = "Ale" // typed var

// Emojis as variables
let 😇 = "Angel"
```

## Strings

### Multiline

```swift
let multilineString = """
multiline
string
"""
```

### Concat

```swift
let hello = "hello"
let world = " world"

let phrase = hello + world
print(phrase) // "hello world"
```

### Interpolation

```swift
let name = "Alex"

// Wrap a variable or function call with inside a string \()
print("Hello \(name)")
```

## More Material

For more in-depth examples and explanations refer to the official [Swift Language Guide](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html).
