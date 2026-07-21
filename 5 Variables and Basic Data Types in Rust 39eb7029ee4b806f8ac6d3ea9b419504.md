# 5. Variables and Basic Data Types in Rust

A **variable** is a named container used to store data in a program.

For example, we may want to store:

- A person's age → `21`
- A person's name → `"Kuldeep"`
- A product price → `99.99`
- Whether a user is logged in → `true`

In Rust, variables are created using the `let` keyword.

```rust
fn main() {
    let age = 21;

    println!("{}", age);
}
```

Here:

- `let` → Used to create a variable
- `age` → Name of the variable
- `21` → Value stored in the variable

Rust can automatically identify that `21` is an integer. This is called **type inference**.

We can also explicitly mention the data type:

```rust
fn main() {
    let age: u8 = 21;

    println!("{}", age);
}
```

Here:

```rust
age: u8
```

means that the variable `age` has the `u8` data type.

The general syntax for creating a variable is:

```rust
let variable_name: data_type = value;
```

Example:

```rust
let age: u8 = 21;
```

---

# Basic Data Types

In this section, we will study:

1. Integer types
2. Floating-point types
3. Boolean type
4. Character type
5. String types

---

# 1. Integer Types

Integers are numbers without decimal points.

Examples:

```
10
500
-25
1,000,000
```

Rust integers are mainly divided into two categories:

1. Signed integers
2. Unsigned integers

---

## Signed Integers

Signed integers can store:

- Positive numbers
- Negative numbers
- Zero

Signed integer types begin with the letter:

```
i
```

The `i` represents **integer**.

Rust provides the following signed integer types:

```
i8
i16
i32
i64
i128
isize
```

The number after `i` represents how many **bits** of memory are used to store the value.

For example:

```
i8 → 8-bit signed integer
i16 → 16-bit signed integer
i32 → 32-bit signed integer
i64 → 64-bit signed integer
i128 → 128-bit signed integer
```

The larger the number of bits, the larger the values that can be stored.

| Type | Minimum Value | Maximum Value |
| --- | --- | --- |
| `i8` | −128 | 127 |
| `i16` | −32,768 | 32,767 |
| `i32` | −2,147,483,648 | 2,147,483,647 |
| `i64` | Very large negative number | Very large positive number |
| `i128` | Extremely large negative number | Extremely large positive number |

> `i32` is the default integer type in Rust.
> 

If we write:

```rust
let number = 100;
```

Rust usually treats `number` as an `i32`.

---

## Signed Integer Example

```rust
fn main() {
    let temperature: i8 = -20;

    let marks: i16 = 450;

    let population: i32 = 1_400_000_000;

    let large_number: i64 = 5_000_000_000;

    let extremely_large_number: i128 =
        100_000_000_000_000_000_000;

    println!("{}", temperature);
    println!("{}", marks);
    println!("{}", population);
    println!("{}", large_number);
    println!("{}", extremely_large_number);
}
```

Output:

```
-20
450
1400000000
5000000000
100000000000000000000
```

### Explanation

```rust
let temperature: i8 = -20;
```

An `i8` is used because temperature may contain a negative value.

```rust
let marks: i16 = 450;
```

An `i16` can easily store the value `450`.

```rust
let population: i32 = 1_400_000_000;
```

An `i32` can store large values up to approximately `2.1 billion`.

```rust
let large_number: i64 = 5_000_000_000;
```

The value `5 billion` is too large for an `i32`, so we use `i64`.

---

## Using Underscores in Numbers

Rust allows underscores inside large numbers to improve readability.

```rust
let population = 1_400_000_000;
```

Rust reads it as:

```
1400000000
```

The underscores do not change the value. They only make large numbers easier for humans to read.

---

# Unsigned Integers

Unsigned integers can store:

- Positive numbers
- Zero

They **cannot store negative numbers**.

Unsigned integer types begin with:

```
u
```

The `u` represents **unsigned**.

Rust provides the following unsigned integer types:

