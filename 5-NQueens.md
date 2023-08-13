# 5. N Queens
?
## Workflow
### 1. Create project and prepare code
Project was created and edited using the commands:
``` bash
[sysadmin@centos8s ~]$ cargo new MLP/FSAwRust-2-Recursion/m5-nqueens/
[sysadmin@centos8s ~]$ geany MLP/FSAwRust-2-Recursion/m5-nqueens/src/main.rs
```
Th code shown below also defines the main program that will exercise the program, including the `board` variable. This is a two-dimensional Rust array (vector of vectors) holding characters that will be a period (`'.'`) if the corresponding square is empty and a `'Q'` if the square is occupied by a Queen.
``` rust
// N-Queens solution by Chris Freeman (13AUG23)
use std::time::{Instant};
// The board dimensions.
const NUM_ROWS: usize = 4;
const NUM_COLS: usize = NUM_ROWS;
const INUM_ROWS: i32 = NUM_ROWS as i32;
const INUM_COLS: i32 = NUM_COLS as i32;
fn main() {
    // Create a NUM_ROWS x NUM_COLS array with all entries Initialized to UNVISITED.
    let mut board = [['.'; NUM_COLS]; NUM_ROWS];
    let start = Instant::now();
    let success = place_queens_1(&mut board, 0, 0);
    //let success = place_queens_2(& mut board, 0, 0, 0);
    //let success = place_queens_3(& mut board);
    let duration = start.elapsed();
    println!("Time: {:?}", duration);
    if success {
        println!("Success!");
    } else {
        println!("Could not find a solution.");
    }
    dump_board(&mut board);
}
```
### 2. Create a *dump_board* function
The following code was entered to provide a way to display the chess board:
``` rust
// Display the board.
fn dump_board(board: &mut [[char; NUM_COLS]; NUM_ROWS]) {
    for r in 0..NUM_ROWS {
        for c in 0..NUM_COLS {
            print!("{:<02}", board[r][c]);
        }
        println!();
    }
    println!();
}
```
### 3. Create *series _is_legal* function
Create a `series_is_legal` method that returns `true` if a series of adjacent squares contains at most one queen.

The function should start at square `[r0][c0]` and enter a loop. Each time through the loop it should add `dr` to the row and `dc` to the column. It should repeat the loop until the row or column falls off the board, counting the number of queens as it goes. If the function finds two or more queens, then it should return `false`.
``` rust
// Return true if this series of squares contains at most one queen.
fn series_is_legal(board: &mut [[char; NUM_COLS]; NUM_ROWS],
    r0: i32, c0: i32, dr: i32, dc: i32) -> bool {
// Return true if this series of squares contains at most one queen.
    let mut has_queen = false;
    let mut r = r0;
    let mut c = c0;
    loop {
        if board[r as usize][c as usize] == 'Q' {
            // If we already have a queen on this row,
            // then this board is not legal.
            if has_queen { return false }
            // Remember that we have a queen on this row.
            has_queen = true;
        }
        // Move to the next square in the series.
        r += dr;
        c += dc;
        // If we fall off the board, then the series is legal.
        if  r >= INUM_ROWS ||
            c >= INUM_COLS ||
            r < 0 ||
            c < 0 {
                return true;
        }
    }
}
```
### 4. Create *board_is_legal* function
This function should call `series_is_legal` for each row, column, and diagonal. If any of those return `false`, then this function should also return false.
``` rust
// Return true if the board is legal.
fn board_is_legal(board: &mut [[char; NUM_COLS]; NUM_ROWS]) -> bool {
// See if each row is legal.
    for r in 0..INUM_ROWS {
        if !series_is_legal(board, r, 0, 0, 1) { return false; }
    }
// See if each column is legal.
    for c in 0..INUM_COLS {
        if !series_is_legal(board, 0, c, 1, 0) { return false; }
    }
// See if diagonals down to the right are legal.
    for r in 0..INUM_ROWS {
        if !series_is_legal(board, r, 0, 1, 1) { return false; }
    }
    for c in 0..INUM_COLS {
        if !series_is_legal(board, 0, c, 1, 1) { return false; }
    }
// See if diagonals down to the left are legal.
    for r in 0..INUM_ROWS {
        if !series_is_legal(board, r, INUM_ROWS - 1, 1, -1) { return false; }
    }
    for c in 0..INUM_COLS {
        if !series_is_legal(board, 0, c, 1, -1) { return false; }
    }
// If we survived this long, then the board is legal.
    return true;
}
```
### 5. Write *board_is_a_solution* function
This function returns `true` if the board is legal and contains `num_rows` queens. For example, if an 8 × 8 board contains only six queens, then it might be legal but it’s not a solution.

