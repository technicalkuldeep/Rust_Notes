# 18. Option Enum (Option<T>)

---

In the previous chapters, we learned about the **`Result` enum**.

`Result` is used when an operation can **succeed or fail**.

For example,

- Reading a file
- Connecting to a database
- Sending a network request

These operations might produce an error.

But not every missing value is an **error**.

Sometimes,

there is simply **no value to return**.

Rust uses another enum for this situation.

It is called

> **`Option<T>`**
> 

---

# The Problem with `null`

Many programming languages have a special value called

```
null
```

or

```
nil
```

or

```
None
```

depending on the language.

It means

> "There is no value."
> 

Imagine a function that searches for a student.

```
Find Student

↓

Student Found ?

Yes → Return Student

No → Return null
```

This seems convenient.

But it causes a lot of problems.

---

# Why is `null` Dangerous?

Suppose JavaScript has

```jsx
let user = findUser(10);
```

If the user doesn't exist,

```jsx
user
```

becomes

```
null
```

Now imagine writing

```jsx
console.log(user.name);
```

💥

The program crashes because

```
null
```

has no

```
name
```

property.

These kinds of bugs became so common that Tony Hoare (who invented `null`) later called it

> **"My Billion-Dollar Mistake."**
> 

It caused countless software bugs over the years.

---

# Rust Has No `null`

Rust completely removes the idea of `null`.

Instead,

Rust forces you to explicitly say

> "A value may or may not exist."
> 

It does this using another enum.

---

# The `Option` Enum

The real definition looks like this.

```rust
pub enum Option<T> {
    Some(T),
    None,
}
```

Don't worry about

```rust
<T>
```

for now.

Just remember that `Option` has only **two variants**.

---

## `Some(T)`

```rust
Some(value)
```

means

> **A value exists.**
> 

---

## `None`

```rust
None
```

means

> **No value exists.**
> 

---

Think of it like this.

```
Option

        │

   ┌────┴────┐

 Some      None
```

Every value is either

- Present

or

- Absent

---

# When Should We Use `Option`?

Whenever a function might **not have a value to return**, use `Option`.

Examples

- Searching for a user
- Searching for a character
- Finding an item in a list
- Looking up a key in a map

These are **not errors**.

Sometimes the value simply isn't there.

---

# Example

Suppose we want to find the first occurrence of the letter

```
'a'
```

inside a string.

If we find it,

return its index.

Otherwise,

return nothing.

---

## Function

```rust
fn find_first_a(s: String) -> Option<i32> {

    for (index, character) in s.chars().enumerate() {

        if character == 'a' {

            return Some(index as i32);

        }

    }

    None

}
```

Notice the return type.

```rust
Option<i32>
```

This means

The function returns

- An integer

or

- No value

---

# Understanding the Code

Suppose

```
raman
```

is passed.

Rust checks

```
r

a ← Found!
```

The index is

```
1
```

So the function returns

```rust
Some(1)
```

---

Now imagine

```
hello
```

Rust checks

```
h

e

l

l

o
```

No

```
a
```

was found.

The function returns

```rust
None
```

---

# Using `match`

Since `Option` is also an Enum,

we use

```rust
match
```

to check it.

```rust
fn main() {

    let my_string = String::from("raman");

    match find_first_a(my_string) {

        Some(index) => {

            println!("Found at index {}", index);

        }

        None => {

            println!("Letter not found.");

        }

    }

}
```

---

# How `match` Works

If the function returns

```rust
Some(1)
```

Rust executes

```rust
Some(index)
```

Here,

```
index = 1
```

---

If the function returns

```rust
None
```

Rust executes

```rust
None
```

There is no value to extract because nothing exists.

---

# Complete Program

```rust
fn find_first_a(s: String) -> Option<i32> {

    for (index, character) in s.chars().enumerate() {

        if character == 'a' {

            return Some(index as i32);

        }

    }

    None

}

fn main() {

    let my_string = String::from("raman");

    match find_first_a(my_string) {

        Some(index) => {

            println!("The letter 'a' is found at index: {}", index);

        }

        None => {

            println!("The letter 'a' was not found.");

        }

    }

}
```

Output

```
The letter 'a' is found at index: 1
```

---

# Another Example

Searching for a student.

```
Student Found

↓

Some(Student)
```

Student doesn't exist.

```
None
```

Notice

No error happened.

The student simply wasn't found.

---

# `Option` vs `Result`

This is one of the most common beginner questions.

| `Option` | `Result` |
| --- | --- |
| Used when a value may or may not exist | Used when an operation may succeed or fail |
| `Some(value)` | `Ok(value)` |
| `None` | `Err(error)` |
| No error information | Contains error information |

---

### Example

Searching for a user.

User doesn't exist.

```rust
Option<User>
```

No error occurred.

---

Reading a file.

File doesn't exist.

```rust
Result<String, Error>
```

This **is** an error.

---

# Visual Comparison

```
Option

      │

 ┌────┴────┐

Some      None
```

```
Result

      │

 ┌────┴────┐

Ok        Err
```

A simple way to remember the difference is:

- **Option** answers: *"Is there a value?"*
- **Result** answers: *"Did the operation succeed?"*

---

# Rust's Philosophy

Instead of allowing hidden `null` values that may crash your program later,

Rust forces you to acknowledge that

> "A value might not exist."
> 

This makes programs much safer and prevents an entire class of bugs caused by unexpected `null` values.

---

# Chapter Summary

- Rust does **not** have a `null` value.
- Instead, Rust uses the **`Option<T>` enum** to represent the presence or absence of a value.
- `Some(value)` means a value exists, while `None` means no value exists.
- Functions that may not have a meaningful value to return should use `Option<T>` instead of returning `null`.
- Since `Option` is an enum, we commonly use **`match`** to handle both `Some` and `None`.
- Use **`Option`** when the absence of a value is normal, and **`Result`** when something can actually fail and you need error information.

---

### 💡 Easy Way to Remember

```
Option
      ↓
"Is there a value?"

Some(value)  → Yes ✅

None         → No ❌

Result
      ↓
"Did the operation succeed?"

Ok(value)    → Success ✅

Err(error)   → Failure ❌
```

This distinction is one of the most important ideas in Rust. If you remember **Option = missing value** and **Result = possible error**, you'll understand when to use each in real Rust programs.