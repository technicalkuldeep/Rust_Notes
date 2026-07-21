# Chapter 2 — References (&) (Borrowing Without a Breakup 😂❤️)

In the previous chapter, we learned Borrowing using Rihanna's story.

Now let's see how Rust actually implements it.

This chapter introduces one of the most commonly used symbols in Rust:

```rust
&
```

This symbol creates a **Reference**.

---

# What is a Reference?

A **Reference** is simply the **address of a value**, not the value itself.

Instead of giving someone ownership of your data,

you simply tell them

> **"Here's where it lives."**
> 

Think of it like this.

Suppose your house is located at

```
House

221B Baker Street
```

Instead of giving your friend your entire house...

you simply give him the address.

```
House

221B Baker Street

          ▲

      Address
```

Your friend knows where the house is.

He can visit it.

But...

he does **not** become the owner.

References work exactly like that.

---

# Ownership vs Reference

Ownership

```rust
let s2 = s1;
```

means

> "Take Rihanna."
> 

Reference

```rust
let s2 = &s1;
```

means

> "You can spend time with Rihanna...
> 

but she still belongs to me."

😂

No breakup.

---

# Creating a Reference

Example

```rust
fn main() {
    let s1 = String::from("Hello");

    let s2 = &s1;

    println!("{}", s1);

    println!("{}", s2);
}
```

Output

```
Hello
Hello
```

Notice something.

Both

```rust
println!("{}", s1);
```

and

```rust
println!("{}", s2);
```

work perfectly.

Unlike Ownership,

nothing became invalid.

---

# Understanding the Code

### Step 1

```rust
let s1 = String::from("Hello");
```

A String is created.

Memory looks like this.

```
STACK                          HEAP

+-------------------+
| s1                |──────────────► "Hello"
| ptr               |
| len = 5           |
| cap = 5           |
+-------------------+
```

Currently,

**s1 owns the String.**

---

### Step 2

```rust
let s2 = &s1;
```

This line **does not copy** the String.

It also **does not move ownership**.

Instead,

Rust creates a **Reference**.

Think of `s2` as simply another arrow pointing to the same String.

```
STACK

+--------------------+
| s1 (Owner)         |────────────┐
+--------------------+            │
                                  ▼
                              +---------+
                              | Hello   |
                              +---------+
                                  ▲
                                  │
+--------------------+            │
| s2 (Reference)     |────────────┘
+--------------------+
```

Notice

There is still only **ONE owner**.

The owner is

```
s1
```

while

```
s2
```

is only a borrower.

---

# Rihanna Story 😂

Imagine this.

```
s1 ❤️ Rihanna
```

Now

```rust
let s2 = &s1;
```

does NOT mean

```
s2 ❤️ Rihanna
```

Instead,

it means

```
s1 ❤️ Rihanna

s2 🤝 Rihanna
```

They're just friends.

No breakup happened.

😂

---

# Why Can We Still Use `s1`?

Remember Ownership?

When we wrote

```rust
let s2 = s1;
```

Rihanna left.

```
s1 💔

s2 ❤️ Rihanna
```

So

```rust
println!("{}", s1);
```

gave an error.

But now

```rust
let s2 = &s1;
```

creates only a friendship.

Ownership never changes.

Therefore

```rust
println!("{}", s1);
```

still works.

---

# References Don't Own Data

This is one of the most important ideas in Rust.

A Reference

- does not own the value.
- does not free memory.
- only points to someone else's data.

Think of it as

```
Owner

↓

House

Reference

↓

Address of House
```

The address is useful.

But the address is **not** the house.

Similarly,

A Reference is **not** the owner.

---

# Borrowing in Functions

Now let's solve the biggest problem from the Ownership chapter.

Previously we wrote

```rust
fn main() {

    let my_string = String::from("Hello, Rust!");

    takes_ownership(my_string);

    println!("{}", my_string);
}

fn takes_ownership(some_string: String) {

    println!("{}", some_string);

}
```