```
u8
u16
u32
u64
u128
usize
```

| Type | Minimum Value | Maximum Value |
| --- | --- | --- |
| `u8` | 0 | 255 |
| `u16` | 0 | 65,535 |
| `u32` | 0 | 4,294,967,295 |
| `u64` | 0 | Very large positive number |
| `u128` | 0 | Extremely large positive number |

Because unsigned integers do not need to store negative numbers, they can store a larger positive value than a signed integer of the same size.

For example:

```
i8 → −128 to 127

u8 → 0 to 255
```

---

## Unsigned Integer Example

```rust
fn main() {
    let age: u8 = 21;

    let score: u16 = 50_000;

    let views: u32 = 3_000_000;

    let total_transactions: u64 = 10_000_000_000;

    let massive_number: u128 =
        200_000_000_000_000_000_000;

    println!("{}", age);
    println!("{}", score);
    println!("{}", views);
    println!("{}", total_transactions);
    println!("{}", massive_number);
}
```

Output:

```
21
50000
3000000
10000000000
200000000000000000000
```

---

## `isize` and `usize`

Rust also provides:

```
isize
usize
```

Their size depends on the computer architecture.

On a:

```
32-bit computer → 32 bits

64-bit computer → 64 bits
```

`usize` is commonly used for:

- Array indexes
- Collection indexes
- Collection sizes
- Memory-related operations

Example:

```rust
fn main() {
    let index: usize = 2;

    let numbers = [10, 20, 30];

    println!("{}", numbers[index]);
}
```

Output:

```
30
```

We will understand arrays and indexing in detail later.

---

# 2. Floating-Point Types

Floating-point numbers are numbers that contain decimal values.

Examples:

```
5.8
99.99
3.14159
-10.5
```

Rust provides two floating-point types:

```
f32
f64
```

| Type | Size | Precision |
| --- | --- | --- |
| `f32` | 32 bits | Lower precision |
| `f64` | 64 bits | Higher precision |

`f64` provides greater accuracy because it uses more bits.

> `f64` is the default floating-point type in Rust.
> 

Example:

```rust
fn main() {
    let height: f32 = 5.8;

    let pi: f64 = 3.141592653589793;

    println!("Height: {}", height);

    println!("Pi: {}", pi);
}
```

Output:

```
Height: 5.8
Pi: 3.141592653589793
```

If we do not explicitly mention the type:

```rust
let price = 99.99;
```

Rust generally treats it as:

```rust
let price: f64 = 99.99;
```

---

# 3. Boolean Type

A Boolean represents one of only two possible values:

```
true
false
```

The Boolean data type in Rust is written as:

```rust
bool
```

Booleans are commonly used for:

- Conditions
- Login status
- Checking permissions
- Enabling or disabling features
- Decision-making

Example:

```rust
fn main() {
    let is_logged_in: bool = true;

    println!("{}", is_logged_in);
}
```

Output:

```
true
```

---

## Boolean with a Condition

```rust
fn main() {
    let is_logged_in: bool = true;

    if is_logged_in {
        println!("Welcome!");
    }
}
```

Output:

```
Welcome!
```

The message is printed because:

```rust
is_logged_in = true
```

We will study `if` conditions in detail later.

---

# 4. Character Type

Rust has a separate data type for storing a single character.

The character type is written as:

```
char
```

A character is written inside **single quotation marks**:

```rust
let grade: char = 'A';
```

Example:

```rust
fn main() {
    let grade: char = 'A';

    let symbol: char = '@';

    let emoji: char = '🦀';

    println!("{}", grade);
    println!("{}", symbol);
    println!("{}", emoji);
}
```

Output:

```
A
@
🦀
```

Rust's `char` type supports Unicode characters. Therefore, it can store:

- English letters
- Symbols
- Characters from many languages
- Many emojis

A Rust `char` occupies **4 bytes**.

---

## Character vs String

```rust
let grade: char = 'A';

let name: &str = "Kuldeep";
```

Notice:

