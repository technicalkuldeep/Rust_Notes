# 16. Pattern Matching (match)

In the previous chapter, we learned about **Enums**.

For example,

```rust
enum Shape {
    Circle(f64),
    Square(f64),
    Rectangle(f64, f64),
}
```

We can create different shapes.

```rust
let circle = Shape::Circle(5.0);

let square = Shape::Square(4.0);

let rectangle = Shape::Rectangle(3.0, 6.0);
```

But now a question arises.

Suppose someone gives us a variable of type

```rust
Shape
```

How do we know whether it is

- Circle?
- Square?
- Rectangle?

Rust provides a powerful feature for this.

It is called

> **Pattern Matching (`match`)**
> 

---

# What is Pattern Matching?

Pattern Matching allows us to **check which variant of an Enum is currently stored** and execute different code for each variant.

Think of it as asking,

> "Which option did the user choose?"
> 

Depending on the answer,

we perform different actions.

---

# Real-Life Example

Imagine a traffic signal.

It can only be one of three colours.

```
Red

Yellow

Green
```

Your actions depend on the colour.

```
Red

↓

Stop
```

```
Yellow

↓

Get Ready
```

```
Green

↓

Go
```

Pattern Matching works exactly the same way.

Different variant

↓

Different logic.

---

# The `match` Keyword

Rust uses the

```rust
match
```

keyword.

Syntax

```rust
match value {

    Pattern => Action,

}
```

You can think of it like a series of

```
If this...

Do this.
```

---

# Example

Let's finally implement the

```rust
calculate_area()
```

function that we skipped in the previous chapter.

```rust
enum Shape {
    Circle(f64),
    Square(f64),
    Rectangle(f64, f64),
}

fn calculate_area(shape: Shape) -> f64 {

    match shape {

        Shape::Circle(radius) => std::f64::consts::PI * radius * radius,

        Shape::Square(side_length) => side_length * side_length,

        Shape::Rectangle(width, height) => width * height,

    }

}
```

---

# Understanding the Code

The function receives

```rust
shape
```

But Rust doesn't know

whether

```
Circle

Square

Rectangle
```

was passed.

So we ask Rust

```rust
match shape
```

This means

> "Look at the current variant stored inside `shape`."
> 

---

# Case 1 — Circle

```rust
Shape::Circle(radius)
```

If the Shape is

```rust
Shape::Circle(5.0)
```

then

```
radius = 5.0
```

Rust automatically extracts

```
5.0
```

into the variable

```
radius
```

Now we calculate

```rust
PI × radius × radius
```

Area of Circle

---

# Case 2 — Square

```rust
Shape::Square(side_length)
```

Suppose

```rust
Shape::Square(4.0)
```

Rust automatically extracts

```
side_length = 4.0
```

Then

```rust
side_length * side_length
```

Area of Square

---

# Case 3 — Rectangle

```rust
Shape::Rectangle(width, height)
```

Suppose

```rust
Shape::Rectangle(3.0, 6.0)
```

Rust automatically extracts

```
width = 3.0

height = 6.0
```

Then

```rust
width * height
```

Area of Rectangle

---

# Complete Program

```rust
enum Shape {
    Circle(f64),
    Square(f64),
    Rectangle(f64, f64),
}

fn calculate_area(shape: Shape) -> f64 {

    match shape {

        Shape::Circle(radius) => std::f64::consts::PI * radius * radius,

        Shape::Square(side_length) => side_length * side_length,

        Shape::Rectangle(width, height) => width * height,

    }

}

fn main() {

    let circle = Shape::Circle(5.0);

    let square = Shape::Square(4.0);

    let rectangle = Shape::Rectangle(3.0, 6.0);

    println!("Circle Area = {}", calculate_area(circle));

    println!("Square Area = {}", calculate_area(square));

    println!("Rectangle Area = {}", calculate_area(rectangle));

}
```

Output

```
Circle Area = 78.53981633974483

Square Area = 16

Rectangle Area = 18
```

---

# How Does `match` Work?

Think of `match` as a decision box.

```
Shape

        │

        ▼

      match

 ┌──────────────┐

 Circle ? ──────► Calculate Circle Area

 Square ? ──────► Calculate Square Area

 Rectangle ? ───► Calculate Rectangle Area

 └──────────────┘
```

Rust checks every possible variant.

When it finds the correct one,

it runs that block.

---

# Why Is `match` Better Than `if`?

Imagine writing

```rust
if shape == Circle { ... }

else if shape == Square { ... }

else if shape == Rectangle { ... }
```

This quickly becomes difficult, especially when enum variants carry different data.

With `match`, Rust not only checks the variant but also **extracts the associated values** for you.

For example,

```rust
Shape::Rectangle(width, height)
```

does two things at once:

- Checks that the variant is `Rectangle`.
- Gives you the values of `width` and `height`.

That's one of the biggest strengths of pattern matching.

---

# Why Do Rust Developers Love `match`?

The best thing about `match` is that it is **exhaustive**.

This means Rust forces you to handle **every possible variant** of an enum.

Suppose later we modify our enum.

```rust
enum Shape {

    Circle(f64),

    Square(f64),

    Rectangle(f64, f64),

    Triangle(f64, f64),

}
```

If we forget to update

```rust
match
```

Rust immediately gives a compile-time error.

It says,

> "You forgot to handle `Triangle`."
> 

This prevents bugs that might otherwise appear later while the program is running.

---

# Enum + Pattern Matching

Enums and Pattern Matching are designed to work together.

```
Enum

↓

Creates possible variants

↓

Pattern Matching

↓

Executes different logic for each variant
```

Think of them as a team.

An Enum defines **what choices exist**, while `match` defines **what should happen for each choice**.

---

# Chapter Summary

- **Pattern Matching (`match`)** lets Rust determine which variant of an enum is currently stored.
- Each branch of a `match` handles one specific variant and can extract any associated data automatically.
- In our `Shape` example, `match` allows us to calculate the correct area for a Circle, Square, or Rectangle using the values stored inside each variant.
- `match` is **exhaustive**, meaning Rust requires every enum variant to be handled. If a new variant is added later, the compiler reminds you to update your code.
- Enums and Pattern Matching are two features that are commonly used together in Rust because they make programs safer, cleaner, and easier to maintain.

---

### 💡 Easy Way to Remember

```
Enum
    ↓
"What are the possible choices?"

match
    ↓
"What should I do for each choice?"
```

> **Enum creates the choices. `match` decides the action.** This simple rule will help you understand most Rust code that uses enums.
>