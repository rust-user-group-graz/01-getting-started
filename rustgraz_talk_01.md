---
author: Lukas Prokop
title: Rust Graz – 01 Getting started
date: July 25, 2019
---

---

## Setup

Either [play.rust-lang.org](https://play.rust-lang.org/) or [macOS/Linux/UNIX]

    curl https://sh.rustup.rs -sSf | sh

*rustup:* Rust installer and version management tool <br/>
Use `rustup self uninstall` to uninstall

1. Determines host triple (arch&amp;vendor&amp;os): `x86_64-unknown-linux-gnu`
2. Installs rust tools to `/root/.cargo/bin`
3. Adds `/root/.cargo/bin` to PATH
4. Sets toolchain to `stable`

---

## Resources

* [doc.rust-lang.org](https://doc.rust-lang.org/)
* [doc.rust-lang.org/book/](https://doc.rust-lang.org/book/) or `rustup doc --book`
* [doc.rust-lang.org/stable/rust-by-example/](https://doc.rust-lang.org/stable/rust-by-example/)
* [exercism.io/tracks/rust](https://exercism.io/tracks/rust)
* [newrustacean.com](https://newrustacean.com/) (stopped in May 2019)
* [rusty-spike.blubrry.net](https://rusty-spike.blubrry.net/)
* [“Help Wanted: Research Questions in Rust - Aaron Turon - OPLSS 2018”](https://www.youtube.com/watch?v=hY0gyzItyEo)

IMHO: huge bunch of community resources!

---

## Tooling

In general: many features provided by the language (e.g. unlike C++ and Doxygen).

* **cargo** is rust's package (= crate) manager
* **rls** is rust's language server
* **rustfmt** formats/normalizes rust programs
* **rustdoc** generates documentation from comments
* **rust-gdb/rust-lldb** for debugging

---

## Tooling

LLVM-based compiler and toolchain. <br/>
IMHO use an IDE for programming:

* IntelliJ IDEA with [Rust plugin](https://intellij-rust.github.io/)
* CLion with same plugin
* Visual Studio Code with `rust-lang.rust` extension
* …

---

## Releases

`rustup install {stable,beta,nightly}`

* **Stable** (1.36.0, default)
* **Beta** (1.37.0-beta.6, tests for stable release)
* **Nightly** (1.38.0-nightly, unstandardized features)

Rapid 6-week release schedule <br/>
set default release with:<br/> `rustup default {stable,beta,nightly}`

---

## Editions

<table>
<tr><th>January 2012</th><td>⇒</td><td>first numbered pre-alpha release</td></tr>
<tr><th>May 2015</th><td>⇒</td><td>1.0.0 release (stable std API), “Rust 2015”</td></tr>
<tr><th>Dec 2018</th><td>⇒</td><td>1.31 release, “Rust 2018”</td></tr>
</table>

Editions (“long-term releases with a theme”):

* Rust 2015 ‘stability’
* Rust 2018 ‘productivity’

---

## Syntactic notes for today

---

### Hello World

```rust
fn main() {
    println!("Hello World!");
}
```

---

### Hello World

```rust
fn main() {
    println!("Hello World!");
}
```

* `fn` for *function*
* C-like block structures, 4 spaces for indentation
* main function w/o return value
* `!` because `println` is a macro, not a function
* strings with `"`
* C-like semicolon after statements

---

### Formatting

```rust
fn main() {
    println!("Hello {:06b} {}!", 9, "rustaceans");
}
```

* featuring string formatting
* inspired by C's `printf` and Python's `str.format`
* `06` for left-padding with `0` character
* `b` for binary representation

```rust
fn main() {
    println!("Hello {no:06b} {who}!",
             no = 9, who = "rustaceans");
}
```

---

### Assignment

```rust
fn main() {
    let attending_rustaceans = 9;
    println!("Hello {} rustaceans!", attending_rustaceans);
}
```

* `let` keyword for assignment
* underlines in variables by convention

---

### Assignment

```rust
fn main() {
    let attending_rustaceans = 9;
    attending_rustaceans += 1;
    println!("Hello {} rustaceans!", attending_rustaceans);
}
```

---

### Wait… what?

<!-- unbuffer rustc test2.rs 1&>2 | aha > test2.html -->

<pre>
<span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">error[E0384]</span><span style="font-weight:bold;">: cannot assign twice to immutable variable `attending_rustaceans`</span>
 <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">--&gt; </span>test2.rs:3:5
  <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">|</span>
<span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">2</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">| </span>    let attending_rustaceans = 9;
  <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">| </span>        <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">--------------------</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">first assignment to `attending_rustaceans`</span>
<span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">3</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">| </span>    attending_rustaceans += 1;
  <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:blue;">| </span>    <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">^^^^^^^^^^^^^^^^^^^^^^^^^</span> <span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">cannot assign twice to immutable variable</span>

<span style="font-weight:bold;"></span><span style="font-weight:bold;filter: contrast(70%) brightness(190%);color:red;">error</span><span style="font-weight:bold;">: aborting due to previous error</span>

<span style="font-weight:bold;">For more information about this error, try `rustc --explain E0384`.</span>
</pre>

---

### Immutability by default!

---

### Assignment

```rust
fn main() {
    let mut attending_rustaceans = 9;
    attending_rustaceans += 1;
    println!("Hello {} rustaceans!", attending_rustaceans);
}
```

* `mut` keyword for *mutability*

---

### How to compile?

---

<pre>
user@sys / % <strong>cargo new hello_world --bin</strong>
     Created binary (application) `hello_world` package
user@sys / % <strong>cd hello_world/</strong>
user@sys /hello_world % <strong>ls -la</strong>
total 8
drwxrwxr-x 1 user user   54 Jul 25 12:03 .
drwxrwxrwt 1 root       root       1356 Jul 25 12:03 ..
drwxrwxr-x 1 user user   82 Jul 25 12:03 .git
-rw-rw-r-- 1 user user   19 Jul 25 12:03 .gitignore
-rw-rw-r-- 1 user user  131 Jul 25 12:03 Cargo.toml
drwxrwxr-x 1 user user   14 Jul 25 12:03 src
</pre>

---

<pre>
user@sys /hello_world % <strong>cat Cargo.toml</strong>
[package]
name = "hello_world"
version = "0.1.0"
authors = ["user &lt;my@git-email.addr&gt;"]
edition = "2018"

[dependencies]
user@sys /hello_world % <strong>cd src/</strong>
user@sys /hello_world/src % <strong>cat main.rs</strong>
fn main() {
    println!("Hello, world!");
}
user@sys /hello_world/src % <strong>cargo run</strong>
   Compiling hello_world v0.1.0 (/hello_world)
    Finished dev [unoptimized + debuginfo] target(s) in 0.42s
     Running `/hello_world/target/debug/hello_world`
Hello, world!
</pre>