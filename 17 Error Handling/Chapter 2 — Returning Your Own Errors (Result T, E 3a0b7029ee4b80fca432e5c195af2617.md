# Chapter 2 — Returning Your Own Errors (Result<T, E>)

In the previous chapter, we learned that many Rust functions return a

```rust
Result<T, E>
```

For example,

```rust
fs::read_to_string()
```

returns

```
Ok(String)
```

if everything goes well,

or

```
Err(Error)
```

if something goes wrong.

Those functions were written by the Rust standard library.

But...

**What if you write your own function?**

Can your function also return a `Result`?

**Absolutely!**

---

# Why Return a `Result`?

Imagine you're writing a function that reads a file.

```
Read File

↓

Success ?
```

If successful,

return the file contents.

If something goes wrong,

return an error.

Instead of crashing,

the function simply tells the caller

> **"I succeeded."**
> 

or

> **"I failed."**
> 

---

# Returning a Result

The syntax looks like this.

```rust
fn function_name(...) -> Result<SuccessType, ErrorType> {

}
```

Notice the return type.

```rust
Result<SuccessType, ErrorType>
```

This means

> "This function may succeed or fail."
> 

---

# Creating Our Own Error Type

Before returning an error,

we need a type to represent it.

Let's create one.

```rust
#[derive(Debug)]
struct FileReadError;
```

This creates a custom error type called

```
FileReadError
```

Think of it like creating your own category of error.

Instead of using Rust's built-in error directly,

we'll return our own.

---

# Complete Program

```rust
use std::fs;

#[derive(Debug)]
struct FileReadError;

fn main() {

    let contents = read_file("hello.txt".to_string());

    match contents {

        Ok(file_content) => {
            println!("File Content: {}", file_content);
        }

        Err(error) => {
            println!("Error: {:?}", error);
        }

    }

}

fn read_file(file_path: String) -> Result<String, FileReadError> {

    let greeting_file_result =
        fs::read_to_string(file_path);

    match greeting_file_result {

        Ok(file_content) => {
            Ok(file_content)
        }

        Err(_) => {
            Err(FileReadError)
        }

    }

}
```

---

# Understanding the Code

## Step 1

We create our own error.

```rust
#[derive(Debug)]
struct FileReadError;
```

This is simply a custom type.

Nothing more.

Think of it as giving a name to a particular kind of failure.

---

## Step 2

The function signature.

```rust
fn read_file(file_path: String)
    -> Result<String, FileReadError>
```

Let's understand this.

The function can return

```
Success

↓

String
```

or

```
Failure

↓

FileReadError
```

That's exactly what

```rust
Result<String, FileReadError>
```

means.

---

## Step 3

Read the file.

```rust
let greeting_file_result =
    fs::read_to_string(file_path);
```

Notice

This already returns

```rust
Result<String, Error>
```

Now we inspect that result.

---

## Step 4

Handle the success case.

```rust
Ok(file_content) => {

    Ok(file_content)

}
```

Suppose the file contains

```
Hello Rust
```

Rust returns

```
Ok("Hello Rust")
```

We simply pass that success back to the caller.

---

## Step 5

Handle the error case.

```rust
Err(_) => {

    Err(FileReadError)

}
```

Suppose

```
hello.txt
```

doesn't exist.

Rust returns

```
Err(...)
```

Instead of returning Rust's built-in error,

we return

```
FileReadError
```

This becomes our custom error.

---

# Why Ignore the Original Error?

Notice this.

```rust
Err(_)
```

What does

```
_
```

mean?

It means

> "Yes, I know an error occurred...
> 

but I don't need the actual error value."

We're simply saying

> "Something failed."
> 

and returning our own error instead.

---

# Visual Flow

```
read_file()

        │

        ▼

read_to_string()

        │

 ┌──────┴──────┐

Success      Failure

   │             │

   ▼             ▼

Ok(String)   Err(FileReadError)
```

Our function hides the internal details and exposes a simpler error type.

---

# Why Create Custom Errors?

Imagine you're building an Online Shopping App.

Possible errors

```
Payment Failed

Product Out Of Stock

Invalid Coupon

User Not Found
```

Instead of returning random Strings,

you can create proper error types.

Example

```rust
struct PaymentError;

struct LoginError;

struct NetworkError;
```

This makes your program

- Easier to understand
- Easier to debug
- Easier to maintain

---

# Returning `Ok` and `Err`

Whenever your function returns a `Result`,

you'll usually return one of these.

Success

```rust
Ok(value)
```

Failure

```rust
Err(error)
```

Think of it as

```
Everything Worked

↓

Ok(...)
```

```
Something Failed

↓

Err(...)
```

---

# Can We Return Different Success Types?

Yes.

For example,

```rust
Result<i32, MyError>
```

means

Success returns an integer.

---

```rust
Result<bool, MyError>
```

Success returns a boolean.

---

```rust
Result<User, MyError>
```

Success returns a User Struct.

Only the

```
Success Type
```

changes.

The idea remains exactly the same.

---

# Result Recap

Remember

```rust
enum Result<T, E> {

    Ok(T),

    Err(E),

}
```

Now it should make much more sense.

For our function,

```
T

↓

String
```

and

```
E

↓

FileReadError
```

So Rust internally treats it like

```
Result

↓

Ok(String)

or

Err(FileReadError)
```

---

# Rust's Philosophy

Rust encourages functions to clearly communicate

whether they succeeded or failed.

Instead of

- returning `null`
- returning `1`
- throwing unexpected exceptions

Rust uses

```rust
Result<T, E>
```

This makes failures explicit and forces programmers to think about error handling.

---

# Chapter Summary

- Your own functions can also return a **`Result<T, E>`**, just like functions from Rust's standard library.
- The success type (`T`) represents the value returned when everything works correctly, while the error type (`E`) represents what is returned when something goes wrong.
- You can create your own custom error types (such as `FileReadError`) to make your program's errors more meaningful and easier to manage.
- Returning `Ok(value)` indicates success, while returning `Err(error)` indicates failure.
- Using `Result` makes Rust programs more reliable because every possible failure is represented explicitly in the function's return type.

---

## Complete Error Handling Journey

You've now learned the complete workflow of error handling in Rust.

```
A Function May Fail
        │
        ▼
Returns Result<T, E>
        │
        ▼
Handle It Using match
        │
        ▼
Success → Ok(value)
Failure → Err(error)
        │
        ▼
You Can Even Create Your Own Error Types
```

🎉 With this chapter, you've covered the core concepts of **Rust Error Handling**. Later, as you explore more advanced Rust, you'll encounter convenient operators like `?` that make working with `Result` even cleaner, but everything builds on the foundation you've just learned.