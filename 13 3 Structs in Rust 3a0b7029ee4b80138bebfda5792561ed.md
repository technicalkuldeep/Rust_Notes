# 13. 3.Structs in Rust

Until now, we've been storing individual values like this:

```rust
let username = String::from("Kuldeep");
let email = String::from("kuldeep@gmail.com");
let active = true;
let sign_in_count = 5;
```

But imagine you're building an application with **thousands of users**.

Will you create four separate variables for every user?

Of course not.

Instead, we group all related information into a single data type.

Rust provides **Structs** for this purpose.

---

# What is a Struct?

A **Struct** is a custom data type that groups multiple related values into a single unit.

Think of it as a **template** for creating objects.

If you've worked with JavaScript, Java, or C++, a Struct is similar to an **Object** or a **Class without methods**.

---

## Real Life Example

Imagine an ID Card.

Every student has

- Name
- Roll Number
- Branch
- Age

Instead of storing them separately,

we create one Student structure.

```
Student

----------------------
Name
Roll Number
Branch
Age
----------------------
```

Similarly,

a User has

- Username
- Email
- Active Status
- Login Count

So we create a User struct.

---

# Creating a Struct

Syntax

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Let's understand every field.

```rust
active: bool
```

Stores whether the user is active.

Example

```
true
```

or

```
false
```

---

```rust
username: String
```

Stores the username.

Example

```
"Kuldeep"
```

---

```rust
email: String
```

Stores the user's email.

---

```rust
sign_in_count: u64
```

Stores how many times the user has logged in.

---

# Creating an Instance (Object)

Once the template is ready,

we can create actual users.

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {

    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

}
```

Here,

```rust
User
```

is the template.

while

```rust
user1
```

is an **instance** (object) created from that template.

---

# Accessing Fields

We use the **dot (`.`)** operator.

```rust
println!("{}", user1.username);
```

Output

```
someusername123
```

Similarly,

```rust
println!("{}", user1.email);

println!("{}", user1.active);

println!("{}", user1.sign_in_count);
```

---

# Where is a Struct Stored?

This is where Rust becomes interesting.

Many beginners think

> "A Struct is either stored on the Stack or on the Heap."
> 

That's **not completely true**.

A Struct is stored **where its fields are stored**.

Let's look at our example.

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

Notice the data types.

| Field | Stored In |
| --- | --- |
| bool | Stack |
| u64 | Stack |
| String | Stack + Heap |
| String | Stack + Heap |

The `bool` and `u64` values are stored directly on the **Stack**.

However, a `String` is special.

The **String object** (pointer, length, capacity) lives on the **Stack**, while the actual text lives on the **Heap**.

This means a Struct can contain both **Stack** and **Heap** data at the same time.

The image below visualizes exactly that.

- `active` (`bool`) → Stored directly on the **Stack**.
- `sign_in_count` (`u64`) → Stored directly on the **Stack**.
- `username` (`String`) → The String metadata (`ptr`, `len`, `capacity`) is on the **Stack**, while the actual characters are on the **Heap**.
- `email` (`String`) → Works exactly the same way.

So a Struct itself is **not purely a Stack object or a Heap object**—its memory layout depends on the types of the fields it contains.

---

# Ownership with Structs

Structs follow the same Ownership rules we've already learned.

Let's first create a Struct that contains only Stack data.

```rust
struct User {
    active: bool,
    sign_in_count: u64,
}
```

Now look at this example.

```rust
struct User {
    active: bool,
    sign_in_count: u64,
}

fn main() {

    let user1 = User {
        active: true,
        sign_in_count: 1,
    };

    print_user(user1);

    println!("{}", user1.active);

}

fn print_user(user: User) {
    println!("{}", user.active);
}
```

Will this compile?

❌ No.

Even though the Struct only contains Stack values,

Rust still moves the entire Struct into the function.

So

```rust
user1
```

becomes invalid after

```rust
print_user(user1);
```

Ownership rules apply to Structs too.

---

# Copy Trait

What if we want Rust to automatically copy the Struct instead of moving it?

We can derive the **Copy** trait.

```rust
#[derive(Copy, Clone)]
struct User {
    active: bool,
    sign_in_count: u64,
}
```

Now the same code works.

Why?

Because every field inside the Struct already supports `Copy`.

Rust simply copies the Struct instead of moving it.

---

# But What Happens When We Add a String?

Now modify the Struct.

```rust
struct User {
    active: bool,
    sign_in_count: u64,
    username: String,
}
```

Now try adding

```rust
#[derive(Copy, Clone)]
```

Rust immediately gives an error.

Why?

Because

```rust
String
```

does **not** implement the `Copy` trait.

A String owns heap memory.

Automatically copying heap memory could be expensive and unsafe.

So Rust refuses.

---

# Clone Trait

Instead,

we use the **Clone** trait.

```rust
#[derive(Clone)]
struct User {
    active: bool,
    sign_in_count: u64,
    username: String,
}
```

Now we can explicitly clone the Struct.

```rust
change_name(user1.clone());

println!("{}", user1.active);
```

Here,

```rust
user1.clone()
```

creates a completely new copy of the Struct.

The original

```rust
user1
```

still remains valid.

---

# Copy vs Clone (Struct Edition)

| Copy | Clone |
| --- | --- |
| Automatic | Manual |
| Works only when every field implements `Copy` | Works even with `String` and other heap data |
| Very fast | Creates a deep copy of heap data |

---

# Chapter Summary

- A **Struct** groups multiple related values into one custom data type.
- A Struct acts as a **template**, while variables like `user1` are **instances** of that template.
- Fields are accessed using the **dot (`.`)** operator.
- A Struct's memory layout depends on the types of its fields—primitive types stay on the **Stack**, while types like `String` store their data on the **Heap**.
- Structs follow the same **Ownership** rules as other Rust values. Passing a Struct to a function moves ownership unless it implements `Copy` or you explicitly `clone()` it.
- `Copy` works only when **every field** inside the Struct implements the `Copy` trait.
- If a Struct contains heap-allocated types like `String`, you must use **`Clone`** when you need an independent copy.

---