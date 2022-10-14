Inline functions are functions that are copied to where they are called, eliminating the overhead of a function call. This is good when you need to abstract some code and only one function will use it, or you have a very small function which is called in several places.

Differently from macros, inline functions are not expanded, they work as a normal function would.

You can define an inline function inside a `.h` file

```c
// inline_ex.h file
inline int sum(int n1, int n2) {
    return n1 + n2;
}
```

And it need’s to be declared on the `.c` file you’re going to use it (it work’s reversed from normal functions, that can’t be defined inside a `.h` file)

```c
// main.c file
#include "inline_ex.h"

// Here we need to declare the use of the inline function.
int sum(int n1, int n2);
// You can also declare the inline function like:
// extern int sum(int n1, int n2);
// extern inline int sum(int n1, int n2);
// 'inline' can only be used inside a .c file if 'extern' comes before it.
// all the above work the same, extern inline makes it more explicit (better).

void main() {
    sum(2, 2); // Calling the inline function
}
```

## Capture the inline function address

You can also capture an inline function to be used as a normal function, when this is done, the inline function will work as a normal function, that’s why we need to declare the inline function inside the `.c` files.

```c
#include "inline_ex.h"

extern inline int sum(int n1, int n2);

void main() {
    int (*captured_function)(int, int) = &sum;
    int val = captures_function(2, 2); // Returns 4
}
```

In this call the function will not be inlined, but called as a normal function.

For more information access: [GNU GCC Inline reference](https://gcc.gnu.org/onlinedocs/gcc/Inline.html)