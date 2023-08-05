## 2. Fibonacci Numbers
Fibonacci sequence is defined as the sequence in which each number is the sum of the two preceding values, where the first two values are 0 and 1.

| *Sequence* | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 | ... |
|------|--|--|--|--|--|--|--|--|--|--|--|--|
| *Fibonacci Value* | 0 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 | 55 | ... |

## Workflow
1. Create a new Rust code project and edit the code
``` bash
$ cargo new MLP/FSAwRust-2-Recursion/m2-fibonacci
     Created binary (application) `MLP/FSAwRust-2-Recursion/m2-fibonacci` package
$ geany MLP/FSAwRust-2-Recursion/m2-fibonacci/src/main.rs
...
```
2. Create a `get_i64` function to interactively obtain a number from the user
``` rust
use std::io;
use std::io::Write;
// ...
// Prompt the user for an i64.
fn get_i64(prompt: &str) -> i64 {
    print!("{prompt}");
    io::stdout().flush().unwrap();

    let mut str_value = String::new();
    io::stdin()
        .read_line(&mut str_value)
        .expect("Error reading input");

    let trimmed = str_value.trim();
    return trimmed.parse::<i64>()
        .expect("Error parsing integer");
}
fn main() {
    println!("Hello, world!");
}
```
3. Create a `fibonacci()` function
	1. ?
	2. ?
4. Using the supplied `main()` functions, calculate various fibonacci values
	1. ?
	2. ?

