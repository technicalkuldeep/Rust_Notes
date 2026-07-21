# Chapter 1 — Error Handling in Rust (Result<T, E>)

Imagine you're writing a program that reads a file.

Everything looks simple.

```rust
Read hello.txt

↓

Print its contents
```

But...

What if

- the file doesn't exist?
- someone deleted it?
- another application locked it?
- you don't have permission to read it?

Your program suddenly has a problem.

This situation is called

> **An Error**
> 

---

# What is Error Handling?

Error Handling is simply

> **Handling situations where something might go wrong.**
> 

Programs don't always work perfectly.

Sometimes

- Internet disappears 🌐
- File doesn't exist 📄
- User enters invalid input ❌
- Database is offline 💾

A good program should not blindly assume everything will succeed.

---

# JavaScript's Way

If you've used JavaScript,

you've probably seen

```jsx
try {
    const data = fs.readFileSync("example.txt", "utf8");
    console.log(data);
}
catch(err) {
    console.log(err);
}
```

Here,

JavaScript says

> "Try running this code."
> 

If something goes wrong,

jump into

```jsx
catch
```

and handle the error.

---

# Why Do We Need `try...catch`?

Suppose

```jsx
fs.readFileSync("hello.txt")
```

tries to read

```
hello.txt
```

Everything works...

if the file exists.

But imagine

```
hello.txt
```

doesn't exist.

JavaScript throws an exception.

Without

```jsx
catch
```

your program crashes.

---

# Rust Takes a Different Approach

Rust does **not** use exceptions like JavaScript.

Instead,

Rust believes

> **Errors are normal.**
> 

Reading a file might fail.

Opening a database might fail.

Connecting to the Internet might fail.

So instead of throwing exceptions,

Rust simply returns a value that says

> "Success"
> 

or

> "Failure"
> 

This value is called

> **Result**
> 

---

# The `Result` Enum

The actual definition looks like this.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Don't worry about

```rust
<T, E>
```

for now.

We'll learn Generics later.

For now,

just remember

`Result` has only **two possible variants**.

---

## Variant 1

```rust
Ok(...)
```

Everything worked successfully.

---

## Variant 2

```rust
Err(...)
```

Something went wrong.

---

Think of it like this.

```
Result

      │

 ┌────┴────┐

Ok        Err
```

Every function that may fail returns one of these two.

---

# Reading a File

Example

```rust
use std::fs;

fn main() {

    let greeting_file_result =
        fs::read_to_string("hello.txt");

}
```

Notice

```rust
read_to_string()
```

does **not** return a String.

Instead,

it returns

```rust
Result<String, Error>
```

Think of Rust saying

> "I might successfully read the file..."
> 

OR

> "Something might go wrong."
> 

---

# What Does It Return?

Conceptually,

Rust returns

```rust
Result

↓

Ok(String)
```

or

```rust
Result

↓

Err(Error)
```

If the file exists,

Rust returns

```rust
Ok("File contents...")
```

If it doesn't,

Rust returns

```rust
Err(FileNotFound)
```

instead of crashing.

---

# Handling Both Cases

Now we use

```rust
match
```

```rust
use std::fs;

fn main() {

    let greeting_file_result =
        fs::read_to_string("hello.txt");

    match greeting_file_result {

        Ok(file_content) => {

            println!("{}", file_content);

        }

        Err(error) => {

            println!("Error: {:?}", error);

        }

    }

}
```

---

# Understanding the Code

Suppose

```
hello.txt
```

exists.

Rust returns

```
Ok("Hello Rust")
```

Then

```rust
Ok(file_content)
```

runs.

Output

```
Hello Rust
```

---

Now imagine

```
hello.txt
```

doesn't exist.

Rust returns

```
Err(...)
```

Then

```rust
Err(error)
```

runs.

Output

```
Failed to read file.
```

Notice

The program **didn't crash**.

---

# Why Use `match`?

Remember the previous chapter?

Enums and Pattern Matching work together.

`Result` is also an Enum.

So naturally,

we use

```rust
match
```

to check

```
Ok ?

or

Err ?
```

---

# The Shortcut — `unwrap()`

Sometimes,

you don't want to write

```rust
match
```

Suppose you're absolutely sure

the file exists.

Rust provides

```rust
unwrap()
```

Example

```rust
use std::fs;

fn main() {

    let content =
        fs::read_to_string("hello.txt").unwrap();

    println!("{}", content);

}
```

---

# What Does `unwrap()` Do?

Think of `unwrap()` saying

> "I don't care about errors."
> 

😂

"If this succeeds..."

Give me the value.

"If it fails..."

Crash the program.

---

So

```rust
Ok("Hello")
```

becomes

```
Hello
```

But

```rust
Err(...)
```

becomes

```
💥 Program Crashes
```

---

# Should We Always Use `unwrap()`?

No.

`unwrap()` is useful only when

- Learning Rust
- Writing small examples
- Quick testing
- You're absolutely sure an error cannot happen

In real-world applications,

it's usually better to handle errors properly using

```rust
match
```

or other error-handling techniques.

---

# `match` vs `unwrap()`

| `match` | `unwrap()` |
| --- | --- |
| Handles both success and failure | Assumes success |
| Program keeps running | Program crashes on error |
| Used in production | Mostly used for quick examples and debugging |

---

# Rust's Philosophy

Many languages say

> "Throw an exception."
> 

Rust says

> "Return a Result."
> 

This makes error handling **explicit**.

When you see

```rust
Result<T, E>
```

you immediately know

> This function might fail.
> 

The compiler even encourages you to deal with that possibility instead of ignoring it.

---

# Chapter Summary

- Programs often encounter situations where operations can fail, such as reading a missing file or accessing a resource without permission.
- Unlike languages such as JavaScript that use **exceptions** (`try...catch`), Rust uses the **`Result` enum** to represent success or failure.
- `Result` has two variants:
    - `Ok(T)` → The operation succeeded.
    - `Err(E)` → The operation failed.
- Since `Result` is an **Enum**, we use **`match`** to handle both success and error cases.
- The `unwrap()` method is a shortcut that extracts the successful value from `Ok`, but it will **panic (crash the program)** if the result is `Err`.
- In real applications, handling errors with `match` (or similar techniques) is generally preferred over relying on `unwrap()`.

---

**Next Chapter:** We'll learn how to write our **own functions** that return `Result<T, E>`, create **custom error types**, and propagate meaningful errors instead of relying only on library functions. This is how Rust programs communicate failures in a clean and type-safe way.