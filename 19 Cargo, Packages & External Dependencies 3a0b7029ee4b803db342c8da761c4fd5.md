# 19. Cargo, Packages & External Dependencies

---

So far, we've written small Rust programs using only the features provided by Rust itself.

But real-world applications need much more.

For example, you might want to

- Make HTTP requests
- Parse JSON
- Connect to a database
- Build a web server
- Create a GUI
- Work with dates and time

Should we write all of these from scratch?

**No.**

Rust has thousands of libraries that other developers have already written.

Cargo helps us use them easily.

---

# What is Cargo?

**Cargo** is Rust's official **package manager** and **build tool**.

Think of Cargo as your personal assistant.

It helps you

- Create new projects
- Download libraries
- Build programs
- Run programs
- Test code
- Manage project dependencies

Instead of doing everything manually,

you simply ask Cargo.

---

## Real-Life Analogy

Imagine you're building a house.

Without Cargo, you'd have to

- Make your own bricks
- Make your own cement
- Build your own tools

😂

That would take forever.

Instead,

you buy ready-made materials.

Cargo works the same way.

It downloads ready-made Rust libraries so you don't have to reinvent everything.

---

# Creating a Project

You've already seen this command.

```bash
cargo new my_project
```

Cargo automatically creates

```
my_project/

├── Cargo.toml
└── src/
    └── main.rs
```

Everything needed to start a Rust project is created for you.

---

# What is a Package?

A **Package** is simply a Rust project managed by Cargo.

When you create

```bash
cargo new calculator
```

the entire folder

```
calculator/
```

is called a **Package**.

Every package contains a special file called

```
Cargo.toml
```

---

# What is `Cargo.toml`?

`Cargo.toml` is the **configuration file** for your Rust project.

It tells Cargo

- Project name
- Version
- Rust edition
- Dependencies

Example

```toml
[package]
name = "calculator"
version = "0.1.0"
edition = "2024"
```

This section describes your project.

---

# What are Dependencies?

A **Dependency** is simply another Rust library that your project uses.

Instead of writing everything yourself,

you reuse code written by other developers.

Think of it like installing an app on your phone.

Your phone already has basic features,

but if you want video editing,

you install a video editing app.

Similarly,

if your Rust program needs extra functionality,

you add a dependency.

---

# Adding an External Dependency

Suppose we want to generate random numbers.

Rust doesn't include this functionality in the standard library.

Instead, we'll use a popular crate called

```
rand
```

Open

```
Cargo.toml
```

and add

```toml
[dependencies]
rand = "0.9"
```

Now save the file.

That's it.

Cargo will automatically download the library.

---

# Using the Dependency

Now we can write

```rust
use rand::Rng;

fn main() {
    let mut rng = rand::rng();

    let number = rng.random_range(1..=10);

    println!("Random Number: {}", number);
}
```

Output (changes every run)

```
Random Number: 7
```

or

```
Random Number: 2
```

or

```
Random Number: 10
```

Cargo downloaded the library automatically.

You didn't have to manually download anything.

---

# Where Do Dependencies Come From?

Most Rust libraries are published on

> **crates.io**
> 

Think of it as Rust's App Store.

Thousands of developers publish reusable Rust libraries there.

Examples

- `rand`
- `serde`
- `tokio`
- `reqwest`
- `clap`

Whenever you add one to

```
Cargo.toml
```

Cargo downloads it automatically.

---

# What is a Crate?

You'll often hear the word

> **Crate**
> 

A **Crate** is simply a Rust library or executable package.

Examples

- `rand`
- `serde`
- `tokio`

are all crates.

When people say

> "Install this crate"
> 

they simply mean

> "Add this library as a dependency."
> 

---

# How Does Cargo Know What to Download?

When you write

```toml
[dependencies]
rand = "0.9"
```

Cargo

↓

Connects to crates.io

↓

Downloads the correct version

↓

Builds it

↓

Makes it available in your program

All of this happens automatically.

---

# Running the Project

Compile and run

```bash
cargo run
```

Compile only

```bash
cargo build
```

Compile optimized version

```bash
cargo build --release
```

Check for errors without building

```bash
cargo check
```

Run tests

```bash
cargo test
```

Format code

```bash
cargo fmt
```

These are the commands you'll use almost every day.

---

# Visual Flow

```
You Write Code
       │
       ▼
Cargo.toml
       │
       ▼
Cargo Reads Dependencies
       │
       ▼
Downloads Crates
       │
       ▼
Builds Project
       │
       ▼
Runs Program
```

---

# Important Terms

| Term | Meaning |
| --- | --- |
| Cargo | Rust's build tool and package manager |
| Package | A Rust project managed by Cargo |
| Cargo.toml | Configuration file for the project |
| Dependency | An external library used by your project |
| Crate | A Rust library or executable package |

---

# Why is Cargo So Popular?

Cargo is one of the reasons Rust developers love the language.

With a few simple commands, it can

- Create projects
- Download libraries
- Build programs
- Run tests
- Format code
- Manage versions
- Publish your own libraries

Everything is integrated into a single tool.

---

# Chapter Summary

- **Cargo** is Rust's official build tool and package manager. It helps create projects, compile code, run programs, manage dependencies, and much more.
- A **Package** is a Rust project managed by Cargo.
- Every package contains a **`Cargo.toml`** file, which stores information about the project and its dependencies.
- A **Dependency** is an external library that your project uses instead of writing everything from scratch.
- Most Rust libraries are distributed as **Crates** through **crates.io**, Rust's official package registry.
- By adding a dependency to `Cargo.toml`, Cargo automatically downloads, builds, and makes the library available to your project.

---

### 💡 Easy Way to Remember

```
Cargo
   ↓
Manages Your Rust Project

Package
   ↓
Your Rust Project

Cargo.toml
   ↓
Project Configuration

Crate
   ↓
A Rust Library

Dependency
   ↓
A Crate Your Project Uses
```

If you remember this chain—

> **Cargo manages Packages, Packages use Crates as Dependencies, and everything is configured in `Cargo.toml`**—
> 

you'll understand how almost every Rust project is organized.