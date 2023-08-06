## 2. Fibonacci Numbers
Fibonacci sequence is defined as the sequence in which each number is the sum of the two preceding values, where the first two values are 0 and 1.

| *Sequence* | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 | ... |
|------|--|--|--|--|--|--|--|--|--|--|--|--|
| *Fibonacci Value* | 0 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 | 55 | ... |

## Workflow
### 1. Create a new Rust code project and edit the code
``` bash
$ cargo new MLP/FSAwRust-2-Recursion/m2-fibonacci
     Created binary (application) `MLP/FSAwRust-2-Recursion/m2-fibonacci` package
$ geany MLP/FSAwRust-2-Recursion/m2-fibonacci/src/main.rs
...
```
### 2. Create and test a `get_i64` function
This will interactively obtain a number from the user and then display the number.
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
    println!("Enter -1 to exit\n");
    loop {
        // Prompt the user for n.
        let n = get_i64("N: ");
        // If n < 0, break out of the loop.
        if n < 0 {
            break;
        }
        // Calculate the Fibonacci number.
        println!("N: {}\n", n);
    }
}
```
When compiled and run, the following output appeared:

``` bash
$ cargo run
   Compiling m2-fibonacci v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m2-fibonacci)
    Finished dev [unoptimized + debuginfo] target(s) in 0.66s
     Running `/home/sysadmin/MLP/FSAwRust-2-Recursion/m2-fibonacci/target/debug/m2-fibonacci`
Enter -1 to exit
N: 1
N: 1
N: -1
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `/home/sysadmin/MLP/FSAwRust-2-Recursion/m2-fibonacci/target/debug/m2-fibonacci`
Enter -1 to exit
N: 1.3
thread 'main' panicked at 'Error parsing integer: ParseIntError { kind: InvalidDigit }', src/main.rs:16:10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
$
```
Note that integer values are accepted until a value of *-1* is entered. Any non-numeric value entered cases a rust run-time error...
### 3. Create a `fibonacci()` function
The `fibonacci` function was implemented in two parts:
* the `fibonacci()` function accepts an integer indicating the required number in the fibonacci sequence, and
* a helper function `next_fibonacci()` that accepts a countdown index (initially set to the required sequence number) and two consecutive fibonacci numbers. This helper is called recursively and returns the next fibonacci number at each stage in the recursion, finally breaking the recursion after the countdown has expired.

The code for the final version is shown below:

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
// Given the target sequence and two preceeding fibonacci values, 
// return the next fibonacci number
fn next_fibonacci(n: i64, n_1: i64, n_2: i64) -> i64 {
	if n < 3 {
		return n_1 + n_2
	}
	return next_fibonacci(n - 1, n_1 + n_2, n_1)
}
// calculate fibonacci number
fn fibonacci(n: i64) -> i64 {
	if n < 1 { return 0 }
	else if n == 1 {return 1 }
	next_fibonacci(n, 1, 0)
}
// START HERE
fn main() {
    println!("Enter -1 to exit\n");
    loop {
        // Prompt the user for n.
        let n = get_i64("N: ");

        // If n < 0, break out of the loop.
        if n < 0 {
            break;
        }

        // Calculate the Fibonacci number.
        println!("fibonacci({}) = {}\n", n, fibonacci(n));
    }
}
```
### 4. Calculate various *fibonacci* values
The following *bash* commands were entered...

``` bash
$ cd MLP/FSAwRust-2-Recursion/m2-fibonacci/
$ cargo run
   Compiling m2-fibonacci v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m2-fibonacci)
    Finished dev [unoptimized + debuginfo] target(s) in 0.65s
     Running `target/debug/m2-fibonacci`
Enter -1 to exit
N: 92
fibonacci(92) = 7540113804746346429
N: -1
$ 
```

The correct answer was produced with no noticeable delay, but trying to calculate Fibonacci numbers greater than 92 caused runtime overflow error:

``` bash
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/m2-fibonacci`
Enter -1 to exit
N: 93
thread 'main' panicked at 'attempt to add with overflow', src/main.rs:21:16
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
 #### (end)