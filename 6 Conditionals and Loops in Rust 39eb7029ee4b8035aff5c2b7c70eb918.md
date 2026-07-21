# 6. Conditionals and Loops in Rust

Programs do not always execute every line in the same way. Sometimes, a program needs to:

- Make a decision based on a condition
- Execute some code only when a condition is true
- Repeat the same code multiple times

Rust provides:

1. **Conditionals** → Used for decision-making
2. **Loops** → Used for repeating code

---

# 1. Conditionals in Rust

Conditionals allow a program to execute different code depending on whether a condition is `true` or `false`.

Rust provides:

- `if`
- `else`
- `else if`

---

## The `if` Statement

The `if` statement executes a block of code only when its condition is `true`.

### Syntax

```rust
if condition {
    // Code executed when the condition is true
}
```

### Example

```rust
fn main() {
    let age = 20;

    if age >= 18 {
        println!("You are eligible to vote.");
    }
}
```

Output:

```
You are eligible to vote.
```

### Explanation

```rust
if age >= 18
```

This checks whether `age` is greater than or equal to `18`.

Since:

```
20 >= 18 → true
```

the code inside the curly braces executes:

```rust
println!("You are eligible to vote.");
```

---

## Comparison Operators

Comparison operators are commonly used to create conditions.

| Operator | Meaning | Example |
| --- | --- | --- |
| `==` | Equal to | `age == 18` |
| `!=` | Not equal to | `age != 18` |
| `>` | Greater than | `age > 18` |
| `<` | Less than | `age < 18` |
| `>=` | Greater than or equal to | `age >= 18` |
| `<=` | Less than or equal to | `age <= 18` |

Example:

```rust
fn main() {
    let marks = 75;

    if marks >= 40 {
        println!("You passed the exam.");
    }
}
```

Output:

```
You passed the exam.
```

---

# The `if-else` Statement

The `else` block executes when the `if` condition is `false`.

### Syntax

```rust
if condition {
    // Runs when the condition is true
} else {
    // Runs when the condition is false
}
```

### Example

```rust
fn main() {
    let age = 16;

    if age >= 18 {
        println!("You are eligible to vote.");
    } else {
        println!("You are not eligible to vote.");
    }
}
```

Output:

```
You are not eligible to vote.
```

### Explanation

The condition is:

```rust
age >= 18
```

Since:

```
16 >= 18 → false
```

the `if` block is skipped and the `else` block executes.

---

# The `else if` Statement

`else if` is used when we need to check multiple conditions.

### Syntax

```rust
if condition_1 {
    // Runs when condition_1 is true
} else if condition_2 {
    // Runs when condition_2 is true
} else {
    // Runs when all conditions are false
}
```

### Example: Grade Calculator

```rust
fn main() {
    let marks = 82;

    if marks >= 90 {
        println!("Grade: A+");
    } else if marks >= 80 {
        println!("Grade: A");
    } else if marks >= 70 {
        println!("Grade: B");
    } else if marks >= 60 {
        println!("Grade: C");
    } else {
        println!("Grade: D");
    }
}
```

Output:

```
Grade: A
```

Rust checks the conditions from top to bottom.

```
marks >= 90 → false

marks >= 80 → true
```

After finding the first `true` condition, Rust executes its code and skips the remaining conditions.

---

# Logical Operators

Logical operators allow us to combine multiple conditions.

| Operator | Name | Meaning |
| --- | --- | --- |
| `&&` | Logical AND | Both conditions must be true |
| `||` | Logical OR | At least one condition must be true |
| `!` | Logical NOT | Reverses a Boolean value |

---

## AND Operator: `&&`

Both conditions must be `true`.

```rust
fn main() {
    let age = 21;
    let has_voter_id = true;

    if age >= 18 && has_voter_id {
        println!("You can vote.");
    }
}
```

Output:

```
You can vote.
```

Both conditions are true:

```
age >= 18       → true

has_voter_id    → true
```

Therefore, the complete condition is true.

---

## OR Operator: `||`

At least one condition must be `true`.

```rust
fn main() {
    let is_admin = false;
    let is_owner = true;

    if is_admin || is_owner {
        println!("Access granted.");
    }
}
```

Output:

```
Access granted.
```

Although `is_admin` is `false`, `is_owner` is `true`. Therefore, access is granted.