Use the `board_is_legal` function to tell if the board is legal and then loop through the board counting queens to see if it is a solution.

Here is the function code:
```
// Return true if the board is legal and a solution.
fn board_is_a_solution(board: &mut [[char; NUM_COLS]; NUM_ROWS]) -> bool {
    // See if it is legal.
    if !board_is_legal(board) { return false; }

    // See if the board contains exactly NUM_ROWS queens.
    let mut num_queens = 0;
    for r in 0..NUM_ROWS {
        for c in 0..NUM_COLS {
            if board[r as usize][c as usize] == 'Q' { num_queens += 1; }
        }
    }
    return num_queens == NUM_ROWS;
}
```
### 6. Write *place_queens_1* function
This function accepts a `board` parameter which is the current board arrangement showing where any previously positioned queens are waiting to ambush new queens!

The `r` and `c` values give the position that the function should examine. It will see what happens if we put a queen here or not. This is achieved by recursively examine every square. After there is  an assignment for each square, check to see whether this is a solution.

The Rust code for this functions is shown below:
``` rust
// Try placing a queen at position [r][c].
// Return true if we find a legal board.
fn place_queens_1(board: &mut [[char; NUM_COLS]; NUM_ROWS],	r: i32, c: i32) -> bool {
	if r >= INUM_ROWS {
		return board_is_a_solution(board) }
	else {
		let mut next_r = r;
		let mut next_c = c + 1;
		if next_c >= INUM_COLS {
			next_r = next_r + 1;
			next_c = 0 }
		if place_queens_1(board, next_r, next_c) {
			return true }
		if next_r >= INUM_ROWS { return false }
		board[next_r as usize][next_c as usize] = 'Q';
		if place_queens_1(board, next_r, next_c) {
			return true }
		board[next_r as usize][next_c as usize] = '.';
		return false
	}
}
```
### 7. Test run
With NUM_ROWS set to 4, running the code resulted in:
``` bash
[sysadmin@centos8s m5-nqueens]$ cargo run --release 
   Compiling m5-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m5-nqueens)
    Finished release [optimized] target(s) in 0.56s
     Running `target/release/m5-nqueens`
Time: 364.249µs
Success!
. . Q . 
Q . . . 
. . . Q 
. Q . . 
```
With NUM_ROWS set to 5, the result was:
``` bash
[sysadmin@centos8s m5-nqueens]$ cargo run --release 
   Compiling m5-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m5-nqueens)
    Finished release [optimized] target(s) in 0.66s
     Running `target/release/m5-nqueens`
Time: 33.197636ms
Success!
. . . . Q 
. . Q . . 
Q . . . . 
. . . Q . 
. Q . . . 
```
With NUM_ROWS set to 6, the result was:
``` bash
[sysadmin@centos8s m5-nqueens]$ cargo run --release 
   Compiling m5-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m5-nqueens)
    Finished release [optimized] target(s) in 0.79s
     Running `target/release/m5-nqueens`
Time: 58.678796888s
Success!
. . . . Q . 
. . Q . . . 
Q . . . . . 
. . . . . Q 
. . . Q . . 
. Q . . . . 
```
With NUM_ROWS set to 7, the result was:
``` bash
?
```

## References
* _Wikipedia_ [Eight Queens puzzle - Algorithm design](https://en.wikipedia.org/wiki/Eight_queens_puzzle#Exercise_in_algorithm_design)
* _Wikipedia_ [Depth First Search -DFS](https://en.wikipedia.org/wiki/Depth-first_search)
* [Solving the N queens problem in Rust](https://theodz.github.io/tutorial/rust/2016/11/22/rust-n-queens.html)
* [N-Queens problem - RosettaCode](https://rosettacode.org/wiki/N-queens_problem#Rust)

### End
