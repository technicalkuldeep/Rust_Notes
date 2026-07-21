# 4. Initializing a Rust Project Locally

After installing Rust and setting up VS Code, we can create our first Rust project.

Rust provides a tool called **Cargo** to create and manage projects.

Cargo helps us:

- Create new Rust projects
- Build and compile Rust code
- Run Rust programs
- Manage external packages or dependencies

---

## Creating a Rust Project

First, create a new folder for the Rust project:

```bash
mkdir rust-project
```

Move inside the newly created folder:

```bash
cd rust-project
```

Initialize a Rust project using Cargo:

```bash
cargo init
```

The complete process is:

```bash
mkdir rust-project
cd rust-project
cargo init
```

Here:

- `mkdir rust-project` → Creates a new folder named `rust-project`
- `cd rust-project` → Moves the terminal inside the folder
- `cargo init` → Initializes a new Rust project inside the folder

After running `cargo init`, Cargo automatically creates the basic files required for a Rust application.

---

## Default Project Structure

```
rust-project/
│
├── src/
│   └── main.rs
│
├── Cargo.toml
│
└── .gitignore
```

### `src` Folder

The `src` folder contains the Rust source code.

`src` stands for **source**.

---

### `main.rs`

The `main.rs` file contains the main code of a Rust application.

```
src/main.rs
```

The `.rs` extension represents a **Rust source-code file**.

---

### `Cargo.toml`

The `Cargo.toml` file contains information about the project, such as:

- Project name
- Project version
- Rust edition
- External packages or dependencies

Example:

```toml
[package]
name = "rust-project"
version = "0.1.0"
edition = "2024"

[dependencies]
```

---

# Application vs Library

Rust projects can mainly be initialized as:

1. Application
2. Library

---

## 1. Application

When we run:

```bash
cargo init
```

Cargo creates an **application project by default**.

An application is a program that can execute independently and perform a task for the end user.

Examples include:

- Calculator
- Command-line tool
- Web server
- Desktop application
- Game

An application contains:

```
src/main.rs
```

When an application is compiled, Rust produces an **executable binary file**.

> A binary is a compiled executable program that can run on a computer.
> 

---

## 2. Library

To initialize a Rust library, use:

```bash
cargo init --lib
```

A library contains reusable code that can be used by other programs or developers.

Examples include:

- Utility functions
- Mathematical libraries
- Database libraries
- Authentication libraries

A library project contains:

```
src/lib.rs
```

instead of:

```
src/main.rs
```

A library is generally not designed to run independently. It provides reusable functionality that other applications can use.

---

## Application vs Library

| Application | Library |
| --- | --- |
| Created using `cargo init` | Created using `cargo init --lib` |
| Contains `src/main.rs` | Contains `src/lib.rs` |
| Can execute independently | Usually used by other programs |
| Produces an executable binary | Produces reusable library code |
| Contains a `main()` function | Does not require a `main()` function |

---

# Hello World Program

After running:

```bash
cargo init
```

Cargo automatically creates a basic Hello World program inside:

```
src/main.rs
```

The code looks like this:

```rust
fn main() {
    println!("Hello, world!");
}
```

---

## Code Explanation

### `fn`

```rust
fn
```

The `fn` keyword is used to create or define a **function** in Rust.

---

### `main()`

```rust
main()
```

`main` is a special function.

It is the **entry point** of a Rust application, which means program execution starts from the `main()` function.

---

### Curly Braces `{ }`

```rust
{

}
```

Curly braces define the body or block of a function.

All the code written inside these braces belongs to the `main()` function.

---

### `println!()`

```rust
println!("Hello, world!");
```

`println!` displays text on the terminal.

Here:

- `println` means **print line**
- `!` indicates that `println!` is a Rust **macro**
- `"Hello, world!"` is the text that will be printed
- `;` marks the end of the statement

Output:

```
Hello, world!
```

---

# Running the Rust Project

To compile and run the project, use:

```bash
cargo run
```

Output:

```
Compiling rust-project
Finished
Running target/debug/rust-project

Hello, world!
```

The `cargo run` command performs two operations:

1. Compiles the Rust code
2. Runs the compiled application

---

## Quick Summary

| Command | Purpose |
| --- | --- |
| `mkdir rust-project` | Creates a project folder |
| `cd rust-project` | Moves inside the project folder |
| `cargo init` | Initializes a Rust application |
| `cargo init --lib` | Initializes a Rust library |
| `cargo run` | Compiles and runs the Rust application |

> By default, `cargo init` creates an application containing `src/main.rs`, while `cargo init --lib` creates a reusable library containing `src/lib.rs`.
>