# Chapter 3 — Mutable References (&mut) — Rihanna's Makeover 😂❤️🔥

In the previous chapter, we learned that people can **borrow Rihanna**.

But there was one condition.

They could:

- Talk ☕
- Watch movies 🎬
- Go shopping 🛍️
- Take selfies 🤳

But...

> **NO HANKY-PANKY.**
> 

😂

In Rust terms,

they could only **read** the data.

They were **not allowed to change it.**

---

# But What If Someone Wants to Change Rihanna?

Suppose Rihanna says,

> "I'm thinking of getting a makeover."
> 

💇‍♀️

Maybe she wants to

- Change her hairstyle 💇
- Change her favourite colour 🎨
- Change her phone wallpaper 📱
- Buy new clothes 👗

In programming,

this means

> **Changing the value of a variable.**
> 

A normal reference cannot do this.

For that, Rust provides something special.

> **Mutable References**
> 

---

# What is a Mutable Reference?

A **Mutable Reference** is a reference that allows the borrower to **modify** the original value.

Its syntax is

```rust
&mut
```

Think of it as:

> "I'm not becoming the owner..."
> 

> "But Rihanna has given me permission to change her."
> 

😂

---

# Example

```rust
fn main() {

    let mut s1 = String::from("Hello");

    update_word(&mut s1);

    println!("{}", s1);

}

fn update_word(word: &mut String) {

    word.push_str(" World");

}
```

Output

```
Hello World
```

---

# Understanding the Code

## Step 1

```rust
let mut s1 = String::from("Hello");
```

Notice

```rust
mut
```

The String itself is mutable.

This means

> The owner allows the String to be modified.
> 

Without `mut`, Rust will not allow anyone to change it.

---

## Step 2

```rust
update_word(&mut s1);
```

This line creates a **Mutable Reference**.

Notice the syntax.

```rust
&mut s1
```

There are two keywords.

```
&
```

means

Borrow.

and

```
mut
```

means

Borrow with permission to modify.

---

## Step 3

The function receives

```rust
fn update_word(word: &mut String)
```

Notice

```rust
&mut String
```

This means

> "I'm borrowing the String..."
> 

> "...and I'm allowed to modify it."
> 

---

## Step 4

Inside the function

```rust
word.push_str(" World");
```

The function adds

```
 World
```

to the original String.

Before

```
Hello
```

After

```
Hello World
```

---

# Rihanna Story 😂

Initially

```
Owner ❤️ Rihanna
```

Rihanna says

> "Today my trusted friend can give me a makeover."
> 

😂

The borrower changes

- Hairstyle 💇
- Dress 👗
- Makeup 💄

When Rihanna returns...

The owner immediately notices

😂

```
Before

❤️ Rihanna
```

↓

Borrower gives makeover

↓

```
❤️ Glamorous Rihanna ✨
```

The owner sees the updated Rihanna because the borrower modified the **same person**, not a copy.

---

# Memory View

Initially

```
STACK

s1 (Owner)
      │
      ▼
   "Hello"
```

During mutable borrowing

```
STACK

s1 (Owner)
      │
      ▼
   "Hello"
      ▲
      │
 word (&mut)
```

The function changes the same String.

After

```
STACK

s1 (Owner)
      │
      ▼
 "Hello World"
```

Notice

The owner never changed.

Only the data changed.

---

# Why Do We Need `mut` Twice?

Many beginners ask this question.

Why do we write

```rust
let mut s1
```

and also

```rust
&mut s1
```

Why not just one `mut`?

Because they mean two different things.

---

## First `mut`

```rust
let mut s1
```

means

> The owner allows this value to change.
> 

Without this,

the String is permanently read-only.

---

## Second `mut`

```rust
&mut s1
```

means

> This borrower has permission to modify it.
> 

Imagine Rihanna saying

> "Yes...
> 

I'm okay with changing."

😂

Then she tells one friend

> "You're the one allowed to give me the makeover."
> 

These are two different permissions.

---

# What Happens Without `mut`?

Suppose we write

```rust
fn main() {

    let s1 = String::from("Hello");

    update_word(&mut s1);

}
```

Rust immediately complains.

Why?

Because

```rust
s1
```

was never declared mutable.

The owner never gave permission for changes.

---

Likewise,

this is also wrong.

```rust
fn update_word(word: &String) {

    word.push_str(" World");

}
```

Here

```rust
&String
```

is an **immutable reference**.

Borrowers are only allowed to look.

Not modify.

Rust gives an error.

---

# The Biggest Rule

Now imagine this.

```rust
fn main() {

    let mut s1 = String::from("Hello");

    let s2 = &mut s1;

    update_word(&mut s1);

    println!("{}", s2);

}
```

Will this compile?

**No.**

Rust gives an error.

---

# Rihanna Story 😂

Imagine Rihanna says

> "Today Rahul is giving me a makeover."
> 

💇

Suddenly another person says

> "Wait!
> 

I'm also going to change her hairstyle."

😂😂

Now both start changing things simultaneously.

One says

> "Black hair."
> 

Another says

> "Blonde hair."
> 

One says

> "Wear blue."
> 

Another says

> "Wear red."
> 

Chaos.

😂

Rihanna immediately says

> **"ONLY ONE MAKEOVER ARTIST!"**
> 

---

Rust says exactly the same thing.

There can be

> **Only ONE Mutable Reference at a time.**
> 

---

# Why?

Because if multiple people modify the same value simultaneously,

the program may produce unpredictable results.

Rust prevents this problem before the program even runs.

This is one of the biggest reasons Rust is considered memory-safe.

---

# Mutable Reference Rules So Far

| Borrow Type | Allowed? |
| --- | --- |
| One mutable reference | ✅ Yes |
| Two mutable references together | ❌ No |

We'll learn the complete borrowing rules in the next chapter.

---

# Immutable vs Mutable References

| Immutable Reference (`&`) | Mutable Reference (`&mut`) |
| --- | --- |
| Can only read data | Can read and modify data |
| Multiple allowed | Only one allowed |
| Does not change the value | Can change the value |

---

# Chapter Summary

- A **Mutable Reference (`&mut`)** allows a borrower to modify the original value.
- The owner must declare the value as mutable using `mut`.
- The borrower must also use `&mut` to receive permission to modify it.
- Changes made through a mutable reference affect the original value because both refer to the same data.
- Rust allows **only one mutable reference at a time** to prevent conflicting modifications and keep programs safe.

---

## What's Next?

We've learned:

- 🤝 **Immutable References (`&`)** — Many people can borrow Rihanna as friends.
- ❤️🔥 **Mutable References (`&mut`)** — One trusted person can give Rihanna a makeover.

But what happens if:

- Two friends borrow Rihanna at the same time? 🤝🤝
- One friend is borrowing her while someone is giving her a makeover? 🤝❤️🔥
- Two makeover artists arrive together? ❤️🔥❤️🔥

In the next chapter, we'll combine everything and learn **the complete Rules of Borrowing**, along with why Rust enforces these rules to prevent **data races** and **inconsistent behaviour**. 😂