This produced an error.

Why?

Because ownership moved into the function.

---

Instead,

let's borrow the String.

```rust
fn main() {

    let my_string = String::from("Hello, Rust!");

    takes_ownership(&my_string);

    println!("{}", my_string);

}

fn takes_ownership(some_string: &String) {

    println!("{}", some_string);

}
```

Output

```
Hello, Rust!

Hello, Rust!
```

Now everything works.

---

# Understanding the Code

### Step 1

```rust
let my_string = String::from("Hello, Rust!");
```

The owner is

```
my_string ❤️ Rihanna
```

---

### Step 2

```rust
takes_ownership(&my_string);
```

Notice

```rust
&
```

before

```rust
my_string
```

We are **not** passing the String.

We are passing a **Reference**.

Think of the function saying

> "Can I borrow Rihanna for 5 minutes?"
> 

😂

---

### Step 3

The function parameter is

```rust
fn takes_ownership(some_string: &String)
```

Notice

```rust
&String
```

This means

> "I don't want ownership."
> 

> "I'm only borrowing the String."
> 

The parameter

```
some_string
```

becomes a borrower.

Not an owner.

---

# Rihanna Story 😂

Initially

```
my_string ❤️ Rihanna
```

Function call

```rust
takes_ownership(&my_string);
```

becomes

```
my_string ❤️ Rihanna

Function 🤝 Rihanna
```

The function simply spends some time with Rihanna.

😂

When the function finishes

```
Function leaves.

my_string ❤️ Rihanna
```

Nobody broke up.

Nobody lost ownership.

---

# Memory View

Before function call

```
STACK

my_string (Owner)
        │
        ▼
      "Hello"
```

During function call

```
STACK

my_string (Owner)
        │
        ▼
      "Hello"
        ▲
        │
some_string (Reference)
```

Notice

Both point to the same String.

Only one owner exists.

---

After the function finishes

```
STACK

my_string (Owner)
        │
        ▼
      "Hello"
```

The borrowed reference disappears.

The owner remains.

---

# Why Is This Better Than Ownership?

Without References

```rust
takes_ownership(s1);

s1 = takes_ownership(s1);

clone();
```

Ownership constantly changes.

😂

With References

```rust
takes_ownership(&s1);
```

The owner never changes.

No cloning.

No moving.

No returning ownership.

Much simpler.

---

# Important Difference

Ownership

```rust
let s2 = s1;
```

Result

```
Ownership moves.
```

Reference

```rust
let s2 = &s1;
```

Result

```
Ownership stays.

Only borrowing happens.
```

---

# Ownership vs Borrowing

| Ownership | Borrowing |
| --- | --- |
| Owner changes | Owner stays the same |
| Original variable becomes invalid | Original variable remains valid |
| Uses `String` | Uses `&String` |
| May require returning ownership | No need to return ownership |

---

# Why Does Rust Use `&`?

The `&` symbol tells Rust:

> "Don't give me the actual value."
> 

> **"Just give me its address."**
> 

That's why a Reference is often described as

> **A pointer (address) to a value that someone else owns.**
> 

---

# Chapter Summary

- A **Reference** is the **address** of a value, not the value itself.
- The `&` operator creates a reference.
- Creating a reference does **not** transfer ownership.
- References allow multiple parts of a program to **read** the same data while keeping a single owner.
- Passing `&String` to a function lets the function borrow the String instead of owning it.
- After the function finishes, the owner still has full access to the value because ownership was never transferred.

---

## What's Next?

So far, borrowers could only **look at Rihanna**. 😂

They could talk, have coffee, and watch movies—but they **couldn't change** anything.

What if Rihanna says:

> "Okay... one trusted friend is allowed to give me a makeover."
> 

💇‍♀️✨

In Rust, this is called a **Mutable Reference (`&mut`)**, and that's what we'll learn in the next chapter.