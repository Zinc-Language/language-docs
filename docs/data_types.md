# Data Types

This page builds upon the concepts introduced in the [variables](./variables.md) section.

Data types in Zinc, as in any other programming language, define the kind of data a variable can hold. Zinc supports several built-in data types, including integers, floats, strings, booleans, and arrays.

## Built-in Data Types

### Integers

Integers are whole numbers without a decimal point. They can be positive or negative.

The data types for integers in Zinc are:
- `i8`
- `i16`
- `i32`
- `i64`
- `u8`
- `u16`
- `u32`
- `u64`
- `usize`

The `i` in front of the number tells the compiler (and you) that the variable is signed, meaning it can hold a positive or negative value.

The `u` in front of the number tells the compiler (and you) that the variable is unsigned, meaning it can only hold positive values.

The number beside the `i` or `u` represents the number of bits the integer can hold. For example, `i8` can hold 7 bits for the number and 1 bit for the sign, which can value between -128 and 127.

A `u8` can hold 8 bits for the number, which can value between 0 and 255, as it does not take into account the sign bit.

An integer in Zinc can be declared by using one of the above keywords.

Example:

```zinc
my_integer: i32 = 42; // The meaning of life
```

### Usize

`usize` is an unsigned integer (meaning it can only be positive) and is usually used to represent the size of a collection or the index of an element in an array.

The size of a `usize` depends on the architecture of the machine it is running on. On a 32-bit machine, `usize` is 32 bits, and on a 64-bit machine, `usize` is 64 bits.

You would typically use this data type when accessing elements in an array or iterating over a collection because it can hold the address of the largest possible address in memory, which is useful for indexing and iterating over collections.

### Floats

Floats are numbers with a decimal point. They can be positive or negative.

The data type for floats in Zinc are `f32` or `f64`.

`f32` is a single-precision floating-point number, while `f64` is a double-precision floating-point number.

This essentially means that `f64` is more precise than `f32`, but also takes up more memory and is slower to compute.

`f32` is generally sufficient for most applications, but `f64` should be used when higher precision is required (such as in scientific computing).

```zinc
const sorta_pi: f32 = 3.1415927;
const closer_to_pi: f64 = 3.141592653589793; // Much more precise
```

### Strings

Strings are sequences of characters enclosed in double quotes.

```zinc
my_string: string = "Hello, Zinc!"; // This is a string
```

### Booleans

Booleans are either true or false, which can be represented as a 1 or 0. While only having 2 states, bools are stored as `i8` with any value over greater than or equal to 1 being recognized as `true` and 0 being recognized as `false`.

You cannot add values to a boolean like you can to an integer.

🚫 Incorrect Usage
```zinc
my_bool: mut bool = false;

my_bool++; // Error: You cannot add values to a bool
```
✅ Correct Usage
```zinc
my_bool: mut bool = false;

my_bool = !my_bool; // OR
my_bool = true;     // OR
my_bool != false;   // OR
my_bool = !false;   // etc...
```

### Linked Lists

Linked lists are ordered collections of elements, where each element (called a node) points to the next one in the list.

Unlike vectors, linked lists do not store their elements in a continuous block of memory. Instead, each node holds its value and a reference to the next node.

This allows linked lists to grow and shrink easily without needing to copy memory, which makes them useful for situations where you frequently insert or remove elements from the middle of a collection.

In Zinc, linked lists are always **homogeneous**, meaning all elements in a list must be of the same type.

Example:
```zinc
my_list: List<i32> = [1, 2, 3, 4];
```

### Vectors

Vectors are ordered collections of elements that all share the same type. They store data in a contiguous block of memory, meaning all elements are placed right next to each other. This layout improves performance by optimizing memory access patterns and reducing overhead.

Because elements are tightly packed, vectors don’t store pointers between values like linked lists do. This allows for constant-time access to any element by index—Zinc can calculate the memory address directly by multiplying the index by the size of the element, rather than looping through the list.

Vectors are ideal for situations where fast access and memory efficiency are important.

> 💡 **List vs Vector**
> - **List<T>**: Great for frequent insertions/removals. Slower indexing.
> - **Vector<T>**: Fast indexing. Best for fixed-size or frequently read collections.

### Maps

Maps are collections of key-value pairs, where each key is unique and maps to a specific value. You can think of a map like a real-life dictionary: you look up a word (the key), and it gives you its definition (the value).

Maps in Zinc allow fast access to values by using their keys, which means you can index directly by key instead of by position like vectors or lists.

You can use the syntax `Map<K, V>` to define a map with keys of type `K` and values of type `V`.

Example:
```zinc
// Make a map with keys of type string and values of type i32
ages: Map<string, i32> = {
    "Alice": 30,
    "Bob": 25,
};
```

