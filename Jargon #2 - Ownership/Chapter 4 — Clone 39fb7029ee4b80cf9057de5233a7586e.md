# Chapter 4 — Clone

In the previous chapter, we learned that whenever Rihanna finds a new boyfriend...

The old boyfriend loses ownership.

```
s1 ❤️ Rihanna

↓

let s2 = s1;

↓

s1 💔

s2 ❤️ Rihanna
```

Poor **s1** is now the ex-boyfriend.

😂

But what if...

**s1 doesn't want to lose Rihanna?**

He says,

> "I don't want to give her away."
> 

> "Can we make another Rihanna instead?"
> 

Believe it or not...

Rust says,

> **"Yes."**
> 

😂

That's exactly what **`clone()`** does.

---

# What is `clone()`?

`clone()` creates a **completely new copy** of heap data.

Instead of transferring ownership,

Rust creates **another independent object**.

Think of it as creating:

> **Rihanna's identical twin sister.** 👯‍♀️
> 

Now both people can have their own girlfriend.

No fights.

No breakups.

---

# Example

```rust
fn main() {

    let s1 = String::from("hello");

    let s2 = s1.clone();

    println!("{}", s1);

    println!("{}", s2);

}
```

Output

```
hello
hello
```

Unlike before,

this compiles successfully.

---

# What Actually Happened?

Initially

```
s1 ❤️ Rihanna
```

After

```rust
let s2 = s1.clone();
```

Rust creates

```
Rihanna's Twin Sister
```

Now

```
s1 ❤️ Rihanna

s2 ❤️ Twin Rihanna
```

Both are completely independent.

Nobody becomes the ex.

😂

---

# Memory View

Before clone

```
STACK                           HEAP

s1  ───────────────►  "hello"
```

After clone

```
STACK                           HEAP

s1 ───────────────► "hello"

s2 ───────────────► "hello"
```

Notice something.

There are now **two different heap allocations**.

Rust actually copied the data.

This is called a

> **Deep Copy**
> 

---

# Deep Copy

A **Deep Copy** means

> Copy the actual data into a completely new memory location.
> 

Before

```
Heap

"hello"
```

After clone

```
Heap

"hello"

"hello"
```

There are now two independent strings.

Changing one does **not** affect the other.

---

# Example

```rust
fn main() {

    let mut s1 = String::from("Hello");

    let mut s2 = s1.clone();

    s2.push_str(" Rust");

    println!("{}", s1);

    println!("{}", s2);

}
```

Output

```
Hello

Hello Rust
```

Notice

Only **s2** changed.

Because

they no longer share the same memory.

---

# Why Doesn't Rust Clone Automatically?

Good question.

Suppose we write

```rust
let s2 = s1;
```

Rust **could** automatically create a copy.

Why doesn't it?

Because copying heap data can be expensive.

Imagine

```
String size

5 bytes
```

Copying it is easy.

Now imagine

```
500 MB
```

or

```
2 GB
```

Automatically copying huge amounts of memory every time would make programs much slower.

Instead,

Rust says

> "If you really want another copy...
> 

tell me explicitly."

That's why we write

```rust
.clone()
```

The programmer decides when copying should happen.

---

# Move vs Clone

This is one of the most important differences in Rust.

## Move

```rust
let s2 = s1;
```

Ownership changes.

```
Before

s1 ❤️ Rihanna
```

↓

```
After

s1 💔

s2 ❤️ Rihanna
```

Only one owner exists.

---

## Clone

```rust
let s2 = s1.clone();
```

Ownership does **not** move.

Instead,

Rust creates another object.

```
s1 ❤️ Rihanna

s2 ❤️ Twin Rihanna
```

Now both variables are valid.

---

# Numbers Don't Need clone()

Remember integers?

```rust
let x = 10;

let y = x;
```

This already creates a copy.

No need to write

```rust
x.clone()
```

Why?

Because integers are stored on the stack.

They are very small.

Copying

```
4 bytes
```

is extremely cheap.

So Rust copies them automatically.

---

# Strings Are Different

A String lives on the heap.

Copying it means

- allocating new memory
- copying every character
- creating another heap object

This is much more expensive.

Therefore,

Rust makes us explicitly call

```rust
.clone()
```

---

# Copy vs Clone

| Copy | Clone |
| --- | --- |
| Automatic | Manual |
| Used for small stack data | Used for heap data |
| Very fast | Can be expensive |
| No extra heap allocation | Creates a new heap allocation |

Examples

```rust
let x = 10;

let y = x;
```

Automatic Copy ✅

---

```rust
let s1 = String::from("Hello");

let s2 = s1.clone();
```

Deep Copy (Clone) ✅

---

# Should We Use clone() Everywhere?

No.

Many beginners think

> "I'll just clone everything."
> 

That works...

but it's usually a bad idea.

Every `clone()` creates another copy of the data.

More copies mean

- more memory usage
- more CPU work
- slower programs

Rust programmers try to avoid unnecessary cloning whenever possible.

Instead, they usually use

> **References (`&`)**
> 

which let multiple parts of a program **use** the same data without copying it.

We'll learn that in the next chapter.

---

# Rihanna Story 😂

Move

```
s1 ❤️ Rihanna

↓

s2 steals Rihanna

↓

s1 💔
```

Clone

```
s1 ❤️ Rihanna

↓

Rust creates Twin Rihanna 👯‍♀️

↓

s2 ❤️ Twin Rihanna
```

Nobody loses their girlfriend.

Everybody is happy.

😂

---

# Chapter Summary

- `clone()` creates a **deep copy** of heap data.
- Both variables become completely independent owners.
- Unlike a move, ownership is **not transferred**.
- `clone()` allocates new memory on the heap and copies the data.
- Cloning is more expensive than copying small stack values, so it should be used only when needed.

---

## Next Chapter (Final Ownership Chapter)

We'll answer the biggest question so far:

> **Why create Rihanna's twin every time?**
> 

What if **s1** just wants his friend (the function) to **talk to Rihanna for a while**, without taking her away and without creating a twin?

That is exactly what **References (`&`)** and **Borrowing** solve.

It's arguably the coolest concept in Rust, and once you understand it, Ownership becomes much more intuitive.