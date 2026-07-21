# Chapter 3 — Ownership in Functions

In the previous chapter, we saw Rihanna leave **s1** and start dating **s2**.

Poor **s1** became the ex-boyfriend. 💔😂

Now let's imagine a different situation.

Instead of Rihanna finding another boyfriend herself...

Suppose **s1** decides to introduce Rihanna to one of his friends.

That friend is...

> **A Function.**
> 

The question is:

> **When we pass a String to a function, who owns Rihanna now?**
> 

Let's find out.

---

# Passing Numbers to Functions

Let's first look at integers.

```rust
fn main() {
    let x = 10;

    print_number(x);

    println!("{}", x);
}

fn print_number(number: i32) {
    println!("{}", number);
}
```

Output

```
10
10
```

Notice something interesting.

Even after calling

```rust
print_number(x);
```

we can still use

```rust
println!("{}", x);
```

Why?

Because integers are stored on the **Stack**.

Rust simply **copies** them.

Think of it like writing the same number on another piece of paper.

```
Main

x = 10
```

↓

Function receives

```
number = 10
```

Now there are two completely independent copies.

Nobody loses ownership.

---

# Passing a String to a Function

Now let's replace the integer with a String.

```rust
fn main() {

    let my_string = String::from("Hello");

    takes_ownership(my_string);

    println!("{}", my_string);
}

fn takes_ownership(some_string: String) {

    println!("{}", some_string);

}
```

Will this compile?

**No.**

Rust gives an error.

```
error[E0382]: borrow of moved value: `my_string`
```

---

# The Rihanna Story Continues 😂❤️

Initially

```
my_string ❤️ Rihanna
```

Everything is fine.

Now this line executes.

```rust
takes_ownership(my_string);
```

Imagine the function says

> "Hey Rihanna...
> 

come inside.

I need to talk to you."

Rihanna replies

> "Sure."
> 

But remember her rule.

She only stays in **one serious relationship**.

So she immediately leaves **my_string**...

and starts dating the function parameter.

```
Before

my_string ❤️ Rihanna
```

↓

```
After

my_string 💔

some_string ❤️ Rihanna
```

Poor **my_string** just became the ex-boyfriend.

😂😂

---

# Memory View

Before function call

```
STACK                          HEAP

my_string  ───────────────►  "Hello"
```

After entering the function

```
STACK                          HEAP

my_string ❌ Invalid

some_string ─────────────► "Hello"
```

Ownership has moved.

The heap memory never moved.

Only the owner changed.

---

# What Happens When the Function Ends?

Now imagine the function finishes.

```rust
fn takes_ownership(some_string: String) {

    println!("{}", some_string);

}
```

The closing brace is reached.

```rust
}
```

The function goes out of scope.

Since **some_string** is now Rihanna's owner...

Rust says

> "No owner left?"
> 

> "Time to clean up."
> 

The heap memory gets automatically deallocated.

```
Before

some_string ❤️ Rihanna
```

↓

Function ends

↓

```
some_string disappears

Rihanna 💀
```

The String is removed from memory.

---

# Why Can't We Use my_string Again?

After the function call

```rust
println!("{}", my_string);
```

Rust immediately stops us.

Why?

Because

> **my_string is no longer the owner.**
> 

It lost Rihanna.

She has already left.

Rust says

> "Bro...
> 

she literally moved into another function.

You have no relationship with her anymore."

😂

---

# Let's Compare Numbers vs Strings

## Numbers

```rust
let x = 10;

print_number(x);
```

Rust performs

> Copy
> 

```
Main

x = 10

↓

Function

number = 10
```

Both variables are valid.

---

## Strings

```rust
let s = String::from("Hello");

takes_ownership(s);
```

Rust performs

> Move
> 

```
Main

s ❤️ String
```

↓

```
Function

some_string ❤️ String
```

The original variable becomes invalid.

---

# Returning Ownership Back

Now imagine the function is very polite.

Instead of stealing Rihanna forever...

it says

> "Thanks.
> 

I'm done talking.

Take her back."

😂

Rust allows this.

```rust
fn main() {

    let s1 = String::from("Hello");

    let s2 = takes_ownership(s1);

    println!("{}", s2);

}

fn takes_ownership(some_string: String) -> String {

    println!("{}", some_string);

    some_string

}
```

Output

```
Hello
Hello
```

What happened?

Initially

```
s1 ❤️ Rihanna
```

↓

Function

```
some_string ❤️ Rihanna
```

↓

Function returns

```
s2 ❤️ Rihanna
```

Notice

Rihanna changed owners again.

She went

```
s1

↓

some_string

↓

s2
```

She never had two owners.

---

# Why Didn't s1 Get Her Back?

Good question.

When the function returned,

Rust didn't magically remember the old boyfriend.

The function simply handed Rihanna to **whoever received the return value.**

That's why

```rust
let s2 = takes_ownership(s1);
```

creates

```
s2 ❤️ Rihanna
```

instead of

```
s1 ❤️ Rihanna
```

Poor **s1** is still the ex.

😂

---

# Can s1 Become Her Boyfriend Again?

Yes!

He simply has to receive ownership again.

```rust
fn main() {

    let mut s1 = String::from("Hello");

    s1 = takes_ownership(s1);

    println!("{}", s1);

}

fn takes_ownership(some_string: String) -> String {

    some_string

}
```

Now the ownership flow is

```
s1 ❤️ Rihanna

↓

Function

↓

s1 ❤️ Rihanna
```

The same variable becomes the owner again because it receives the returned value.

---

# But This Looks Annoying...

Imagine every function call looked like this.

```rust
let s = takes_ownership(s);
```

or

```rust
let s = another_function(s);

let s = third_function(s);

let s = fourth_function(s);
```

We're constantly passing ownership around like Rihanna is being dropped off and picked up after every conversation.

😂

This works...

but it's inconvenient.

---

# Is There a Better Way?

Suppose **s1** doesn't want to break up with Rihanna.

He just wants his friend (the function) to **talk to her for a few minutes**.

No breakup.

No ownership transfer.

Just...

> "Bro, you can hang out with her, but she's still my girlfriend."
> 

😄

Can Rust do that?

**Yes!**

That is exactly what **References (&)** and **Borrowing** are designed for.

And that's what we'll learn in the next chapter.

---

# Chapter Summary

- Passing **stack values** (like integers) to functions creates a **copy**, so the original variable remains usable.
- Passing **heap values** (like `String`) transfers **ownership** to the function.
- When the function ends, the owner goes out of scope, and Rust automatically frees the memory.
- Ownership can be returned using the function's return value.
- Passing ownership back and forth works, but it quickly becomes cumbersome.

> **Next Chapter:** Instead of transferring ownership every time, we'll learn how a function can temporarily **borrow** a value using **References (`&`)**—like letting someone spend time with Rihanna without becoming her new boyfriend. 😄
>