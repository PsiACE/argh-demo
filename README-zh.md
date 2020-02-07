# Argh - 人类友好的命令行参数解析

> 实践出真知。

最近，我注意到一个名为「argh」的命令行参数解析工具，它很轻巧、易用，而且确实对人类友好。

我喜欢它有以下两点原因：

- 代码量小，而且简洁、直观
- 巧妙地利用「Rust」的语法

这里我将会从用户的角度来讲一下「argh」的使用，就当作自己的一个小「提词板」。

首先，我们新建一个项目，很自然地 `cargo new <argh-demo>`。

然后将 argh 添加到我们的依赖项中，像下面这样：

```toml
[dependencies]
argh = "0.1"
```

让我们一起做点什么，比如实现一个简单的命令行加法程序？就像 `a + b` 那个样子。
很显然，我们的函数需要两个参数 `arg1` 和 `arg2`。

```rust
fn add(num1: u16, num2: u16) {
    println!(arg1 + arg2);
}
```

看看 argh 是怎么做的：

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

然后，我们在 `main` 函数中将它们连在一起。

```rust
fn main() {
    let cli: DemoCli = argh::from_env();
    add(cli.num1, cli.num2);
}
```

现在，让我们试着运行一下 `cargo run`，看看会发生什么：

```text
Required options not provided:
    --num1
    --num2
```

试试 `--help` 选项，`target/debug/argh-demo --help`：

```text
Usage: target/debug/argh-demo --num1 <num1> --num2 <num2>

Add two numbers

Options:
  --num1            the first number.
  --num2            the second number
  --help            display usage information
```

好吧，看来我们不得不加上参数 `target/debug/argh-demo --num1 1 --num2 2`：

```text
1 + 2 = 3
```