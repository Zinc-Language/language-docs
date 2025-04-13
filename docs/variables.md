# Variables

Variables are the fundamental building blocks of any useful programming language. They allow you to store and access data for manipulation and later use.

This document outlines the technical details of variables in Zinc.

### Declaration

In Zinc, variables can be declared by writing the variable's name followed by a colon, its mutability, and its [type](./data_types.md).

Here is an example of variable declarations in Zinc:
```zinc
fun main() -> i32 {
    // name: type = value;
    message: string = "Hello, Zinc!";
    age: i32 = 42;

    return 0;
}
```

If a variable is declared without an explicit value, Zinc assigns a default value based on its type at compile time.

If no value is provided, Zinc assigns a default value based on the variable’s type:

| Type                 | Default Value |
| -------------------- | ------------- |
| **string**           | `""`          |
| **i8**               | `0`           |
| **i16**              | `0`           |
| **i32**              | `0`           |
| **i64**              | `0`           |
| **u8**               | `0`           |
| **u16**              | `0`           |
| **u32**              | `0`           |
| **u64**              | `0`           |
| **usize**            | `0`           |
| **f32**              | `0.0`         |
| **f64**              | `0.0`         |
| **bool**             | `false`       |
| **char**             | `'\0'`        |
| **vector**           | `[]`          |
| **map**              | `{}`          |

In Zinc, there is no `let` or `var` keyword to declare a variable. Instead, the compiler infers that a variable is being declared when it is assigned a type.

This design makes Zinc simpler to read and write, placing the variable name first for clarity, followed by its type.

A variable's type cannot be changed after it is declared.

Examples:

✅ Correct
```zinc
fun main() -> i32 {
    my_var: mut string = "Hello, Zinc!"; // Make it mutable
    my_var = "New value";

    return 0;
}
```
🚫 Incorrect
```zinc
fun main() -> i32 {
    my_var: mut string = "Hello, Zinc!"; // Make it mutable
    my_var: i32 = 123; // Error: `my_var` has already been declared in this scope with type `string`

    return 0;
}
```

Because of this rule, the language is much more explicit and significantly less error-prone.

> 🦦 Declare variables by using the `name: [mut] type = value` syntax.

### Mutability

In Zinc, variables can either be mutable or immutable. Mutable variables can have their values changed after they are declared, while immutable variables cannot.

Why use immutability?

Immutability is a safety feature that prevents accidental changes to data. It also makes code easier to reason about and debug, knowing that the value of a variable will not change unexpectedly.

By default, variables are immutable. To make a variable mutable, you can use the `mut` keyword before the type.

This feature makes sure that the variable's value remains consistent throughout its scope or the lifetime of the program.

Here are some examples of mutable and immutable variables:

Mutable variable:
```zinc
fun main() -> i32 {
    message: mut string = "Hello, Zinc!";
    message = "More Zinc!";

    return 0;
}
```

Immutable variable:
```zinc
fun main() -> i32 {
    message: string = "Hello, Zinc!";
    message = "More Zinc!"; // Error: cannot assign to immutable variable!

    return 0;
}
```

> 🦦 Use immutability for values that should *never* change.

### Locking

In Zinc, variables must be declared with a default value and can later be locked to another value to prevent further changes.

To declare a variable that can be locked later, wrap its type in `lock<>`. Once locked, the variable cannot be changed, much like a final assignment.

To lock a lockable variable, use the `.set()` method and pass the desired value in the parenthesis.

```zinc
MESSAGE: lock<string> = "Default";

fun main() {
    // Locks the variable; its value can't be changed again
    MESSAGE.set("Locked now, haha!");

    MESSAGE = "Oops..."; // Error: You can't change the value of a locked variable

    MESSAGE.set("Uh oh..."); // Error: Already locked!

    return 0;
}
```

You can get the value of a locked variable the same way that you would any other variable.

```zinc
MESSAGE: lock<string> = "Default String";

fun main() -> i32 {
    MESSAGE.set("Zinc is cool!");
    print("{}", MESSAGE); // Zinc is cool!

    return 0;
}
```

Why would you want a locked variable?

Locking, similarly to immutability, is a safety feature that prevents unwanted changes to certain variables.

Locked variables offer more flexibility than immutable ones, allowing you to assign a default and finalize it later. This is useful when the final value isn't known at compile time.

Here is a simple example of when you would use a locked variable:

```zinc
USERNAME: lock<string> = "John Doe";
const INPUT: string = "Ollie";

fun main() -> i32 {
    if INPUT == "" {
      print("{}", USERNAME); // Prints 'John Doe'
    } else {
      USERNAME.set(INPUT); // Lock the username
      print("{}", USERNAME); // Prints 'Ollie'
    }

    return 0;
}
```

