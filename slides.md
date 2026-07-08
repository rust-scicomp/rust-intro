---
marp: true
theme: default
title: An introduction to Rust - Ignacia Fierro Piccardo & Matthew Scroggs
---

![bg right fit](assets/qr.svg)

# [rust-scicomp.github.io/rust-intro/](https://rust-scicomp.github.io/rust-intro/)

---

<!--
footer: Slide template setup borrowed from [Sam Cunliffe](https://github.com/samcunliffe).
-->

![width:10em](assets/science-ferris-transparent.png)

# An introduction to Rust

[Ignacia Fierro Piccardo](https://github.com/ignacia-fp) & [Matthew Scroggs](https://mscroggs.co.uk)

![h:31](https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-nc-sa.png)


---

<!--
paginate: true
footer: An introduction to Rust (rust-scicomp.github.io/rust-intro)
-->

# Plan for today

0. Hello world
1. Basic Rust syntax
   - variables
   - if, else
   - functions
   - loops
   - vectors
3. Reasons to like Rust

---

# Getting started

- In terminal:
    - `cargo init`
    - `cargo run`
- Can instead do `cargo init --lib` to initialise a library
---

# Getting started

- `cargo init` creates:
    - **Cargo.toml**
        - You can add dependencies to this file
    - **src/main.rs** with Hello World code in it
    - By default, it also initialises a git repo in your folder, creating **.git/** and **.gitignore**

---

# Basic syntax

---

# Basic syntax #1: variables

```rust
let n = 5;
println!("n is {n}");
```

---

# Basic syntax #1: variables

Note: all variables are constant by default. This will give a compiler error:

```rust
let n = 5;
n += 1;
println!("n is {n}");
```

Instead, you must use the `mut` keyword to make the variable mutable:

```rust
let mut n = 5;
n += 1;
println!("n is {n}");
```

---

# Basic syntax #2: `if`, `else`

```rust
if sides == 1 {
    panic!("A polygon cannot have 1 side");
} else if sides == 2 {
    panic!("A polygon cannot have 2 sides");
} else {
    println!("Creating a polygon with {sides} sides")
}
```

- No `()` around conditions (like Python)
- `{}` around code blocks and `;` at ends of lines (like C++)
- Indentation is for style (unlike Python

---

# Basic syntax #3: functions

You can do either:

```rust
fn one_more(n: i32) -> i32 {
    return n + 1;
}
```

Or:

```rust
fn one_more(n: i32) -> i32 {
    n + 1
}
```

When making a library, the keyword `pub` can be used before `fn` to define public functions.

---

# Activity

Write a function that takes the number of sides as an input and returns the size
of an angle in a regular polygon with that number of sides.

<br />

Hint: `f64` and `f32` are the Rust types for 64 and 32 bit floats

---

# Basic syntax #4: loops

- `while` loop:
    ```rust
    while condition {
        println!("Print this");
    }
    ```
- `for` loop:
    ```rust
    for n in 0..5 {
        println!("The next number is {n}")
    }

---

# Basic syntax #4: loops

- `loop` loops forever:
    ```rust
    loop {
        println!("Still going...");
    }
    ```

---

# Basic syntax #5: `Vec`

- A `Vec` is similar to a Python `list`. They can be created using `vec!`:
    ```rust
    let ten_zeros = vec![0.0; 10];
    println!("{ten_zeros:?}");
    let mut odds = vec![1, 3, 5, 7, 9];
    println!("{odds:?}");
    ```
- `[]` can be used to get an item:
    ```rust
    println!("{}", odds[1]);
    ```
- `push` adds an item to the end of a `Vec`:
    ```rust
    odds.push(11);
    println!("{odds:?}");
    ```

---

# Basic syntax #5: `Vec`

- Functions can return a `Vec`:
    ```rust
    fn f(number_of_zeros: usize) -> Vec<f64> {
        vec![0.0; number_of_zeros]
    }
    ```
---

# Basic syntax #5: Maths functions

- The `f32` and `f64` types support lots of common functions:
    ```rust
    let angle = 2.0;
    println!("{}", f64::sin(angle));
    println!("{}", angle.sin());

    println!("{}", f32::sqrt(2.0));
    println!("{}", f64::sqrt(2.0));

    println!("{}", std::f64::consts::PI);
    ```

---

# Activity

Write a function that takes the number of sides as an input and output the coordinates
of the vertices of a regular polygon with that number of sides.

---

# Reasons to like Rust

---

# Reasons to like Rust #1: `trait`s

A trait is use to define an abstract interface that could be implemented. For example:

```rust
/// Any polygon
trait Polygon {
    /// Number of vertices
    fn vertex_count(&self) -> int;

    /// The area of the polygon
    fn area(&self) -> f64;

    /// The perimeter of the polygon
    fn perimeter(&self) -> f64;
}
```

These traits can them be `impl`emented for a `struct`

---

# Reasons to like Rust #2: `&impl Trait`

Functions can use "any item that implements this trait" as an input type:

```rust
struct Prism {
}

fn make_prism(&impl Polygon) -> Prism {
    todo!();
}
```

This means your function can be used for objects in someone else's library as long as the other person has implemented your `Polygon` trait.

---

# Reasons to like Rust #3: package management

Adding dependencies is easy (Cargo.toml):
```toml
[dependencies]
quadraturerules = "0.9.0"
itertools = "0.14.*"
rlst = { version = "0.6" }
mpi = { version = "0.8.0", optional = true }
serde = { version = "1", features = ["derive"], optional = true }
```

<br />

`cargo publish` can be used to make your crate available on [crates.io](https://crates.io).

---

# Reasons to like Rust #4: enums can hold values

```rust
pub enum Transformation {
    /// An identity transformation
    Identity,
    /// A permutation
    Permutation(Vec<usize>),
}
```

---

# Reasons to like Rust #5: `break 'a`

`'` can used to label a loop then tell break which loop to break.
```rust
'a: for i in 2..100 {
    for j in 2..100 {
        if i * j == 56 {
            println!("{i} {j}");
            break 'a;
        }
    }
}
```

---

# Reasons to like Rust #6: Borrowing

Every item in Rust has an owner. If an item is passed into a function, the function takes ownership

```rust
let a = vec![0, 1];
function(a);

// This will cause a compiler error
println!("{:?}", a);
```

---

# Reasons to like Rust #6: Borrowing

`&` can be used to "borrow" items to let a function use them without taking ownership:

```rust
let a = vec![0, 1];
function(&a);

// This will NOT cause a compiler error
println!("{:?}", a);
```

`&mut` can be used to let functions have a mutable borrow.

---

# Reasons to like Rust #7: Memory safety

The compiler checks that at any time, any item has either:

- one mutable borrow
- one or more non-mutable borrow

This is an important part of how the compiler enforces memory safety.

---

# Reasons to like Rust #8: `cargo fmt` and `cargo clippy`

- Code formatting and linting out of the box

---

# Reasons to like Rust #9: (Reasonably) simple Python wrapping

- Using [PyO3](https://pyo3.rs/) or [Maturin](https://github.com/pyo3/maturin).

---

# Where to learn more

- Rust book
    - free at [doc.rust-lang.org/book](https://doc.rust-lang.org/book/)
    - non-free paper versions also available
- Ask un on Zulip!

---

---

![bg right fit](assets/qr.svg)

![width:10em](assets/science-ferris-transparent.png)

# Thanks for coming!

[rust-scicomp.github.io/rust-intro](https://rust-scicomp.github.io/rust-intro/)

![h:31](https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-nc-sa.png)

