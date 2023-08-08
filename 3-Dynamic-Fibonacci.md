# 3. Dynamic Fibonacci Numbers
In this milestone we develop methods that calculate Fibonacci numbers using **dynamic programming** or **memoization**. The **dynamic** concept relies on caching partial solutions for later reuse. This should significantly reduce the effort required to calculate large Fibonacci values. A rust **vector array** will be used as the memoization store. Method that will be explored include:
1. *Fill in on the fly* - Check if the requested fibonacci number (N) is in the cache and if so, use that; Otherwise recursively calculate the fibonacci of N-1 and N-2 to form the requested number (N-1 + N-2) and it to the cache.
2. *Prefilled* - initially calculates every fibonacci number (up to 90) and saves them in a *vec* array. This can be don in two ways: *Top-down*, where the requested number and all lesser values are recursively calculated, and *Bottom-up*, where sequentials values are calulated up to the requested value... 
## Workflow
### 1. Research *Dynamic Programming*
Read [Dynamic Programming/Computer Science](https://en.wikipedia.org/wiki/Dynamic_programming#Computer_science)
### 2. Write **fibonacci()** function
The *Fill on the Fly* method was tried first: Here is the rust code developed...
``` rust
// Dynamic Fibonacci Numbers
// Manning Learning Project https://liveproject.manning.com/project/1553
// by Chris Freeman - 2023-08-08
use std::io;
use std::io::Write;
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
// Given the target sequence and two preceeding fibonacci values, return the next fibonacci number
fn next_fibonacci(n: i64, n_1: i64, n_2: i64) -> i64 {
	if n < 3 {
		return n_1 + n_2
	}
	return next_fibonacci(n - 1, n_1 + n_2, n_1)
}
// calculate fibonacci number without cache lookup
fn fibonacci(n: i64) -> i64 {
	if n < 1 { return 0 }
	else if n == 1 {return 1 };
	return next_fibonacci(n, 1, 0)
}
// calculate fibonacci using cache populated with *on the fly* method
fn fibonacci_on_the_fly(values: &mut Vec<i64>, n: i64) -> i64 {
	if n < 1 { return 0 }
	else if n == 1 { return 1 };
		if values[n as usize] == 0 {      // no cached value
		values[n as usize] = fibonacci(n) // go get it and update cache
	}
	return values[n as usize]
}
// START HERE
fn main() {
	// initialise the fibonacci cache
	let mut fill_on_the_fly_values: Vec<i64> = vec![0; 92];	// zero fill
	// get a number
    println!("Enter -1 to exit\n");
    loop {
        // Prompt the user for n.
        let n = get_i64("N: ");
        // If n < 0, break out of the loop.
        if n < 0 {
            break;
        }
        // Calculate the Fibonacci number.
        println!("fibonacci({}) = {}\n", n, fibonacci_on_the_fly(&mut fill_on_the_fly_values, n));
    }
}

```
### 3. Test **fibonacci()** function
Using the **rust** code shown above, the program was compiled and run and the following output was produced...
``` bash
[sysadmin@centos8s m3-dynamic]$ cargo run
   Compiling m3-dynamic v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m3-dynamic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.71s
     Running `target/debug/m3-dynamic`
Enter -1 to exit

N: 10
fibonacci(10) = 55

N: 20
fibonacci(20) = 6765

N: 30
fibonacci(30) = 832040

N: 91
fibonacci(91) = 4660046610375530309

N: -1
[sysadmin@centos8s m3-dynamic]$ 
```


#### (end)