---

## NOT Operator: `!`

The NOT operator reverses a Boolean value.

```
!true  → false

!false → true
```

Example:

```rust
fn main() {
    let is_logged_in = false;

    if !is_logged_in {
        println!("Please log in.");
    }
}
```

Output:

```
Please log in.
```

---

# Important: Conditions Must Be Boolean

In Rust, an `if` condition must always produce a Boolean value:

```
true
```

or:

```
false
```

This is valid:

```rust
fn main() {
    let number = 10;

    if number > 5 {
        println!("The number is greater than 5.");
    }
}
```

This is not valid:

```rust
fn main() {
    let number = 10;

    if number {
        println!("Hello");
    }
}
```

Rust does not automatically treat numbers as `true` or `false`.

---

# Using `if` to Assign a Value

In Rust, `if` is an **expression**. This means it can return a value.

Example:

```rust
fn main() {
    let age = 20;

    let status = if age >= 18 {
        "Adult"
    } else {
        "Minor"
    };

    println!("{}", status);
}
```

Output:

```
Adult
```

Here, the result of the `if-else` expression is stored in `status`.

Notice that there is no semicolon after:

```rust
"Adult"
```

and:

```rust
"Minor"
```

These are the values returned by the blocks.

---

## Both Branches Must Return the Same Type

This is valid:

```rust
let result = if true {
    10
} else {
    20
};
```

Both branches return integers.

This is invalid:

```rust
let result = if true {
    10
} else {
    "Twenty"
};
```

One branch returns an integer, while the other returns a string.

Rust requires both branches to return compatible data types.

---

# 2. Loops in Rust

Loops allow us to execute the same block of code multiple times.

Rust provides three main types of loops:

1. `loop`
2. `while`
3. `for`

---

# The `loop` Loop

The `loop` keyword repeatedly executes a block of code.

### Syntax

```rust
loop {
    // Code to repeat
}
```

Example:

```rust
fn main() {
    loop {
        println!("Hello, Rust!");
    }
}
```

This creates an **infinite loop** because there is no condition to stop it.

The output continues:

```
Hello, Rust!
Hello, Rust!
Hello, Rust!
Hello, Rust!
...
```

---

## Stopping a Loop with `break`

The `break` keyword immediately stops a loop.

Example:

```rust
fn main() {
    let mut number = 1;

    loop {
        println!("{}", number);

        if number == 5 {
            break;
        }

        number += 1;
    }
}
```

Output:

```
1
2
3
4
5
```

### Explanation

The variable starts with:

```rust
let mut number = 1;
```

`mut` is required because the value of `number` changes.

After every repetition:

```rust
number += 1;
```

increases the value by `1`.

This is a shorter way of writing:

```rust
number = number + 1;
```

When:

```rust
number == 5
```

the `break` statement stops the loop.

---

## Returning a Value from a Loop

A Rust `loop` can return a value using `break`.

```rust
fn main() {
    let mut number = 0;

    let result = loop {
        number += 1;

        if number == 5 {
            break number * 2;
        }
    };

    println!("{}", result);
}
```

Output:

```
10
```

Here:

```rust
break number * 2;
```

stops the loop and returns:

```
5 × 2 = 10
```

The returned value is stored in `result`.

---

# The `while` Loop

A `while` loop continues executing as long as its condition remains `true`.

### Syntax

```rust
while condition {
    // Code to repeat
}
```

Example:

```rust
fn main() {
    let mut number = 1;

    while number <= 5 {
        println!("{}", number);

        number += 1;
    }
}
```

Output:

```
1
2
3
4
5
```

### How It Works

Initially:

```
number = 1
```

Rust checks:

```
1 <= 5 → true
```

Therefore, the loop executes.

The value is then increased:

```rust
number += 1;
```

The loop continues until:

```
number = 6
```

Now:

```
6 <= 5 → false
```

Therefore, the loop stops.

---

## Countdown Using `while`

```rust
fn main() {
    let mut number = 5;

    while number > 0 {
        println!("{}", number);

        number -= 1;
    }

    println!("Go!");
}
```

Output:

```
5
4
3
2
1
Go!
```

Here:

```rust
number -= 1;
```

is a shorter way of writing:

```rust
number = number - 1;
```

---

# The `for` Loop