In this example, `"Alice"` and `"Bob"` are the keys, and `30` and `25` are their corresponding values. You can access them like this:

```zinc
print(ages["Alice"]); // Outputs: 30
print(ages["Bob"]); // Outputs: 25
```

Maps can also be iterated over, letting you loop through all the key-value pairs in any order (note: map order is not guaranteed to be consistent unless explicitly defined).

Use a `Map<K, V>` when:
- You want to look up values using unique keys
- You don’t care about the order of items
- You need efficient insertion and retrieval

Keys must be unique, but values can be duplicated.

Replaces a value:
```zinc
fun main() -> i32 {
    ages: mut Map<string, i32> {
        "Alice": 30,
        "Bob": 25,
    };

    ages.insert("Alice", 75);
    print(ages["Alice"]); // Outputs: 75

    return 0;
}
```
Or:
```zinc
fun main() -> i32 {
    ages: mut Map<string, i32> = {
        "Alice": 30,
        "Bob": 25,
    };

    ages["Alice"] = 75;
    print(ages["Alice"]); // Outputs: 75

    return 0;
}
```

### Enums

Enums are a way to define a set of named constants. They are useful when you have a fixed set of values that you want to represent in your code.

By default, each variant is automatically assigned a number starting from 0, but you can also explicitly assign custom values.

Here is an example of an enum:

```zinc
enum Color {
    Red, // 0
    Green, // 1
    Blue, // 2
}

fun main() -> i32 {
    print(Color.Red); // Prints: 0
    print(Color.Green); // Prints: 1
    print(Color.Blue); // Prints: 2

    return 0;
}
```

You can also define enums with custom values:

```zinc
enum RedValue {
    Red = 255,
    Green = 128,
    Blue = 0,
}

fun main() -> i32 {
    print(RedValue.Red); // Prints: 255
    print(RedValue.Green); // Prints: 128
    print(RedValue.Blue); // Prints: 0

    return 0;
}
```
This is useful when the enum variants need to represent meaningful numbers, such as RGB components, status codes, or bit flags.

# Structs

Structs in Zinc are user-defined data types that allow you to group together related data under a single name. They are useful for creating custom types that represent real-world entities or complex data structures.

This document outlines the syntax and usage of structs in Zinc.

In Zinc, you define a struct using the `struct` keyword, followed by the name of the struct and a block of code enclosed in curly braces `{}`. Inside the block, you declare the fields (members) of the struct, specifying their names and types.

Here is the syntax for defining a struct:

```zinc
struct StructName {
    field_name1: Type1,
    field_name2: Type2,
    // ... more fields
}
```
Example:
```zinc
struct Person {
    name: string,
    age: i32,
}
```

In this example, we define a struct named `Person` with two fields: `name` of type `string` and `age` of type `i32`.

Once you have defined a struct, you can create instances of it using the struct name followed by curly braces `{}` containing the values for each field.

Here is the syntax for creating an instance of a struct:
```zinc
struct_name: StructName {
    field_name1: value1,
    field_name2: value2,
    // ... more fields
};
```
Example:
```zinc
struct Person {
    name: string,
    age: i32,
}

fun main() -> i32 {
    billy: Person {
        name: "Billy",
        age: 30,
    };

    naomi: Person {
        name: "Naomi",
        age: 25,
    };

    return 0;
}
```
In this example, we create two instances of the `Person` struct: `billy` and `naomi`. Each instance has its own values for the `name` and `age` fields.

The order in which you assign values to the fields during the declaration must match the order in which the fields are declared in the struct definition.

✅ Correct:
```zinc
struct Person {
    name: string,
    age: i32,
}

fun main() -> i32 {
    billy: Person {
        name: "Billy",
        age: 30,
    };

    return 0;
}
```
🚫 Incorrect:
```
struct Person {
    name: string, // Name is first
    age: i32,     // Age is second
}

fun main() -> i32 {
    billy: Person {
        age: 30,       // Name should be the first field
        name: "Billy", // Age should be the second field
    };

    return 0;
}
```
To access the fields of a struct, you can use the syntax `struct_name.field_name`.

Example:
```zinc
struct Person {
    name: string,
    age: i32,
}

fun main() -> i32 {
    billy: Person {
        name: "Billy",
        age: 30,
    };

    println("{}", billy.name); // Prints "Billy"
    println("{}", billy.age); // Prints 30

    return 0;
}
```
A struct can also hold methods (functions) that operate on the data within the struct.

Example:

```zinc
struct Person {
  name: string,
  age: i32,

  fun happyBirthday() -> void {
    age++;
  }
}

fun main() -> i32 {
    // Make him `mut` so we can change his age
    billy: mut Person {
        name: "Billy",
        age: 30,
    };

    billy.happyBirthday(); // Adds 1 to billy's age
    println("{}", billy.age); // Prints 31

    return 0;
}
```
