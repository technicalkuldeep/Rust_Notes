# Chapter 4 — The Rules of Borrowing (Why Rust Is So Strict 😂❤️)

In the previous chapters, we learned:

- 🤝 **Immutable References (`&`)** → Borrowing only to read.
- ❤️🔥 **Mutable References (`&mut`)** → Borrowing to modify.

But Rust has some very strict rules about borrowing.

At first, these rules may feel annoying.

You might think:

> "Why is Rust stopping me? I know what I'm doing!"
> 

😂

But these rules exist to prevent some of the most difficult bugs in programming.

Let's understand them one by one using Rihanna's story.

---

# Rule 1 — Many Immutable References Are Allowed 🤝

Suppose Rihanna says,

> "Today I'm free."
> 

"My friends can come and spend time with me."

They can

- Have coffee ☕
- Watch movies 🎬
- Talk 📞
- Take selfies 🤳

But...

No one is allowed to change her.

Everybody is simply hanging out.

This is perfectly safe.

```
                🤝 Rahul

Owner ❤️ Rihanna 🤝 Aman

                🤝 Priya

                🤝 Ankit
```

Nobody is changing Rihanna.

Everyone is simply reading.

Rust also allows this.

---

## Example

```rust
fn main() {
    let s1 = String::from("Hello");

    let s2 = &s1;
    let s3 = &s1;

    println!("{}", s1);
    println!("{}", s2);
    println!("{}", s3);
}
```

Output

```
Hello
Hello
Hello
```

---

## Understanding the Code

First,

```rust
let s1 = String::from("Hello");
```

creates the owner.

Then

```rust
let s2 = &s1;
```

creates the first immutable reference.

Then

```rust
let s3 = &s1;
```

creates another immutable reference.

Now the memory conceptually looks like

```
             Owner
              │
              ▼
           "Hello"
            ▲   ▲
            │   │
          s2    s3
```

There is still

✅ One owner

✅ Multiple readers

No problem.

---

## Why Is This Safe?

Because nobody can modify the String.

Everyone is only reading it.

If 100 people read the same book simultaneously...

Nothing changes.

The book remains exactly the same.

Reading is always safe.

---

# Rule 2 — Only One Mutable Reference Is Allowed ❤️🔥

Now suppose Rihanna says,

> "Today one person is allowed to give me a makeover."
> 

💇‍♀️

That person can

- Change her hairstyle
- Buy new clothes
- Change her favourite colour

Basically,

modify her.

Rust allows this.

But...

Only **ONE** person.

---

## Example

```rust
fn main() {
    let mut s1 = String::from("Hello");

    let s2 = &mut s1;

    update_word(&mut s1);

    println!("{}", s2);
}

fn update_word(word: &mut String) {
    word.push_str(" World");
}
```

This produces an error.

---

## Why?

Look carefully.

This line

```rust
let s2 = &mut s1;
```

already created one mutable reference.

Then this line

```rust
update_word(&mut s1);
```

tries to create another mutable reference.

Now two different borrowers are trying to modify the same String.

Rust immediately says

> ❌ Not allowed.
> 

---

## Rihanna Story 😂

Imagine this.

```
Owner ❤️ Rihanna

Rahul ❤️🔥 Rihanna
```

Rahul starts giving Rihanna a makeover.

Suddenly...

Aman arrives.

😂

He says

> "Wait!!
> 

I also want to change her hairstyle."

Now both start working.

Rahul says

> Black Hair
> 

Aman says

> Blonde Hair
> 

Rahul says

> Blue Dress
> 

Aman says

> Red Dress
> 

😂😂

Chaos.

Rihanna immediately stops both of them.

> **"Only ONE makeover artist!"**
> 

Rust follows exactly the same rule.

---

# Rule 3 — Mutable and Immutable References Cannot Exist Together

Now imagine this situation.

```
Owner ❤️ Rihanna

Rahul ❤️🔥 Rihanna

Priya 🤝 Rihanna
```

Rahul is changing Rihanna.

Meanwhile...

Priya says

> "Let's take a selfie."
> 

😂

But...

Which version of Rihanna will appear?

Before makeover?

After makeover?

Halfway through?

Nobody knows.

This creates inconsistent behaviour.

Rust simply says

> **NO.**
> 

---

## Example