The `for` loop is commonly used to iterate over:

- A range of numbers
- Arrays
- Collections

### Syntax

```rust
for variable in sequence {
    // Code to repeat
}
```

---

## Looping Through a Range

```rust
fn main() {
    for number in 1..5 {
        println!("{}", number);
    }
}
```

Output:

```
1
2
3
4
```

The range:

```rust
1..5
```

includes:

```
1, 2, 3, 4
```

but does not include `5`.

---

## Inclusive Range: `..=`

To include the ending value, use:

```
..=
```

Example:

```rust
fn main() {
    for number in 1..=5 {
        println!("{}", number);
    }
}
```

Output:

```
1
2
3
4
5
```

The range:

```rust
1..=5
```

includes both `1` and `5`.

---

## Exclusive vs Inclusive Range

| Range | Values |
| --- | --- |
| `1..5` | `1, 2, 3, 4` |
| `1..=5` | `1, 2, 3, 4, 5` |

---

# Looping Through an Array

```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];

    for number in numbers {
        println!("{}", number);
    }
}
```

Output:

```
10
20
30
40
50
```

In every iteration, one value from the array is stored in the `number` variable.

---

# The `continue` Keyword

The `continue` keyword skips the current iteration and moves to the next iteration.

Example:

```rust
fn main() {
    for number in 1..=5 {
        if number == 3 {
            continue;
        }

        println!("{}", number);
    }
}
```

Output:

```
1
2
4
5
```

When:

```
number = 3
```

`continue` skips the remaining code for that iteration.

Therefore, `3` is not printed.

---

# `break` vs `continue`

| Keyword | Purpose |
| --- | --- |
| `break` | Completely stops the loop |
| `continue` | Skips only the current iteration |

Example using `break`:

```rust
fn main() {
    for number in 1..=10 {
        if number == 5 {
            break;
        }

        println!("{}", number);
    }
}
```

Output:

```
1
2
3
4
```

Example using `continue`:

```rust
fn main() {
    for number in 1..=5 {
        if number == 3 {
            continue;
        }

        println!("{}", number);
    }
}
```

Output:

```
1
2
4
5
```

---

# Nested Loops

A loop written inside another loop is called a **nested loop**.

```rust
fn main() {
    for row in 1..=3 {
        for column in 1..=2 {
            println!("Row: {}, Column: {}", row, column);
        }
    }
}
```

Output:

```
Row: 1, Column: 1
Row: 1, Column: 2
Row: 2, Column: 1
Row: 2, Column: 2
Row: 3, Column: 1
Row: 3, Column: 2
```

For every iteration of the outer loop, the complete inner loop executes.

---

# Loop Labels

When using nested loops, Rust allows us to give a loop a name called a **loop label**.

A loop label begins with an apostrophe:

```rust
'outer:
```

Example:

```rust
fn main() {
    'outer: for row in 1..=3 {
        for column in 1..=3 {
            if row == 2 && column == 2 {
                break 'outer;
            }

            println!("Row: {}, Column: {}", row, column);
        }
    }
}
```

Here:

```rust
break 'outer;
```

stops the outer loop instead of stopping only the inner loop.

Loop labels are useful when working with nested loops.

---

# Which Loop Should We Use?

| Loop | When to Use |
| --- | --- |
| `loop` | When we want to repeat indefinitely or control stopping manually |
| `while` | When repetition depends on a condition |
| `for` | When iterating through a range, array, or collection |

Examples:

```
Repeat until the user exits
→ loop

Repeat while the password is incorrect
→ while

Print numbers from 1 to 10
→ for
```

---

# Quick Summary

## Conditionals

| Keyword | Purpose |
| --- | --- |
| `if` | Executes code when a condition is true |
| `else if` | Checks additional conditions |
| `else` | Executes when all previous conditions are false |
| `&&` | Both conditions must be true |
| `||` | At least one condition must be true |
| `!` | Reverses a Boolean value |

## Loops

| Keyword | Purpose |
| --- | --- |
| `loop` | Repeats code indefinitely |
| `while` | Repeats while a condition is true |
| `for` | Iterates through ranges and collections |
| `break` | Stops a loop |
| `continue` | Skips the current iteration |

> Conditionals help a program **make decisions**, while loops help a program **repeat tasks**.
>