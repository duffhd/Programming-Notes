There are 3  written conventions for reserved identifiers in C

- _Bool -> Words with a **leading underscore** followed by a **capital letter**.
- __reserved -> Words with **two leading underscores**.
- _type -> Words with a **leading underscore** followed by a **lower letter**.

The only naming convention that can be used are for local variables and member variables in struct is the _lower underscore format, for example:

```c
void main() {
    int _local = 10; // Valid
}

struct example {
    int _var // Valid
};
```