```rust
fn main() {
    let mut s1 = String::from("Hello");

    let s2 = &mut s1;

    let s3 = &s1;

    println!("{}", s2);
    println!("{}", s3);
}
```

Rust gives an error.

---

## Why?

This line

```rust
let s2 = &mut s1;
```

allows modifications.

Then

```rust
let s3 = &s1;
```

tries to create an immutable reference.

Rust refuses.

Because one borrower wants to modify the data...

while another borrower expects the data to remain unchanged.

---

## Rihanna Story 😂

Friend says

> Rihanna...
> 

What's your favourite colour?

She replies

> Blue.
> 

One second later...

FWB changes it.

😂

Friend asks again.

Now Rihanna says

> Red.
> 

Friend becomes confused.

> "Wait...
> 

Didn't you just say Blue?"

😂😂

Rust prevents this entire situation.

---

# Why Does Rust Enforce These Rules?

These rules are not random.

They solve real programming problems.

---

## Problem 1 — Inconsistent Behaviour

Suppose one part of your program reads

```
Balance = ₹5000
```

At the exact same time,

another part changes it to

```
Balance = ₹1000
```

Different parts of the program now see different values.

This can produce unpredictable results.

---

## Problem 2 — Data Race

A **Data Race** happens when multiple parts of a program try to access the same data simultaneously, and at least one of them is modifying it.

For example,

Imagine two threads trying to update the same bank account balance at the same time.

Thread 1

```
Balance = ₹5000

Withdraw ₹1000
```

Thread 2

```
Balance = ₹5000

Withdraw ₹2000
```

If both operations happen simultaneously without proper synchronization, the final balance may become incorrect.

Rust prevents many such problems at compile time by enforcing strict borrowing rules.

> We'll study threads and concurrency later, but these borrowing rules are one of the reasons Rust is considered highly reliable for concurrent programming.
> 

---

# The Three Borrowing Rules

| Rule | Allowed? |
| --- | --- |
| Many Immutable References (`&`) | ✅ Yes |
| One Mutable Reference (`&mut`) | ✅ Yes |
| Mutable + Immutable Together | ❌ No |
| Two Mutable References Together | ❌ No |

---

# Easy Way to Remember 😂

Imagine Rihanna's calendar.

### Friends Day ☕

```
🤝 Rahul

🤝 Aman

🤝 Priya

🤝 Ankit
```

Everyone can come.

No problem.

---

### Makeover Day 💇‍♀️

```
❤️🔥 Rahul
```

Only one person.

---

### Forbidden Day 🚫

```
🤝 Rahul

❤️🔥 Aman
```

Not allowed.

---

### Also Forbidden 🚫

```
❤️🔥 Rahul

❤️🔥 Aman
```

Not allowed.

---

# Why Rust Is Loved ❤️

Many programming languages allow these mistakes.

The bugs only appear while the program is running.

Rust takes a different approach.

Instead of letting the program crash later,

Rust says

> "I'm not even going to compile this code."
> 

😂

That's why many Rust bugs are caught **before the program is ever executed**.

---

# Quick Summary

| Borrow Type | Can Read | Can Modify | How Many Allowed? |
| --- | --- | --- | --- |
| Immutable (`&`) | ✅ Yes | ❌ No | Many |
| Mutable (`&mut`) | ✅ Yes | ✅ Yes | Only One |

---

# Chapter Summary

Rust's borrowing rules ensure that data remains safe and consistent throughout the program.

The rules are:

1. ✅ You can have **many immutable references** at the same time because reading data is always safe.
2. ✅ You can have **only one mutable reference** at a time because only one borrower should be allowed to modify the data.
3. ❌ You cannot mix immutable and mutable references because readers expect the data to remain unchanged while they are using it.

These rules help Rust prevent **data races**, **inconsistent behaviour**, and many difficult bugs **at compile time**, long before the program is run.

---

## What's Next?

We've now learned:

- ✅ Ownership
- ✅ References
- ✅ Borrowing
- ✅ Mutable References
- ✅ Borrowing Rules

The next topic introduces two advanced concepts that build on borrowing:

1. **Lifetimes** – How long is someone allowed to borrow Rihanna? 😂
2. **String Slices (`&str`)** – What if someone only wants to borrow a small part of the String instead of the entire String?

We'll study both of these in the upcoming sections.