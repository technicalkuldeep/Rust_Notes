# 7. Functions in Rust

A **function** is a reusable block of code designed to perform a specific task.

Instead of writing the same code again and again, we can place that code inside a function and call the function whenever it is needed.

Functions help us:

- Organize code into smaller parts
- Reuse the same code
- Avoid code repetition
- Make programs easier to read
- Make programs easier to test and maintain

---

# Creating a Function

In Rust, a function is created using the `fn` keyword.

### Syntax

```rust
fn function_name() {
    // Code to execute
}
```

Here:

- `fn` → Keyword used to define a function
- `function_name` → Name of the function
- `()` → Used to receive input values called parameters
- `{ }` → Contains the function body

---

## Simple Function Example

```rust
fn greet() {
    println!("Hello, welcome to Rust!");
}

fn main() {
    greet();
}
```

Output:

```
Hello, welcome to Rust!
```

### Explanation

First, we created a function:

```rust
fn greet() {
    println!("Hello, welcome to Rust!");
}
```

The function contains the code that prints:

```
Hello, welcome to Rust!
```

Then, we called the function inside `main()`:

```rust
greet();
```

Calling a function means telling Rust to execute the code written inside that function.

---

# The `main()` Function

Every executable Rust program starts its execution from the `main()` function.

```rust
fn main() {
    println!("Program started!");
}
```

The `main()` function is called the **entry point** of a Rust application.

> The execution of a Rust application begins from `main()`.
> 

Other functions do not execute automatically. We need to call them from `main()` or another function.

Example:

```rust
fn say_hello() {
    println!("Hello!");
}

fn main() {
    println!("Program started");

    say_hello();

    println!("Program ended");
}
```

Output:

```
Program started
Hello!
Program ended
```

---

# Function Parameters

Sometimes, a function needs some information to perform its task.

The values received by a function are called **parameters**.

### Syntax

```rust
fn function_name(parameter: data_type) {
    // Use the parameter
}
```

Example:

```rust
fn greet(name: &str) {
    println!("Hello, {}!", name);
}

fn main() {
    greet("Kuldeep");
}
```

Output:

```
Hello, Kuldeep!
```

Here:

```rust
fn greet(name: &str)
```

- `name` → Parameter name
- `&str` → Data type of the parameter

When we call:

```rust
greet("Kuldeep");
```

the value `"Kuldeep"` is passed to the `name` parameter.

The function then uses it:

```rust
println!("Hello, {}!", name);
```

---

# Parameters vs Arguments

The terms **parameter** and **argument** are closely related but have different meanings.

```rust
fn greet(name: &str) {
    println!("Hello, {}!", name);
}

fn main() {
    greet("Kuldeep");
}
```

Here:

```rust
name: &str
```

is a **parameter** because it is written in the function definition.

```rust
"Kuldeep"
```

is an **argument** because it is the actual value passed while calling the function.

| Term | Meaning |
| --- | --- |
| Parameter | Variable written in the function definition |
| Argument | Actual value passed while calling the function |

---

# Function with Multiple Parameters

A function can receive multiple parameters.

Parameters are separated using commas.

### Syntax

```rust
fn function_name(
    parameter1: data_type,
    parameter2: data_type
) {
    // Function code
}
```

Example:

```rust
fn introduce(name: &str, age: u8) {
    println!("My name is {}.", name);
    println!("I am {} years old.", age);
}

fn main() {
    introduce("Kuldeep", 21);
}
```

Output:

```
My name is Kuldeep.
I am 21 years old.
```

Here:

```rust
fn introduce(name: &str, age: u8)
```

contains two parameters:

| Parameter | Type | Value Received |
| --- | --- | --- |
| `name` | `&str` | `"Kuldeep"` |
| `age` | `u8` | `21` |

---

## Addition Using a Function

```rust
fn add(a: i32, b: i32) {
    let sum = a + b;

    println!("Sum: {}", sum);
}

fn main() {
    add(10, 20);
}
```

Output:

```
Sum: 30
```

The values are passed as:

```
a = 10

b = 20
```

The function calculates:

```
10 + 20 = 30
```

---

# Functions with Return Values

A function can perform a calculation and return the result to the place where it was called.

The return type is written after an arrow:

```
->
```

### Syntax

```rust
fn function_name() -> return_type {
    value
}
```

Example:

```rust
fn get_number() -> i32 {
    10
}

fn main() {
    let number = get_number();

    println!("{}", number);
}
```

Output:

```
10
```

Here:

```rust
fn get_number() -> i32
```

means that the function returns an `i32` value.

The returned value is:

```rust
10
```

It is stored inside:

```rust
let number = get_number();
```

---

# Returning the Result of a Calculation

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}

fn main() {
    let result = add(10, 20);

    println!("Sum: {}", result);
}
```

Output:

```
Sum: 30
```

### Explanation

The function receives:

```
a = 10

b = 20
```

It calculates:

```rust
a + b
```

The result is:

```
30
```

The value `30` is returned and stored in:

```rust
let result = add(10, 20);
```

---

# Important: Returning a Value Without a Semicolon

In Rust, the last expression of a function can automatically become its return value.

Notice:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

There is **no semicolon** after:

```rust
a + b
```

This means:

> Calculate `a + b` and return its value.
> 

If we add a semicolon:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b;
}
```

