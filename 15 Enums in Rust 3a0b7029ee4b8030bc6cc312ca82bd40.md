# 15. Enums in Rust

So far, we've learned how to create our own data types using **Structs**.

A Struct answers the question:

> **"What information does an object have?"**
> 

For example,

```rust
struct User {
    username: String,
    email: String,
    active: bool,
}
```

A User always has

- Username
- Email
- Active Status

These are all pieces of information.

---

But sometimes we don't need multiple pieces of information.

Sometimes we just want to say:

> **A value can only be one of a few predefined options.**
> 

For this, Rust provides

> **Enums (Enumerations).**
> 

---

# What is an Enum?

An **Enum** is a custom data type that allows a value to be **one of several predefined variants**.

Think of it like a multiple-choice question.

```
Choose your favourite season

A. Summer

B. Winter

C. Rainy

D. Spring
```

You can only choose **one** option.

You cannot invent

```
Chocolate Season
```

😂

Similarly,

Enums allow only the variants you define.

---

# Why Do We Need Enums?

Suppose we're building a game.

The player can move only in four directions.

- North
- South
- East
- West

A beginner might write

```rust
fn move_player(direction: String) {

}
```

Then call

```rust
move_player("north".to_string());
```

Looks fine...

Until someone writes

```rust
move_player("upwards".to_string());
```

or

```rust
move_player("left-left-right".to_string());
```

😂

Rust happily accepts these Strings because

a String can contain **anything**.

This is not safe.

---

# The Better Solution

Instead,

we define exactly four valid directions.

```rust
enum Direction {
    North,
    East,
    South,
    West,
}
```

Now these are the **only** possible values.

Rust will never allow

```
Up

Down

North-East

RandomDirection
```

unless you explicitly add them.

This makes your program much safer.

---

# Creating an Enum

```rust
enum Direction {
    North,
    East,
    South,
    West,
}
```

Each option is called a

> **Variant**
> 

Here,

the variants are

- North
- East
- South
- West

---

# Creating an Enum Value

```rust
fn main() {

    let my_direction = Direction::North;

}
```

Notice the syntax.

```rust
Direction::North
```

means

> Create the **North** variant of the `Direction` enum.
> 

Similarly,

```rust
Direction::East

Direction::South

Direction::West
```

are all valid.

---

# Passing Enums to Functions

```rust
enum Direction {
    North,
    East,
    South,
    West,
}

fn main() {

    let my_direction = Direction::North;

    move_around(my_direction);

}

fn move_around(direction: Direction) {

    println!("Moving...");

}
```

Here,

the function accepts

```rust
Direction
```

instead of

```rust
String
```

Now only valid directions can be passed.

---

# Why Does This Work?

Notice

```rust
let my_direction = Direction::North;

let new_direction = my_direction;
```

This works without any problem.

Why?

Because simple enums like this contain no heap data.

They are small values stored on the **Stack**, so Rust automatically copies them.

Just like

- `i32`
- `bool`

these enums implement the **Copy** trait automatically.

---

# Why Not Use a String?

Let's compare both approaches.

### Using String

```rust
fn move_player(direction: String) {

    if direction == "north" {

        println!("Moving North");

    }

}
```

This works.

But it accepts anything.

For example,

```rust
move_player("Pizza".to_string());

move_player("Moon".to_string());

move_player("Random".to_string());
```

😂

These make no sense,

yet Rust allows them.

---

### Using Enum

```rust
move_player(Direction::North);
```

Now Rust guarantees

that only

- North
- East
- South
- West

can ever be passed.

This eliminates an entire class of bugs.

---

# Enums Can Also Store Data

So far,

our variants looked like this.

```rust
North

South

East

West
```

But variants can also carry their own data.

Example

```rust
enum Shape {

    Circle(f64),

    Square(f64),

    Rectangle(f64, f64),

}
```

Now each variant stores different information.

---

## Circle

```rust
Shape::Circle(5.0)
```

The value

```
5.0
```

is the radius.

---

## Square

```rust
Shape::Square(4.0)
```

The value

```
4.0
```

is the side length.

---

## Rectangle

```rust
Shape::Rectangle(3.0, 6.0)
```

Stores

- Width
- Height

---

# Creating Enum Values

```rust
fn main() {

    let circle = Shape::Circle(5.0);

    let square = Shape::Square(4.0);

    let rectangle = Shape::Rectangle(3.0, 6.0);

}
```

Notice

Although all three variables have the same type

```rust
Shape
```

they store different variants.

---

# Why Is This Useful?

Imagine writing

```rust
struct Shape {

    radius: f64,

    side: f64,

    width: f64,

    height: f64,

}
```

This makes no sense.

A circle doesn't have

- width
- height
- side

A square doesn't have

- radius

Most of the fields would always remain unused.

Enums solve this beautifully.

Each variant stores **only the data it actually needs**.

---

# What About Calculating the Area?

Suppose we write

```rust
fn calculate_area(shape: Shape) -> f64 {

}
```

How does Rust know

whether it's

- Circle
- Square
- Rectangle

?

The answer is

> **Pattern Matching**
> 

Rust provides a very powerful feature called

```rust
match
```

which allows us to check which variant we're dealing with and execute different code accordingly.

We'll implement

```rust
calculate_area()
```

in the next chapter when we study **Pattern Matching**.

---

# Struct vs Enum

| Struct | Enum |
| --- | --- |
| Groups related data together | Represents one of several possible variants |
| Every field always exists | Only one variant exists at a time |
| Example: User | Example: Direction |

---

# Chapter Summary

- An **Enum (Enumeration)** is a custom data type that allows a value to be **one of several predefined variants**.
- Enums are useful when a variable should have only a limited set of valid values, making programs safer than using plain strings.
- Variants can be **simple** (like `North`, `South`) or they can **carry their own data** (like `Circle(f64)` or `Rectangle(f64, f64)`).
- Simple enums are small values and are typically copied rather than moved because they don't contain heap-allocated data.
- Enums become especially powerful when combined with **Pattern Matching (`match`)**, which we'll learn in the next chapter to perform different actions based on the selected variant.

---

### 💡 Easy Way to Remember

```
Struct
       ↓
"What information does this object have?"

Enum
       ↓
"Which one of these predefined choices is this value?"
```

This single distinction helps you decide **when to use a Struct and when to use an Enum** in most beginner Rust programs.