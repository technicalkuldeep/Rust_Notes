# 9. Jargon #0 — Mutability in Rust

Before understanding **Ownership**, we need to understand an important Rust concept called **Mutability**.

Mutability tells us whether the value stored inside a variable can be changed after the variable has been created.

A variable can be:

1. **Immutable** → Its value cannot be changed.
2. **Mutable** → Its value can be changed.

---

# Immutable Variables

An **immutable variable** is a variable whose value cannot be changed after it has been assigned.

In Rust, variables are **immutable by default**.

Example:

```rust
fn main() {
    let x: i32 = 1;

    x = 2;

    println!("{}", x);
}
```

This code produces a compilation error.

The variable was initially assigned:

```rust
let x: i32 = 1;
```

Later, we tried to change its value:

```rust
x = 2;
```

However, `x` is immutable because it was created using only the `let` keyword.

Therefore, Rust does not allow its value to be changed.

Conceptually:

```
x = 1

Try to change:

x = 2  ❌ Error
```

The compiler gives an error similar to:

```
cannot assign twice to immutable variable `x`
```

---

## Why Are Variables Immutable by Default?

Many programming languages allow variables to change by default. Rust takes the opposite approach.

Rust makes variables immutable by default because it helps make programs:

- Safer
- Easier to understand
- Easier to debug
- Better suited for concurrent programming
- Easier for the compiler to optimize

---

### 1. Prevents Accidental Changes

Suppose we create a variable:

```rust
let total_marks = 500;
```

We expect `total_marks` to remain `500`.

If another part of the program accidentally tries to change it:

```rust
total_marks = 400;
```

Rust produces a compilation error.

This prevents accidental changes and makes the program's behaviour more predictable.

---

### 2. Makes Code Easier to Understand

When we see:

```rust
let age = 21;
```

we know that the value of `age` will not change later.

We do not need to search through the entire program to check whether another line modifies it.

This makes code easier to read and understand.

---

### 3. Helps with Thread Safety

A **thread** is an independent path of execution inside a program.

Multiple threads may access the same data at the same time.

If the data cannot be changed, multiple threads can read it without worrying that another thread may modify it unexpectedly.

```
Thread 1 ───┐
            │
Thread 2 ───┼──→ Read immutable data ✅
            │
Thread 3 ───┘
```

Since no thread can modify immutable data, many synchronization problems are avoided.

> Immutable data is generally safer to share between multiple threads because it cannot be changed unexpectedly.
> 

We will study threads in detail later.

---

### 4. Can Help the Compiler Optimize Code

If the Rust compiler knows that a value will never change, it may be able to make better decisions while compiling the program.

This can help produce efficient machine code.

---

# Mutable Variables

Sometimes, we intentionally need to change the value of a variable.

For example:

- Increasing a counter
- Updating a score
- Changing a user's balance
- Adding items to a list
- Updating a player's position

To make a variable mutable, we use the `mut` keyword.

`mut` stands for **mutable**.

### Syntax

```rust
let mut variable_name: data_type = value;
```

Example:

```rust
fn main() {
    let mut x: i32 = 1;

    x = 2;

    println!("{}", x);
}
```

Output:

```
2
```

Here:

```rust
let mut x: i32 = 1;
```

creates a mutable variable.

Because the `mut` keyword is present, Rust allows:

```rust
x = 2;
```

The value changes successfully:

```
Initial value:

x = 1

     ↓

Updated value:

x = 2 ✅
```

---

# Immutable vs Mutable Variables

| Immutable Variable | Mutable Variable |
| --- | --- |
| Created using `let` | Created using `let mut` |
| Value cannot be changed | Value can be changed |
| Default behaviour in Rust | Must be explicitly requested |
| Safer and more predictable | Useful when values need to change |

Example:

```rust
let age = 21;
```

`age` is immutable.

```rust
let mut score = 0;
```

`score` is mutable.

---

# Example: Updating a Score

```rust
fn main() {
    let mut score: i32 = 0;

    println!("Initial score: {}", score);

    score = 10;

    println!("Updated score: {}", score);
}
```

Output:

```
Initial score: 0
Updated score: 10
```

The variable must be mutable because its value changes from `0` to `10`.

