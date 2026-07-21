# 10. Jargon #1 — Stack vs Heap

Before understanding **Ownership**, we need to understand two important areas of memory:

1. **Stack**
2. **Heap**

Whenever a Rust program runs, it stores data in memory. Depending on the type and size of the data, Rust may store it on the **stack**, the **heap**, or use both.

Understanding the stack and heap will make concepts such as **Ownership, Borrowing, References, and Lifetimes** much easier.

---

# Stack Memory

The **stack** is a fast area of memory used mainly for data whose size is known at compile time.

Examples include:

- Integers: `i32`, `i64`, `u32`, etc.
- Floating-point numbers: `f32`, `f64`
- Boolean values: `bool`
- Characters: `char`
- Fixed-size arrays
- Other fixed-size values

Example:

```rust
fn main() {
    let age: i32 = 21;

    let price: f64 = 99.99;

    let is_learning: bool = true;
}
```

The values stored by these variables have fixed and known sizes.

For example:

```
age

Type: i32

Size: 4 bytes
```

The Rust compiler knows the size of an `i32` before the program starts running. Therefore, its value can be stored directly on the stack.

---

## How Does the Stack Work?

The stack follows a rule called:

> **LIFO — Last In, First Out**
> 

This means:

> The last value added to the stack is the first value removed.
> 

Think of a stack of plates:

```
        ┌─────────┐
        │ Plate 3 │  ← Removed first
        ├─────────┤
        │ Plate 2 │
        ├─────────┤
        │ Plate 1 │  ← Added first
        └─────────┘
```

We place new plates on the top and also remove plates from the top.

Computer stack memory works using a similar idea.

---

## Adding and Removing Data

Adding data to the stack is commonly called:

```
Push
```

Removing data from the stack is commonly called:

```
Pop
```

Example:

```
Push A

┌─────┐
│  A  │
└─────┘

Push B

┌─────┐
│  B  │ ← Top
├─────┤
│  A  │
└─────┘

Pop

B is removed first.
```

Because the stack follows a simple order, allocating and removing stack memory is very fast.

---

# Stack Example with Numbers

```rust
fn main() {
    let a: i32 = 10;

    let b: i32 = 20;

    let c: i32 = a + b;

    println!("{}", c);
}
```

Output:

```
30
```

Conceptually, the stack may look like:

```
Stack

┌────────────┐
│ c = 30     │
├────────────┤
│ b = 20     │
├────────────┤
│ a = 10     │
└────────────┘
```

All these values have the type `i32`.

An `i32` always has a fixed size of **4 bytes**, so Rust knows how much memory is required before the program runs.

---

# Stack Memory with Functions

Whenever a function is called, a new section is added to the stack.

This section is commonly called a **stack frame**.

A stack frame stores information related to a function call, such as:

- Function parameters
- Local variables
- Information needed to return to the calling function

Consider this program:

```rust
fn add(a: i32, b: i32) -> i32 {
    let result = a + b;

    result
}

fn main() {
    let answer = add(10, 20);

    println!("{}", answer);
}
```

Output:

```
30
```

---

## What Happens in Memory?

First, the program starts executing `main()`.

A stack frame for `main()` is created:

```
Stack

┌──────────────────┐
│ main()           │
└──────────────────┘
```

Then this function is called:

```rust
add(10, 20);
```

A new stack frame for `add()` is placed on top:

```
Stack

┌──────────────────┐
│ add()            │
│ a = 10           │
│ b = 20           │
│ result = 30      │
├──────────────────┤
│ main()           │
└──────────────────┘
```

After `add()` returns the value `30`, its stack frame is removed:

```
Stack

┌──────────────────┐
│ main()           │
│ answer = 30      │
└──────────────────┘
```

When `main()` finishes, its stack frame is also removed.

---

# Heap Memory

The **heap** is another area of memory.

It is used for data whose size may:

- Not be known at compile time
- Be decided while the program is running
- Grow or shrink during runtime

Common examples include:

- `String`
- `Vec<T>` or vectors
- Dynamically created data

Example:

```rust
fn main() {
    let name = String::from("Kuldeep");

    println!("{}", name);
}
```

The text:

```
Kuldeep
```

is stored on the heap.

