# 8. Memory Management in Rust

Before learning **Ownership, Borrowing, and References**, we first need to understand why memory management is required.

Whenever we run a program—whether it is written in **C, C++, Java, JavaScript, Rust**, or any other programming language—the program uses some space in the computer's memory.

A computer mainly has two types of storage:

| SSD / Hard Drive | RAM |
| --- | --- |
| Stores data permanently | Stores data temporarily |
| Data remains after the computer is turned off | Data is removed when the computer is turned off |
| Has more storage capacity | Has less storage capacity |
| Slower than RAM | Much faster than SSD |

When a program is stored on our computer, it remains on the **SSD**. When we run the program, the required program data is loaded into **RAM** because RAM is much faster.

> Programs use RAM while they are running.
> 

![image.png](8%20Memory%20Management%20in%20Rust/image.png)

---

# What Is Memory Management?

**Memory management** is the process of allocating memory when a program needs it and releasing that memory when it is no longer required.

It mainly involves two operations:

### 1. Memory Allocation

Giving some memory to a program for storing data is called **memory allocation**.

For example:

```rust
let name = String::from("Kuldeep");
```

Rust allocates memory to store the text `"Kuldeep"`.

---

### 2. Memory Deallocation

Releasing memory after the stored data is no longer required is called **memory deallocation**.

The released memory can then be reused by other parts of the program or by other applications.

---

In simple words:

> **Allocate memory → Use the memory → Release the memory**
> 

Proper memory management is important because RAM is limited.

---

# Example of Memory Allocation

Consider the following JavaScript program:

```jsx
function main() {
    runLoop();
}

function runLoop() {
    let x = [];

    for (let i = 0; i < 100000; i++) {
        x.push(1);
    }

    console.log(x);
}

main();
```

Here:

```jsx
let x = [];
```

creates an empty array.

The loop runs `100,000` times:

```jsx
for (let i = 0; i < 100000; i++) {
    x.push(1);
}
```

During every iteration, the value `1` is added to the array.

As the array becomes larger, more memory is allocated in RAM.

After the `runLoop()` function finishes and the array is no longer needed, JavaScript's **Garbage Collector** eventually identifies the unused memory and releases it.

The process is approximately:

```
Program starts

      ↓

Array is created

      ↓

Memory is allocated in RAM

      ↓

Values are added to the array

      ↓

The function finishes

      ↓

The array is no longer required

      ↓

Garbage Collector eventually releases its memory
```

---

# Ways of Managing Memory

![image.png](8%20Memory%20Management%20in%20Rust/image%201.png)

Programming languages mainly use three approaches to memory management:

1. Garbage collection
2. Manual memory management
3. Rust's Ownership model

---

# 1. Garbage Collection

Languages such as:

- Java
- JavaScript
- C#
- Go

use a **Garbage Collector**, commonly called a **GC**.

A Garbage Collector is a part of the language runtime that automatically finds memory that is no longer being used and releases it.

For example:

```jsx
function createUser() {
    let name = "Kuldeep";

    console.log(name);
}

createUser();
```

When the function finishes, the `name` value is no longer required.

The Garbage Collector can later detect that unused memory and clean it automatically.

### Advantages

- Memory management is mostly automatic.
- Developers usually do not need to manually release memory.
- It reduces many common memory-related errors.
- It is generally easier for beginners.

### Disadvantages

- The Garbage Collector needs time and computer resources to run.
- Memory may not be released immediately.
- Garbage collection can sometimes cause small pauses during program execution.
- The programmer has less direct control over when memory is released.

> Garbage collection makes memory management easier but introduces some runtime overhead.
> 

---

# 2. Manual Memory Management

Languages such as **C** use manual memory management.

The programmer is responsible for:

1. Allocating memory
2. Using the memory
3. Releasing the memory

Conceptually:

```
Allocate memory manually

          ↓

Use the memory

          ↓

Release the memory manually
```

This provides developers with more control, but it also creates the possibility of memory-related errors.

### Common Problems

#### Memory Leak

A **memory leak** occurs when memory is allocated but is never released after it is no longer required.

Over time, the program may continue consuming more and more RAM.

```
Memory allocated

       ↓

Memory used

       ↓

Memory is no longer needed

       ↓

Program forgets to release it ❌
```

---

#### Dangling Pointer

A **dangling pointer** is a pointer that refers to memory that has already been released.

Trying to use such memory may cause:

- Unexpected behaviour
- Program crashes
- Security problems

---

#### Double Free

A **double-free error** occurs when a program tries to release the same memory more than once.

---

### Advantages

