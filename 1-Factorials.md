# 1. Factorials
The outcome for this milestone is a recursive `factorial` function that calculates a number’s factorial and is used as a as a warmup for the more complicated recursive functions that follow...
## Workflow
1. Create a new Rust object for the first milestone using `cargo`
``` bash
$ cargo new MLP/FSAwRust-2-Recursion/M1-Recursion
     Created binary (application) `MLP/FSAwRust-2-Recursion/m1-recursion` package
```

2.  Open the new rust code object in the code editor `geany MLP/FSAwRust-2-Recursion/m1-recursion/src/main.rs `

3. Enter the following code:
``` rust
// Calculate the factorial iteratively.
fn iterative_factorial(n: i64) -> i64 {
    let mut result = 1i64;
    for i in 2i64..=n {
        result *= i;
    }

    return result;
}

// Calculate the factorial recursively.
fn factorial(n: i64) -> i64 {
    let mut iresult = 1i64;
    if n == 0 {
        iresult = 1;
    }
    else {
        iresult = n * factorial(n - 1)
    }
    return iresult
}

// START HERE...
fn main() {
    for n in 0..22 {
        println!("{}! = {}", n, factorial(n));
    }
}
```
4.  Compile and Run the code in `geany` using the *run* button. The following output was produced:
``` text
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5040
8! = 40320
9! = 362880
10! = 3628800
11! = 39916800
12! = 479001600
13! = 6227020800
14! = 87178291200
15! = 1307674368000
16! = 20922789888000
17! = 355687428096000
18! = 6402373705728000
19! = 121645100408832000
20! = 2432902008176640000
thread 'main' panicked at 'attempt to multiply with overflow', src/main.rs:18:19
```
As expected, the program exceeded i64 integer capacity on the 21st iteration...