![image.png](10%20Jargon%20#1%20%E2%80%94%20Stack%20vs%20Heap/image.png)

---

## Why Does a `String` Use the Heap?

A Rust `String` can grow while the program is running.

Example:

```rust
fn main() {
    let mut name = String::from("Kuldeep");

    name.push_str(" Sharma");

    println!("{}", name);
}
```

Output:

```
Kuldeep Sharma
```

Initially, the string contains:

```
Kuldeep
```

Later, more text is added:

```
 Sharma
```

The final string becomes:

```
Kuldeep Sharma
```

Since the amount of text may change while the program is running, its contents are stored on the heap.

---

# Important: A `String` Uses Both Stack and Heap

Saying that a `String` is stored only on the heap is a simplification.

A Rust `String` uses **both the stack and the heap**.

Consider:

```rust
let name = String::from("Kuldeep");
```

Conceptually:

```
STACK                         HEAP

┌──────────────┐             ┌──────────────┐
│ Pointer ─────┼────────────→│ K u l d e e p│
├──────────────┤             └──────────────┘
│ Length: 7    │
├──────────────┤
│ Capacity: 7  │
└──────────────┘
```

The `String` information stored on the stack contains:

1. **Pointer**
2. **Length**
3. **Capacity**

The actual text is stored on the heap.

---

## Pointer

The pointer stores the memory address where the actual string data is located on the heap.

```
Stack                          Heap

Pointer ─────────────────────→ "Kuldeep"
```

The pointer helps Rust locate the heap data.

---

## Length

Length represents how much data the string currently contains.

For:

```
Kuldeep
```

the length is:

```
7 bytes
```

---

## Capacity

Capacity represents how much memory has currently been reserved for the string on the heap.

For example:

```
Length:   7

Capacity: 10
```

This means:

- The string currently uses `7` bytes.
- It has space reserved for up to `10` bytes before more memory may be needed.

> For simple ASCII text, each character uses one byte. Unicode characters may use more than one byte.
> 

---

# Heap Allocation

When a program needs heap memory, it requests a suitable amount of memory.

The memory system finds an available location and returns its address.

Conceptually:

```
Request memory

       ↓

Memory is allocated on the heap

       ↓

Address of the memory is returned

       ↓

A pointer stores that address
```

Finding and managing heap memory requires more work than simply placing data on top of the stack.

Therefore, heap allocation is generally slower than stack allocation.

---

# Stack vs Heap

| Stack | Heap |
| --- | --- |
| Stores fixed-size data | Stores dynamically sized data |
| Size is known at compile time | Size may be decided or changed at runtime |
| Allocation is very fast | Allocation generally requires more work |
| Automatically follows function scope | Memory is accessed through pointers |
| Uses LIFO order | Does not follow simple LIFO order |
| Common for numbers and Booleans | Common for `String` and `Vec<T>` data |

---

# What Is Commonly Stored on the Stack?

Examples include:

### Integers

```rust
let age: i32 = 21;
```

### Floating-Point Numbers

```rust
let price: f64 = 99.99;
```

### Booleans

```rust
let is_logged_in: bool = true;
```

### Characters

```rust
let grade: char = 'A';
```

### Fixed-Size Arrays

```rust
let numbers: [i32; 5] =
    [10, 20, 30, 40, 50];
```

The size of this array is known at compile time:

```
5 values × 4 bytes each

= 20 bytes
```

Therefore, its data can be stored directly on the stack in this simple case.

---

# What Commonly Uses Heap Memory?

Examples include:

### `String`

```rust
let language =
    String::from("Rust");
```

The actual text data is stored on the heap.

---

### Vector

```rust
let numbers =
    vec![10, 20, 30];
```

A vector is a growable collection.

More values can be added while the program is running:

```rust
fn main() {
    let mut numbers =
        vec![10, 20, 30];

    numbers.push(40);

    println!("{:?}", numbers);
}
```

Output:

```
[10, 20, 30, 40]
```

The vector's elements are stored on the heap because the number of elements can grow or shrink during runtime.

> We will study vectors separately later.
> 

---

# Memory in Action

Consider the following program:

```rust
fn main() {
    stack_fn();

    heap_fn();

    update_string();
}

fn stack_fn() {
    let a = 10;

    let b = 20;

    let c = a + b;

    println!(
        "Stack function: \
        The sum of {} and {} is {}",
        a,
        b,
        c
    );
}

fn heap_fn() {
    let s1 =
        String::from("Hello");

    let s2 =
        String::from("World");

    let combined =
        format!("{} {}", s1, s2);

    println!(
        "Heap function: \
        Combined string is '{}'",
        combined
    );
}

fn update_string() {
    let mut s =
        String::from("Initial string");

    println!(
        "Before update: {}",
        s
    );

    s.push_str(
        " and some additional text"
    );

    println!(
        "After update: {}",
        s
    );
}
```

Output:

```
Stack function: The sum of 10 and 20 is 30

Heap function: Combined string is 'Hello World'

Before update: Initial string

After update: Initial string and some additional text
```

---

# Understanding `stack_fn()`

```rust
fn stack_fn() {
    let a = 10;

    let b = 20;

    let c = a + b;
}
```

The variables contain integer values:

```
a = 10

b = 20

c = 30
```

Their sizes are known at compile time.

Conceptually:

```
STACK

┌────────────┐
│ c = 30     │
├────────────┤
│ b = 20     │
├────────────┤
│ a = 10     │
└────────────┘
```

When `stack_fn()` finishes, its stack frame is removed.

---

# Understanding `heap_fn()`

```rust
fn heap_fn() {
    let s1 =
        String::from("Hello");

    let s2 =
        String::from("World");

    let combined =
        format!("{} {}", s1, s2);
}
```

The variables `s1`, `s2`, and `combined` are values of the `String` type.

Their `String` metadata is stored on the stack, while their actual text is stored on the heap.

Conceptually:

```
STACK                         HEAP

┌─────────────┐              ┌─────────┐
│ s1 metadata │─────────────→│ "Hello" │
├─────────────┤              └─────────┘
│ s2 metadata │─────────────→┌─────────┐
├─────────────┤              │ "World" │
│ combined    │───────┐      └─────────┘
│ metadata    │       │
└─────────────┘       └─────→┌───────────────┐
                             │ "Hello World" │
                             └───────────────┘
```

When the function ends, these variables go out of scope, and Rust cleans up their owned heap memory.

---

# Understanding `update_string()`

```rust
fn update_string() {
    let mut s =
        String::from("Initial string");

    s.push_str(
        " and some additional text"
    );
}
```

The variable is mutable:

```rust
let mut s
```

because the string is modified later.

Initially:

```
Initial string
```

After calling:

```rust
s.push_str(
    " and some additional text"
);
```

the string becomes:

```
Initial string and some additional text
```

If the existing heap allocation does not have enough capacity for the additional text, Rust's `String` may:

1. Request a larger area of heap memory.
2. Move or copy the text into the new allocation.
3. Update its pointer and capacity.
4. Release the old allocation.

This memory management is handled by the `String` type.

---

# Important Clarification

It is common to say:

> Numbers are stored on the stack, while strings are stored on the heap.
> 

This is useful as a beginner-level explanation, but the more accurate rule is:

> Data with a fixed, compile-time-known size can generally be stored directly on the stack. Dynamically sized or growable data is commonly stored on the heap and accessed through fixed-size information stored on the stack.
> 

For example:

```rust
let name: &str = "Kuldeep";
```

and:

```rust
let name: String =
    String::from("Kuldeep");
```

do not have the same memory representation.

A `String` owns dynamically allocated text, while a string literal such as `"Kuldeep"` is stored differently.

We will understand this distinction more clearly while studying **Ownership and Borrowing**.

---

# Quick Summary

| Concept | Meaning |
| --- | --- |
| Stack | Fast memory used mainly for fixed-size data |
| Heap | Memory commonly used for dynamically sized or growable data |
| LIFO | Last In, First Out |
| Push | Add data to the stack |
| Pop | Remove data from the stack |
| Stack frame | Memory used for a function call |
| Pointer | Stores the address of data |
| `String` | Stores metadata on the stack and text data on the heap |
| `Vec<T>` | Stores metadata on the stack and elements on the heap |

Common examples:

```
Usually stored directly on the stack:

i32
i64
f32
f64
bool
char
Fixed-size arrays
```

```
Commonly use heap allocation:

String
Vec<T>
Other dynamically sized or growable data
```

> The stack is fast and works well for fixed-size data. The heap provides flexibility for data that may grow or whose size is determined at runtime. Rust uses Ownership to safely manage heap-allocated memory without requiring a garbage collector.
>