Static functions in C are used to modify a function visibility. In C every function is global, meaning that if you define a function inside a `.c` file, by simply importing the file you can use every function inside it, but by declaring a variable `static` it makes the function only available to be called inside that file.

Example:

```c
// main.c file

#include "sum.h"

void main() {
    sum(5, 5); // Can't compile
}
```


```c
// sum.h file
int sum (int n1, int n2);

// sum.c file
static int sum(int n1, int n2) { 
    return n1 + n2;
}
```

Your program wont compile because we cannot find the definition for the `sum` function from the `main` file, if `sum` wasn’t declared with `static`, then the program would compile and run as expected.

It’s considered a good practice to declare a function with `static` if it’s only going to be used inside the file it’s declared on. 
