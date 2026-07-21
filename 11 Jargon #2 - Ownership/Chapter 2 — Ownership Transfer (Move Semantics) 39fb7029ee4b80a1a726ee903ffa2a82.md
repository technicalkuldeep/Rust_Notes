# Chapter 2 — Ownership Transfer (Move Semantics)

In the previous chapter, we learned that every heap value in Rust has **exactly one owner**.

Now it's time to answer the most important question.

> **What happens if someone else wants to become the owner?**
> 

Let's continue our story...

---

# Rihanna Finds a New Boyfriend 😂❤️

Currently, Rihanna is happily dating **s1**.

```
s1 ❤️ Rihanna
```

Everything is going well.

One day, a new person named **s2** enters her life.

He says,

> "Hi Rihanna, I want to date you."
> 

Now remember Rihanna's rules:

✅ She always wants a boyfriend.

❌ She never dates two people at the same time.

❌ She doesn't believe in casual relationships.

Only one **serious relationship** is allowed.

So what happens?

She immediately breaks up with **s1**...

and starts dating **s2**.

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

Notice something.

Rihanna didn't create another copy of herself.

She simply **changed her owner**.

This is exactly what Rust does.

---

# Let's See It in Rust

```rust
fn main() {
    let s1 = String::from("hello");

    let s2 = s1;

    println!("{}", s2);
}
```

This program works perfectly.

Output

```
hello
```

Many beginners think this line

```rust
let s2 = s1;
```

copies the string.

**It does NOT.**

It transfers the ownership.

This process is called a **Move**.

---

# What Actually Happens?

Initially,

```rust
let s1 = String::from("hello");
```

Memory looks like this:

```
STACK                              HEAP

+------------------+          +----------------+
| s1               | -------> | h e l l o      |
| ptr              |          +----------------+
| len = 5          |
| cap = 5          |
+------------------+
```

Here,

**s1 owns the heap memory.**

---

Now this line executes.

```rust
let s2 = s1;
```

Rust **does not copy** the heap data.

Instead,

ownership moves from **s1** to **s2**.

```
STACK                              HEAP

+------------------+
| s1 ❌ Invalid    |
+------------------+

+------------------+          +----------------+
| s2               | -------> | h e l l o      |
| ptr              |          +----------------+
| len = 5          |
| cap = 5          |
+------------------+
```

The heap memory never moved.

Only the ownership changed.

---

# What Happens to s1?

Remember Rihanna.

She has already moved on.

Poor **s1** is now...

> **The Ex.** 💔😂
> 

He no longer has any relationship with Rihanna.

Similarly,

after ownership moves,

**s1 becomes invalid.**

---

So if we write

```rust
fn main() {
    let s1 = String::from("hello");

    let s2 = s1;

    println!("{}", s1);
}
```

Rust immediately says

```
error[E0382]: borrow of moved value: `s1`
```

In simple English,

Rust is saying

> "Sorry bro...
> 

> She's your ex now.
> 

> You don't own her anymore."
> 

😂😂

---

# Why Doesn't Rust Copy Automatically?

Imagine Rust allowed this.

```
s1 ❤️ Rihanna ❤️ s2
```

Now there are two owners.

Everything looks fine...

until one boyfriend breaks up.

Suppose **s1** says

> "Delete all our memories."
> 

Rust deletes the heap memory.

```
Heap Memory

❌ Deleted
```

But **s2** still thinks Rihanna is his girlfriend.

Now **s2** points to memory that no longer exists.

This creates a **Dangling Pointer**.

---

Or imagine both boyfriends say

> "Delete our memories."
> 

Now both try to delete the same memory.

First delete

✅ Success

Second delete

💥 Boom!

Memory has already been deleted.

This is called a

> **Double Free Error**
> 

These bugs are extremely dangerous.

Instead of trying to detect these problems later,

Rust completely prevents them.

Its rule is simple:

> **One owner.**
> 

No exceptions.

---

# Ownership Moves Only for Heap Data

This behaviour happens for heap-allocated data like:

- `String`
- `Vec`
- `HashMap`

Example

```rust
let s1 = String::from("Hello");
let s2 = s1;
```

Ownership moves.

---

# But Numbers Behave Differently

Look at this example.

```rust
fn main() {

    let x = 10;

    let y = x;

    println!("{}", x);

    println!("{}", y);
}
```

Output

```
10
10
```

Wait...

Didn't Ownership say only one owner?

Why is this allowed?

---

Because integers live on the **Stack**.

An `i32` is only **4 bytes**.

Copying 4 bytes is extremely cheap.

Instead of moving ownership,

Rust simply copies the value.

```
Before

x = 10
```

↓

```
After

x = 10

y = 10
```

Both variables now contain their own independent copy.

No heap memory exists.

No ownership transfer is required.

---

# Copy vs Move

This is one of the most important differences in Rust.

### Stack Data

```rust
let x = 10;

let y = x;
```

Rust performs

> **Copy**
> 

```
x = 10

↓

x = 10

y = 10
```

Both variables remain valid.

---

### Heap Data

```rust
let s1 = String::from("Hello");

let s2 = s1;
```

Rust performs

> **Move**
> 

```
Before

s1 ❤️ Hello
```

↓

```
After

s1 💔

s2 ❤️ Hello
```

Only **s2** remains valid.

---

# Why Is This Called Move Semantics?

The word **Move** simply means

> "Transfer ownership from one variable to another."
> 

It does **not** necessarily mean moving the actual heap data.

The heap memory usually stays where it is.

Only the ownership changes.

---

# Ownership Prevents Memory Bugs

Because only one owner exists,

Rust automatically prevents many dangerous bugs such as

- Dangling pointers
- Double free errors
- Use-after-free errors

All of these are caught **before the program even runs.**

That's one of Rust's biggest strengths.

---

# Poor s1 😂

Let's summarize Rihanna's love life.

Initially

```
s1 ❤️ Rihanna
```

Then

```
s2 enters...
```

Rihanna says

> "Sorry s1...
> 

I've moved on."

Now

```
s1 💔

s2 ❤️ Rihanna
```

If **s1** later sends her a message

```rust
println!("{}", s1);
```

Rust replies

> ❌ "Access denied."
> 

> "She's not your girlfriend anymore."
> 

😂

---

# But What If...

At this point, you might wonder:

> "What if **s1** doesn't want to lose Rihanna?"
> 

Or,

> "Can we make an identical twin of Rihanna?"
> 

Yes!

Rust provides a method called

```rust
clone()
```

which creates a completely new copy of the heap data.

Now both people can have their own Rihanna.

We'll study that in the next chapter. 😄