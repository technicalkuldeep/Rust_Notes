# 14. Implementing Structs (Methods)

In the previous chapter, we learned that a **Struct** groups related data together.

For example,

```rust
struct Rect {
    width: u32,
    height: u32,
}
```

This Struct only stores information.

It knows

- Width
- Height

But...

it doesn't know **how to calculate its area**.

For that, Rust provides something called an

> **`impl` block.**
> 

---

# What is an `impl` Block?

An `impl` block lets us **attach functions to a Struct.**

These attached functions are called

> **Methods**
> 

Think of it like this:

```
Struct

↓

Stores Data
```

```
impl

↓

Defines Behaviour
```

A Struct answers

> **"What information do I have?"**
> 

An `impl` block answers

> **"What can I do?"**
> 

---

# Syntax

```rust
struct Rect {
    width: u32,
    height: u32,
}

impl Rect {

    fn area(&self) -> u32 {
        self.width * self.height
    }

}
```

Here,

```rust
area()
```

is a **method** of the `Rect` Struct.

Every rectangle automatically gets this method.

---

# Complete Example

```rust
struct Rect {
    width: u32,
    height: u32,
}

impl Rect {

    fn area(&self) -> u32 {
        self.width * self.height
    }

}

fn main() {

    let rect = Rect {
        width: 30,
        height: 50,
    };

    println!("Area = {}", rect.area());

}
```

Output

```
Area = 1500
```

---

# Understanding the Code

## Step 1

Create the Struct.

```rust
struct Rect {
    width: u32,
    height: u32,
}
```

This only creates the blueprint.

Nothing more.

---

## Step 2

Implement methods.

```rust
impl Rect {

}
```

Everything inside this block belongs to

```rust
Rect
```

---

## Step 3

Create a method.

```rust
fn area(&self) -> u32
```

Let's understand this line.

---

### `fn area`

This is simply the method name.

```rust
area
```

---

### `&self`

This is the most important part.

```rust
&self
```

means

> "Use the current object."
> 

Think of

```rust
self
```

as

> **"Me."**
> 

😂

If we have

```rust
let rect = Rect {
    width: 30,
    height: 50,
};
```

then inside the method,

```rust
self
```

is exactly the same as

```rust
rect
```

So

```rust
self.width
```

means

```rust
rect.width
```

and

```rust
self.height
```

means

```rust
rect.height
```

---

### Why `&self` and not `self`?

Notice the `&`.

```rust
&self
```

This means

> Borrow the object.
> 

We are **not taking ownership**.

We only want to read its data.

This should immediately remind you of Borrowing.

😂

---

# Calculating the Area

```rust
self.width * self.height
```

For our rectangle

```
Width = 30

Height = 50
```

Area becomes

```
30 × 50 = 1500
```

---

# Calling a Method

Instead of writing

```rust
area(rect);
```

Rust lets us write

```rust
rect.area();
```

This is called

> **Method Syntax**
> 

It reads much more naturally.

```
Rectangle

↓

Calculate Area
```

---

# Real-Life Analogy

Imagine a TV.

The TV has data.

```
TV

Brand

Size

Volume

Channel
```

These are the Struct fields.

Now imagine the TV also has buttons.

```
Increase Volume

Decrease Volume

Change Channel

Turn Off
```

These are methods.

The TV stores information,

and the buttons define what the TV can do.

Similarly,

```
Rect

Width

Height
```

stores data,

while

```
area()
```

defines behaviour.

---

# Struct vs impl

| Struct | impl |
| --- | --- |
| Stores data | Defines behaviour |
| Fields | Methods |
| What it has | What it can do |

---

# Why Use Methods?

Without methods,

we would write

```rust
fn area(rect: &Rect) -> u32 {
    rect.width * rect.height
}
```

and call

```rust
area(&rect);
```

With methods,

we simply write

```rust
rect.area();
```

This makes the code

- Cleaner
- Easier to read
- More organized

---

# A Quick Note About `self`

You'll often see three versions inside an `impl` block:

```rust
&self
```

Borrow the object (read-only).

---

```rust
&mut self
```

Borrow the object and allow modifications.

---

```rust
self
```

Take ownership of the object.

For now, you'll mostly use

```rust
&self
```

We'll explore the other two as we build more Rust programs.

---

# Chapter Summary

- An **`impl` block** lets us attach **methods** to a Struct.
- A **Struct** stores data, while an **`impl` block** defines the behaviour of that data.
- Methods are called using the **dot (`.`)** syntax, such as `rect.area()`.
- The `&self` parameter refers to the current instance of the Struct and borrows it without taking ownership.
- Using methods makes code cleaner, more organized, and follows the object-oriented style found in many languages, while still fitting Rust's ownership model.

---

### 💡 Easy Formula to Remember

```
Struct
     ↓
Stores Data

impl
     ↓
Adds Behaviour

Object
     ↓
Uses Both
```

This simple mental model is enough to understand **90% of beginner Rust code involving structs**.