🤔 What would happen if you replaced the `input` string's value with an empty string?

Immutable Vs Locked Variables

| Feature                    | Immutable Variable | Locked Variable |
|---------------------------|--------------------|-----------------|
| Can change after assignment? | ❌                 | ✅ Until set |
| Can assign after declaration? | ❌               | ✅ Until set         |
| Can be locked explicitly? | ❌                 | ✅ Yes (`.set`) |

> 🦦 Use locking when a value might be updated once at runtime.

### Constants

Constants in Zinc are declared using the `const` keyword.

Here is an example of declaring a constant:
```zinc
const PI: f64 = 3.14159265358973;
```

Unlike immutable variables, constants must be fully known at compile time and cannot be computed during runtime initialization.

> 🦦 Use constants for values known before runtime, and that will *never* change.

### Naming

Variables in Zinc should follow the `snake_case` naming convention, where all the words are lower case and separated by underscores.

```zinc
good_variable_name: i32 = 42; // uses good naming conventions

badvariablename: i32 = 42; // uses bad naming conventions
```

Constants should follow the `SCREAMING_SNAKE_CASE` naming convention, where all the words in the name are uppercase and separated by underscores.

```zinc
const GOOD_CONSTANT_NAME: i32 = 42; // Uses good naming conventions

const bad_constant_name: i32 = 42; // Uses bad naming conventions
```

Lock variables should also follow the `SCREAMING_SNAKE_CASE`. This is because lock variables essentially become constant after being set.

```zinc
GOOD_LOCK_VARIABLE_NAME: lock<i32> = 42; // Uses good naming conventions

bad_lock_variable_name: lock<i32> = 42; // Uses bad naming conventions
```

These naming conventions are generally standard for most programming languages and help ensure code readability and maintainability.

The compiler will warn you if you don't follow these naming conventions though. The warnings can be ignored and won't affect compilation, but will still be displayed in the compiler output and language server.

To ignore these errors, you can pass the flag `--ignore-warnings` to the compiler, or optionally use the `@[ignore(naming_conventions)]` attribute at the top of the file. This is generally not recommended as it can lead to confusion and (potentially) errors in the code.

```zinc
@[ignore(naming_conventions)] // Add this to ignore naming conventions warnings

const BadNamingConvention: i32 = 42; // The compiler will ignore this, but it's not recommended

fun main() -> i32 {
    BAD_VARIABLE_NAME: i32 = 42; // Uses bad naming conventions
    println("{}, {}", BadNamingConvention, BAD_VARIABLE_NAME); // 42, 42

    return 0;
}
```

Variable names should be descriptive and meaningful, but the compiler can't enforce this so it's up to the programmer to ensure their code is readable and maintainable.

Zinc only allows variable names to contain letters [a-zA-Z], numbers [0-9], and underscores '_'. Any other characters like spaces, special characters, or emojis are not allowed by the compiler to ensure the widest range of compatibility.

Here is a table of good and bad naming conventions:

| Variable type | ✅ Good Naming | 🚫 Bad Naming |
| --- | --- | --- |
| Variable | `snake_case` | `camelCase`, `SCREAMING_SNAKE_CASE`, `emoji_😊`, `with space`,... |
| Constant | `SCREAMING_SNAKE_CASE` | `snake_case`, `camelCase`, `EMOJI_😊`, `WITH SPACE`,... |
| Lock | `SCREAMING_SNAKE_CASE` | `snake_case`, `camelCase`, `EMOJI_😊`, `WITH SPACE`,... |

> 🦦 Use `snake_case` for normal variables and `SCREAMING_SNAKE_CASE` for constants and locks.

### Assigning

In Zinc, variables are assigned values by using the `=` operator.

You can only assign a variable a new value if the variable is mutable.

Here is an example of changing the value of a variable:
```zinc
fun main() -> i32 {
    message: mut string; // Assigned "" by default
    message = "More Zinc!";

    println(message); // More Zinc!

    return 0;
}
```

```zinc
fun main() -> i32 {
    message: string = "Hello, Zinc!";
    message = "More Zinc!"; // Error: Cannot assign to immutable variable

    return 0;
}
```

Additionally, you use the `=` operator to assign a value to a new variable.

```zinc
new_message: string = "Hello, Zinc!";
```

> 🦦 Use `=` to assign a new value to a mutable variable, or to assign a value to a new variable.

**Next Page:** [Data Types](./data-types.md)
