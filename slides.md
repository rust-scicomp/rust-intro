---
marp: true
theme: default
title: An introduction to Rust - Matthew Scroggs
---

# As you arrive

On a post it note write a reason why you're interested in learning Rust.

Stick the note on the whiteboard.

---

![bg right fit](assets/qr.svg)

# [mscroggs.github.io/rust-intro](https://mscroggs.github.io/rust-intro/)

---

<!--
footer: Based on material developed with [Ignacia Fierro Piccardo](https://github.com/ignacia-fp). Slide template setup borrowed from [Sam Cunliffe](https://github.com/samcunliffe).
-->

![width:10em](assets/science-ferris-transparent.png)

# An introduction to Rust

[Matthew Scroggs](https://mscroggs.co.uk)

![h:31](https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-nc-sa.png)


---

<!--
paginate: true
footer: An introduction to Rust (mscroggs.github.io/rust-intro)
-->

# Plan for today

0. Hello world
1. Basic Rust syntax
2. Testing in Rust
3. Traits and structs
4. Reasons to like Rust

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
fn one_more(n: int) -> int {
    return n + 1;
}
```

Or:

```rust
fn one_more(n: int) -> int {
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

# Testing in Rust

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```
then run `cargo test`

- `#[cfg(test)]` means the `mod` will only be compiled if running `cargo test`
- Any function marked with `#[test]` will be run by `cargo test`

---

# Activity

Add a test that checks the output of your angle function.

<br />

You may want to:

- Add `approx = "0.5"` as a dependency
- Add `use approx::*;` at the start of your tests
- Use `assert_relative_eq!(a, b);` to assert that two values are close
    - `assert_relative_eq!(a, b, epsilon=1e-8);` to adjust tolerance

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
        vec![0.0; number_of_zeros];
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

Write a function that takes the number of sides as an input and the coordinates
of the vertices of a regular polygon with that number of sides.

---

# Probably a good time for a break

... I hope I remembered to make cake

---

# `trait`s

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

---

# `struct`s

A struct can be used to store data of a mixture of types.

```rust
/// A regular polygon
struct RegularPolygon {
    /// Number of sides
    n_sides: int,
}


let triangle = RegularPolygon { n_sides: 3 };
```

---

# `struct`s

Functions can be implemented for a struct. The function `new` is often defined to make a nicer user interface:

```rust
impl RegularPolygon {
    /// Create a new
    pub fn new(number_of_sides: int) -> Self {
        Self { n_sides: number_of_sides }
    }
}


let triangle = RegularPolygon::new(3);
```

---

# `struct`s

When initialising a struct `variable: variable` can be simplified to just `variable`, eg:

```rust
impl RegularPolygon {
    /// Create a new
    pub fn new(n_sides: int) -> Self {
        Self { n_sides }
    }
}
```

---

# `struct`s

Traits can be implemented for a struct:

```rust
impl Polygon for RegularPolygon {
    fn vertex_count(&self) -> usize {
        self.n_sides
    }
    fn area(&self) -> f64 {
        todo!();
    }
    fn perimeter(&self) -> f64 {
        todo!();
    }
}
```

---

# Activity

Finish implementing the `Polygon` trait for `RegularPolygon`:

```rust
impl Polygon for RegularPolygon {
    fn vertex_count(&self) -> usize {
        self.n_sides
    }
    fn area(&self) -> f64 {
        todo!();
    }
    fn perimeter(&self) -> f64 {
        todo!();
    }
}
```

... then test it!

---

# Reasons to like Rust

---

# Reasons to like Rust #1: `&impl Trait`

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

# Reasons to like Rust #2: package management

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

# Reasons to like Rust #3: Any code block can return a value

```rust
let n_points = if degree < 2 {
    0
} else {
    degree - 2
};
```

```rust
fn collatz(n: int) -> int {
    if n % 2 == 0 {
        n / 2
    } else {
        3 * n + 1
    }
}
```

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

# Reasons to like Rust #5: `if let`

```rust
let t = Transformation::Identity;

if let Permutation(p) = t {
    todo!();
}
```


---

# Reasons to like Rust #6: `break 'a`

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

# Reasons to like Rust #7: Borrowing

Every item in Rust has an owner. If an item is passed into a function, the function takes ownership

```rust
let a = vec![0, 1];
function(a);

// This will cause a compiler error
println!("{:?}", a);
```

---

# Reasons to like Rust #7: Borrowing

`&` can be used to "borrow" items to let a function use them without taking ownership:

```rust
let a = vec![0, 1];
function(&a);

// This will NOT cause a compiler error
println!("{:?}", a);
```

`&mut` can be used to let functions have a mutable borrow.

---

# Reasons to like Rust #8: Memory safety

The compiler checks that at any time, any item has either:

- one mutable borrow
- one or more non-mutable borrow

This is an important part of how the compiler enforces memory safety.

---

# Reasons to like Rust #9: `cargo fmt` and `cargo clippy`

- Code formatting and linting out of the box

---

# Where to learn more

- Rust book
    - free at [doc.rust-lang.org/book](https://doc.rust-lang.org/book/)
    - non-free paper versions also available
- Scientific Computing in Rust 2026, 8- 10 July, virtual free event
    - [scientificcomputing.rs/2026](https://scientificcomputing.rs/2026)
    - Talks from previous years on [YouTube](https://www.youtube.com/@ScientificComputinginRust)
- Ask me!

---

# Feedback and conclusions

- One post it notes, write:
    - One thing that you learned today
    - How this session has affected your desire to learn Rust
    - Any suggestions for next time I run something like this, eg:
        - anything that was unclear
        - anything that we didn't cover that you'd like to have seen
        - anything that we spent too long on
    - Any suggestions for follow on events if you want to do more Rust

---

![bg right fit](assets/qr.svg)

![width:10em](assets/science-ferris-transparent.png)

# Thanks for coming!

[mscroggs.github.io/rust-intro](https://mscroggs.github.io/rust-intro/)

![h:31](https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-nc-sa.png)