---

# Example: Counter in a Loop

Mutable variables are commonly used with loops.

```rust
fn main() {
    let mut count = 1;

    while count <= 5 {
        println!("{}", count);

        count += 1;
    }
}
```

Output:

```
1
2
3
4
5
```

Here:

```rust
let mut count = 1;
```

creates a mutable variable because its value needs to change during every iteration.

```rust
count += 1;
```

is a shorter way of writing:

```rust
count = count + 1;
```

Without `mut`, Rust would not allow the value of `count` to be updated.

---

# Mutability Does Not Mean Changing the Data Type

A mutable variable can change its **value**, but its data type must remain the same.

This is valid:

```rust
fn main() {
    let mut number: i32 = 10;

    number = 20;

    println!("{}", number);
}
```

Both values are integers:

```
10 → i32

20 → i32
```

Therefore, the code works.

---

This is not valid:

```rust
fn main() {
    let mut value: i32 = 10;

    value = "Rust";
}
```

The variable was created as:

```
i32
```

but we tried to assign:

```
&str
```

Rust does not allow the type of a variable to change just because the variable is mutable.

> `mut` allows the value to change, not the data type.
> 

---

# Mutability with a `String`

Mutability also allows us to modify a `String`.

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

Initially:

```
Kuldeep
```

After:

```rust
name.push_str(" Sharma");
```

the value becomes:

```
Kuldeep Sharma
```

The variable must be declared with `mut` because the contents of the `String` are being modified.

---

# JavaScript `const` vs Rust Immutability

Rust's immutable variables may initially look similar to JavaScript's `const`, but they are not exactly the same.

Consider this JavaScript code:

```jsx
const numbers = [10, 20, 30];

numbers.push(40);

console.log(numbers);
```

Output:

```
[10, 20, 30, 40]
```

Even though `numbers` was created using `const`, JavaScript allows the contents of the array to be modified.

However, JavaScript does not allow the variable to be reassigned:

```jsx
const numbers = [10, 20, 30];

numbers = [100, 200];
```

This produces an error.

Therefore, JavaScript's `const` mainly prevents **reassignment of the variable binding**. It does not automatically make the contents of an array or object immutable.

---

In Rust:

```rust
fn main() {
    let numbers = vec![10, 20, 30];

    numbers.push(40);
}
```

Rust produces an error because `numbers` is immutable.

To modify the vector, we must explicitly use `mut`:

```rust
fn main() {
    let mut numbers = vec![10, 20, 30];

    numbers.push(40);

    println!("{:?}", numbers);
}
```

Output:

```
[10, 20, 30, 40]
```

Rust requires us to clearly state that the value is intended to change:

```rust
let mut numbers = vec![10, 20, 30];
```

---

## JavaScript `const` vs Rust `let`

| JavaScript `const` | Rust `let` |
| --- | --- |
| Prevents reassignment | Creates an immutable variable |
| Object or array contents may still change | Value generally cannot be mutated through that variable |
| Mutation may still be possible | Mutation requires explicit `mut` |

> JavaScript's `const` and Rust's immutable variables are related ideas, but they do not behave exactly the same way.
> 

---

# Important: Mutability Is Explicit in Rust

Rust requires developers to clearly indicate when a variable can change.

Immutable:

```rust
let score = 100;
```

Mutable:

```rust
let mut score = 100;
```

The `mut` keyword communicates the programmer's intention:

> “The value of this variable may change later.”
> 

This makes changes easier to notice while reading the code.

---

# Quick Summary

| Concept | Meaning |
| --- | --- |
| Immutable | Value cannot be changed after assignment |
| Mutable | Value can be changed after assignment |
| `let` | Creates an immutable variable by default |
| `let mut` | Creates a mutable variable |
| `mut` | Stands for mutable |
| Default in Rust | Variables are immutable |
| Benefit | Safer and more predictable code |

Immutable variable:

```rust
let x = 1;

// x = 2; ❌ Error
```

Mutable variable:

```rust
let mut x = 1;

x = 2; // ✅ Allowed
```

> Rust variables are immutable by default. When a value needs to change, we must explicitly make the variable mutable using the `mut` keyword.
>