- Greater control over memory
- No garbage collector is required
- Can provide very high performance

### Disadvantages

- Memory management is more difficult.
- Developers must manage memory carefully.
- Mistakes can cause crashes and security vulnerabilities.
- Memory leaks and dangling pointers may occur.

---

# 3. The Rust Way: Ownership

Rust uses a different approach called the **Ownership model**.

Rust does not normally require:

❌ A Garbage Collector

❌ Manual memory allocation and deallocation for ordinary Rust code

Instead, Rust checks a set of Ownership rules while compiling the program.

When a value is no longer needed or its owner goes out of scope, Rust automatically releases its memory.

```
Value is created

       ↓

Memory is allocated

       ↓

A variable owns the value

       ↓

The owner goes out of scope

       ↓

Rust automatically releases the memory
```

Example:

```rust
fn main() {
    let name = String::from("Kuldeep");

    println!("{}", name);
}
```

Here:

```rust
let name = String::from("Kuldeep");
```

creates a `String`, and the variable `name` becomes its **owner**.

When the `main()` function ends:

```rust
}
```

`name` goes out of scope, and Rust automatically releases the memory used by the `String`.

We do not need to manually free the memory.

---

## Why Is Rust's Approach Special?

Rust tries to provide the advantages of both approaches:

| Garbage-Collected Languages | Manual Memory Management | Rust |
| --- | --- | --- |
| Automatic memory management | High control and performance | Automatic cleanup using Ownership |
| Easier to use | No Garbage Collector | No Garbage Collector |
| Runtime GC overhead | More memory-related risks | Many memory errors prevented during compilation |
| Example: Java, JavaScript | Example: C | Example: Rust |

Rust's Ownership system helps prevent many problems, such as:

- Dangling references
- Use-after-free errors
- Double-free errors
- Many memory-safety problems

Most of these checks happen during **compilation**, before the program runs.

---

# Does Rust Have a Garbage Collector?

No. Rust does not use a traditional Garbage Collector.

Instead, Rust determines when memory should be cleaned using rules related to:

- Ownership
- Scope
- Borrowing
- References
- Lifetimes

Because Rust does not need a Garbage Collector continuously checking memory while the program is running, it can provide predictable and high performance.

> The absence of a Garbage Collector is one important reason Rust can be fast, but it is not the only reason.
> 

---

# Concepts Used by Rust for Safe Memory Management

To understand Rust's memory-management system, we will study the following concepts:

### 1. Mutability

Rust variables are immutable by default.

We will learn how values can be safely changed using the `mut` keyword.

```rust
let mut age = 20;

age = 21;
```

---

### 2. Stack and Heap

We will understand the two important areas of memory used while a program is running:

- Stack
- Heap

Understanding them will make Ownership much easier.

---

### 3. Ownership

Ownership determines which variable is responsible for a value and its memory.

```rust
let name = String::from("Kuldeep");
```

Here, `name` owns the `String`.

---

### 4. Borrowing

Borrowing allows us to temporarily use a value without taking its ownership.

---

### 5. References

References allow us to access a value without becoming its owner.

They are created using the `&` symbol.

```rust
let name = String::from("Kuldeep");

let reference = &name;
```

---

### 6. Lifetimes

Lifetimes help Rust ensure that references remain valid for as long as they are being used.

They prevent references from pointing to data that has already been removed from memory.

---

# Quick Comparison

| Feature | Garbage Collector | Manual Management | Rust Ownership |
| --- | --- | --- | --- |
| Memory cleanup | Automatic | Done by programmer | Automatic based on Ownership rules |
| Garbage Collector | Yes | No | No |
| Runtime GC overhead | Present | Not present | Not present |
| Risk of memory errors | Lower | Higher | Many errors prevented by the compiler |
| Ease of use | Generally easier | More difficult | Can be difficult initially |
| Examples | Java, JavaScript, Go | C | Rust |

---

# Summary

Memory management means:

> **Allocating memory when data is needed and releasing it when the data is no longer required.**
> 

There are three common approaches:

1. **Garbage Collection**
    
    A Garbage Collector automatically finds and cleans unused memory.
    
2. **Manual Memory Management**
    
    The programmer manually allocates and releases memory.
    
3. **Rust's Ownership Model**
    
    Rust uses compile-time Ownership rules to manage memory safely without requiring a Garbage Collector.
    

Rust memory management mainly depends on:

> **Mutability → Stack and Heap → Ownership → Borrowing → References → Lifetimes**
> 

These concepts are connected. We will study them one by one before combining them to understand how Rust safely manages memory.