Rust will show an error.

This happens because adding a semicolon converts the expression into a statement, and its calculated value is no longer returned.

---

# Statements vs Expressions

Understanding statements and expressions is important in Rust.

## Statement

A **statement** performs an action but does not return a useful value.

Example:

```rust
let age = 21;
```

This creates a variable.

Statements usually end with a semicolon:

```
;
```

---

## Expression

An **expression** produces or returns a value.

Examples:

```rust
10
```

```rust
5 + 5
```

```rust
a + b
```

For example:

```rust
fn get_number() -> i32 {
    10
}
```

The expression:

```rust
10
```

returns the value `10`.

---

## Semicolon Difference

Without a semicolon:

```rust
5 + 5
```

It produces the value:

```
10
```

With a semicolon:

```rust
5 + 5;
```

The value is not returned.

> In Rust, the absence or presence of a semicolon can change the meaning of code.
> 

---

# Returning Values Using the `return` Keyword

Rust also provides the `return` keyword.

Example:

```rust
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}

fn main() {
    let result = add(10, 20);

    println!("{}", result);
}
```

Output:

```
30
```

The following two functions produce the same result:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

```rust
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}
```

However, Rust programmers usually prefer the first style when returning the final value of a function:

```rust
a + b
```

It is shorter and follows common Rust style.

---

# Early Return

The `return` keyword is useful when we want to exit a function before reaching its end.

Example:

```rust
fn check_age(age: u8) -> &'static str {
    if age < 18 {
        return "Minor";
    }

    "Adult"
}

fn main() {
    let result = check_age(16);

    println!("{}", result);
}
```

Output:

```
Minor
```

When:

```rust
age < 18
```

is `true`, this line executes:

```rust
return "Minor";
```

The function immediately stops and returns `"Minor"`.

The remaining code is not executed.

> The `&'static str` syntax will become clearer when we study references and lifetimes. For now, understand that this function returns fixed text.
> 

---

# Function Returning a Boolean

A function can return a Boolean value.

```rust
fn is_adult(age: u8) -> bool {
    age >= 18
}

fn main() {
    let result = is_adult(21);

    println!("{}", result);
}
```

Output:

```
true
```

The expression:

```rust
age >= 18
```

returns either:

```
true
```

or:

```
false
```

---

# Function Returning a String

```rust
fn get_language() -> String {
    String::from("Rust")
}

fn main() {
    let language = get_language();

    println!("{}", language);
}
```

Output:

```
Rust
```

The function returns a value of type:

```
String
```

---

# Functions Can Call Other Functions

One function can call another function.

```rust
fn greet() {
    println!("Hello!");
}

fn welcome() {
    greet();

    println!("Welcome to Rust.");
}

fn main() {
    welcome();
}
```

Output:

```
Hello!
Welcome to Rust.
```

The execution flow is:

```
main()

   ↓

welcome()

   ↓

greet()
```

---

# Function Naming Rules

Rust functions are generally written using **snake_case**.

In snake case:

- All letters are lowercase
- Multiple words are separated using underscores

Correct:

```rust
fn calculate_total() {
}
```

```rust
fn get_user_name() {
}
```

```rust
fn check_login_status() {
}
```

Not recommended:

```rust
fn CalculateTotal() {
}
```

```rust
fn calculateTotal() {
}
```

Rust may show a warning when function names do not follow Rust naming conventions.

---

# Complete Example

```rust
fn calculate_total(
    price: f64,
    quantity: u32
) -> f64 {
    price * quantity as f64
}

fn main() {
    let total = calculate_total(99.50, 3);

    println!("Total price: {}", total);
}
```

Output:

```
Total price: 298.5
```

The function:

```rust
fn calculate_total(
    price: f64,
    quantity: u32
) -> f64
```

receives:

- `price` as an `f64`
- `quantity` as a `u32`

It returns:

```rust
price * quantity as f64
```

The result is stored in:

```rust
let total = calculate_total(99.50, 3);
```

> `quantity as f64` converts the `u32` value into an `f64`. Type conversion will be studied separately.
> 

---

# Quick Summary

| Concept | Meaning |
| --- | --- |
| `fn` | Keyword used to define a function |
| `main()` | Entry point of a Rust application |
| Function call | Executes a function |
| Parameter | Variable received by a function |
| Argument | Actual value passed to a function |
| `->` | Specifies the return type |
| Return value | Value sent back by a function |
| `return` | Immediately returns a value and exits the function |
| Expression | Produces a value |
| Statement | Performs an action |
| `snake_case` | Recommended naming style for Rust functions |

---

## Basic Function Structure

```rust
fn function_name(
    parameter: data_type
) -> return_type {
    return_value
}
```

Example:

```rust
fn square(number: i32) -> i32 {
    number * number
}

fn main() {
    let result = square(5);

    println!("{}", result);
}
```

Output:

```
25
```

> Functions divide a large program into smaller, reusable, and easy-to-understand blocks of code.
>