# fun main()

In Zinc, the `fun main() -> i32` function is the entry point of a program. It is where the execution of the program begins.

The `-> i32` indicates that the function returns a 32-bit integer value.

You must always return some integer value from `main()`. By convention, this value is `0` to indicate successful execution. Any other values typically indicate an error, but it's not inherently wrong to return other values.

```zinc
fun main() -> i32 {
    println("Hello, world!"); // Prints "Hello, world!"
    return 0; // Indicates successful execution
}
```

You put your logic in the `main()` function, which is executed when the program starts. Zinc will not execute any other code unless it is called from `main()`.

To get arguments, you can use the `args` vector in the `main()` function.

Here is a snippet that gets the arguments and prints them out.
```zinc
fun main(args: Vec<string>) -> i32 {
    for arg in args {
        println!("Argument: {}", arg);
    }
    return 0;
}
```

> 🦦 Use `fun main()` to start your program.

Next Page [variables](variables.md)
