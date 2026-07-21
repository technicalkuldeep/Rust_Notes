# 3. Installing Rust and Setting Up the IDE

Before writing Rust programs, we need to:

1. Install Rust on our computer.
2. Set up an IDE or code editor to write Rust code easily.

---

## 1. Installing Rust

Rust can be installed using **`rustup`**, the official Rust installation and version-management tool.

### What is `rustup`?

`rustup` is used to:

- Install Rust
- Update Rust to newer versions
- Manage different Rust versions
- Install additional Rust development tools

### Installation Steps

1. Open the official Rust installation website:

[Install Rust](https://www.rust-lang.org/tools/install?utm_source=chatgpt.com)

1. Download and run **`rustup-init.exe`**.
2. A terminal window will open. Select the default installation option:

```
1) Proceed with standard installation (default)
```

Press:

```
Enter
```

1. After the installation is complete, restart the terminal or VS Code.

---

### Verify the Installation

Open the terminal and run:

```bash
rustc --version
```

Example output:

```
rustc 1.x.x
```

If the Rust version is displayed, Rust has been installed successfully.

---

## Important Tools Installed with Rust

Installing Rust through `rustup` also installs important tools:

### `rustc` — Rust Compiler

`rustc` converts Rust code into machine-executable code.

```bash
rustc --version
```

---

### `cargo` — Rust Package Manager

Cargo is Rust's project and package-management tool.

It is used to:

- Create Rust projects
- Build projects
- Run Rust programs
- Install and manage external packages

```bash
cargo --version
```

> We will use **Cargo** regularly while learning Rust.
> 

Cargo is similar to `npm`. It’s a package manager for rust

---

## 2. IDE Setup

An **IDE (Integrated Development Environment)** or code editor provides tools that make writing, running, and debugging code easier.

For Rust development, we will use:

> **Visual Studio Code (VS Code)**
> 

After installing VS Code, open the **Extensions** section using:

```
Ctrl + Shift + X
```

Search for and install the following extensions:

---

### 1. rust-analyzer

`rust-analyzer` is the main VS Code extension for Rust development.

It provides:

- Code suggestions
- Auto-completion
- Syntax highlighting
- Error detection
- Type information
- Code navigation

For example, if we make a mistake, `rust-analyzer` may show the error directly inside the editor before we run the program.

> `rust-analyzer` makes writing Rust code easier and faster.
> 

---

### 2. CodeLLDB

`CodeLLDB` is a debugging extension.

A **debugger** helps us run a program step by step and inspect what is happening inside it.

It provides:

- Breakpoints
- Step-by-step code execution
- Variable inspection
- Error debugging

> CodeLLDB helps us find and understand problems in our Rust programs.
> 

---

### 3. Even Better TOML

Rust projects contain a file named:

```
Cargo.toml
```

The `Cargo.toml` file stores important project information, such as:

- Project name
- Project version
- Rust edition
- External dependencies

The **Even Better TOML** extension provides:

- Syntax highlighting
- Auto-completion
- Error detection
- Better support for `.toml` files

> TOML stands for **Tom's Obvious, Minimal Language**.
> 

---

## Quick Summary

| Tool/Extension | Purpose |
| --- | --- |
| `rustup` | Installs, updates, and manages Rust |
| `rustc` | Compiles Rust programs |
| `cargo` | Creates, builds, runs, and manages Rust projects |
| `rust-analyzer` | Provides code suggestions and detects errors |
| `CodeLLDB` | Helps debug Rust programs |
| Even Better TOML | Provides better support for `Cargo.toml` files |

After completing these steps, our computer is ready for Rust development.