```
'A' → Single quotes → Character

"Kuldeep" → Double quotes → String
```

---

# 5. String Types

Strings are used to store text.

Examples:

```
"Kuldeep"

"Hello, Rust!"

"Learning Rust is fun"
```

Rust mainly has two commonly used string types:

1. `&str` — String slice
2. `String` — Owned and growable string

---

# `&str` — String Slice

A string literal such as:

```rust
"Hello"
```

usually has the type:

```
&str
```

It is commonly used for fixed text that does not need to be changed.

Example:

```rust
fn main() {
    let name: &str = "Kuldeep";

    println!("{}", name);
}
```

Output:

```
Kuldeep
```

Here:

```rust
let name: &str = "Kuldeep";
```

- `name` → Variable name
- `&str` → String-slice type
- `"Kuldeep"` → Text stored in the variable

You generally cannot directly modify the contents of a string literal.

---

# `String` — Owned and Growable String

`String` is used when text needs to be:

- Created dynamically
- Modified
- Expanded
- Owned by a variable

A `String` can be created using:

```rust
String::from("text")
```

Example:

```rust
fn main() {
    let mut name: String = String::from("Kuldeep");

    name.push_str(" Sharma");

    println!("{}", name);
}
```

Output:

```
Kuldeep Sharma
```

### Explanation

```rust
let mut name: String = String::from("Kuldeep");
```

- `let` → Creates a variable
- `mut` → Allows the variable's value to be modified
- `name` → Variable name
- `String` → Data type
- `String::from("Kuldeep")` → Creates an owned and growable string

```rust
name.push_str(" Sharma");
```

`push_str()` adds more text to the existing `String`.

The value changes from:

```
Kuldeep
```

to:

```
Kuldeep Sharma
```

---

## `&str` vs `String`

| `&str` | `String` |
| --- | --- |
| String slice | Owned string |
| Commonly used for fixed text | Used for growable or changeable text |
| Usually borrowed text | Owns its text |
| Often written as a string literal | Commonly created using `String::from()` |
| Usually cannot be directly expanded | Can be expanded using methods such as `push_str()` |

Example:

```rust
let language: &str = "Rust";

let mut message: String = String::from("Hello");

message.push_str(", Rust!");
```

> The difference between `&str` and `String` is closely related to **Ownership, Borrowing, Stack, and Heap**. We will understand it more deeply after learning those concepts.
> 

---

# Type Inference

Rust can often automatically determine the type of a variable from its value.

Therefore, instead of writing:

```rust
let age: i32 = 21;
```

we can write:

```rust
let age = 21;
```

Rust automatically infers that `age` is an `i32`.

More examples:

```rust
fn main() {
    let age = 21;

    let price = 99.99;

    let is_learning = true;

    let grade = 'A';

    let language = "Rust";

    println!("{}", age);
    println!("{}", price);
    println!("{}", is_learning);
    println!("{}", grade);
    println!("{}", language);
}
```

Rust generally infers these types as:

```
age        → i32

price      → f64

is_learning → bool

grade      → char

language   → &str
```

---

# Quick Summary

| Data Type | Purpose | Example |
| --- | --- | --- |
| `i8`, `i16`, `i32`, `i64`, `i128` | Positive and negative integers | `-20` |
| `u8`, `u16`, `u32`, `u64`, `u128` | Zero and positive integers | `21` |
| `isize` | Architecture-sized signed integer | `-10` |
| `usize` | Sizes and indexes | `2` |
| `f32` | Decimal number with lower precision | `5.8` |
| `f64` | Decimal number with higher precision | `3.14159` |
| `bool` | True or false value | `true` |
| `char` | A single Unicode character | `'A'` |
| `&str` | String slice, commonly used for fixed text | `"Rust"` |
| `String` | Owned and growable text | `String::from("Rust")` |

The basic types covered here are the ones we will use frequently while learning Rust. Rust also provides other data types, such as **tuples, arrays, structs, and enums**, which we will study separately.