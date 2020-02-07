# Argh Demo

> Real knowledge comes from practice.

Recently, I noticed a command line parameter parsing tool called "argh", which is lightweight, easy to use, and really human friendly.

I like it for two reasons:

- Small amount of code, simple and intuitive
- Ingenious use of "Rust" syntax

Here I will talk about the use of "argh" from the perspective of the user, as a small "prompt board" for myself.

First, let's create a new project. Naturally, `cargo new <argh-demo>`.

Then add argh to our dependencies like this:

```toml
[dependencies]
argh = "0.1"
```

Let's do something together, such as implementing a simple command line adder? It's like "a + b".
Obviously, our function takes two arguments, `num1` and `num2`.

```rust
fn add(num1: u16, num2: u16) {
    println!(arg1 + arg2);
}
```

See how *argh* does it:

```rust
#[derive(FromArgs)]
/// Add two numbers
struct DemoCli {
    /// the first number.
    #[argh(option)]
    num1: u16,

    /// the second number
    #[argh(option)]
    num2: u16,
}
```

We then connected them together in the main function.

```rust
fn main() {
    let cli: DemoCli = argh::from_env();
    add(cli.num1, cli.num2);
}
```

Now, let's try to run `cargo run` and see what happens:

```text
Required options not provided:
    --num1
    --num2
```

Try the `--help` option,`target / debug / argh-demo --help`:

```text
Usage: target/debug/argh-demo --num1 <num1> --num2 <num2>

Add two numbers

Options:
  --num1            the first number.
  --num2            the second number
  --help            display usage information
```

Well, it seems we have to add all the options `target/debug/argh-demo --num1 1 --num2 2`：

```text
1 + 2 = 3
```