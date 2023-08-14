# 6. N Queens Redux
This task will reformulate the previous N-Queens solution focusing on the solution space where there can only be one queen in each column. 
## Milestones
### 1. Write `place_queens_4`
The code for this function is shown below:
``` rust
// Try to place a queen in this column.
// Return true if we find a legal board.
fn place_queens_4(board: &mut [[char; NUM_COLS]; NUM_ROWS], c: i32) -> bool {
	// all columns occupied by at least one queen?
	if c == INUM_ROWS { return board_is_legal(board) }
	if c < INUM_ROWS {
		if !board_is_legal(board) { return false }
		for r in 0..INUM_ROWS {
			board[r as usize][c as usize] = 'Q';
			if place_queens_4(board, c+1) { return true }
			board[r as usize][c as usize] = '.'; }
		}
	return false
}
```
This function is simpler than previous versions and uses the `board_is_legal` function directly instead of testing with `board_is_solution` (which has been removed, along with the previous `place_queen...` functions). The `main` function was also modified to call `place_queens_4` correctly. The solution space paring techniques tried in `place_queens_3` have not been implemented here, so those functions were removed. 

The entire Rust code is available at [N_Queens_Redux_main.rs](N_Queens_Redux_main.rs.md)
### 2. Test run
The code was run with several settings for NUM_ROWS.

The `20 ROWS` solution matches that provided in the MLP notes :-) 
``` bash
[sysadmin@centos8s ~]$ cd MLP/FSAwRust-2-Recursion/m6-nqueens/
[sysadmin@centos8s m6-nqueens]$ cargo run --release  (20 ROWS)
   Compiling m6-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m6-nqueens)
    Finished release [optimized] target(s) in 1.73s
     Running `target/release/m6-nqueens`
Time: 2.186821076s
Success!
Q . . . . . . . . . . . . . . . . . . . 
. . . Q . . . . . . . . . . . . . . . . 
. Q . . . . . . . . . . . . . . . . . . 
. . . . Q . . . . . . . . . . . . . . . 
. . Q . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . Q . 
. . . . . . . . . . . . . . . . Q . . . 
. . . . . . . . . . . . . . Q . . . . . 
. . . . . . . . . . . Q . . . . . . . . 
. . . . . . . . . . . . . . . Q . . . . 
. . . . . . . . . . . . . . . . . . . Q 
. . . . . . . Q . . . . . . . . . . . . 
. . . . . Q . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . Q . . 
. . . . . . Q . . . . . . . . . . . . . 
. . . . . . . . . . . . Q . . . . . . . 
. . . . . . . . . . Q . . . . . . . . . 
. . . . . . . . Q . . . . . . . . . . . 
. . . . . . . . . . . . . Q . . . . . . 
. . . . . . . . . Q . . . . . . . . . . 

[sysadmin@centos8s m6-nqueens]$ cargo run --release  (22 ROWS)
   Compiling m6-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m6-nqueens)
    Finished release [optimized] target(s) in 0.93s
     Running `target/release/m6-nqueens`
Time: 26.70036539s
Success!
Q . . . . . . . . . . . . . . . . . . . . . 
. . . Q . . . . . . . . . . . . . . . . . . 
. Q . . . . . . . . . . . . . . . . . . . . 
. . . . Q . . . . . . . . . . . . . . . . . 
. . Q . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . Q . . . . 
. . . . . . . . . . . . . . . . . . . Q . . 
. . . . . . . . . . . . . Q . . . . . . . . 
. . . . . . . . . . . . . . . . Q . . . . . 
. . . . . Q . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . Q . 
. . . . . . . . . . . . . . . Q . . . . . . 
. . . . . . . . . Q . . . . . . . . . . . . 
. . . . . . Q . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . Q 
. . . . . . . . . . . . . . . . . . Q . . . 
. . . . . . . Q . . . . . . . . . . . . . . 
. . . . . . . . . . . . Q . . . . . . . . . 
. . . . . . . . . . Q . . . . . . . . . . . 
. . . . . . . . Q . . . . . . . . . . . . . 
. . . . . . . . . . . . . . Q . . . . . . . 
. . . . . . . . . . . Q . . . . . . . . . . 

[sysadmin@centos8s m6-nqueens]$ cargo run --release  (28 ROWS)
   Compiling m6-nqueens v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m6-nqueens)
    Finished release [optimized] target(s) in 0.81s
     Running `target/release/m6-nqueens`
Time: 87.577726452s
Success!
Q . . . . . . . . . . . . . . . . . . . . . . . . . . . 
. . . Q . . . . . . . . . . . . . . . . . . . . . . . . 
. Q . . . . . . . . . . . . . . . . . . . . . . . . . . 
. . . . Q . . . . . . . . . . . . . . . . . . . . . . . 
. . Q . . . . . . . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . . . . . Q . . 
. . . . . . . . . . . . . . . . . Q . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . Q . . . . . . 
. . . . . Q . . . . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . . Q . . . . . 
. . . . . . Q . . . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . Q . . . . . . . . . 
. . . . . . . Q . . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . . . Q . . . . 
. . . . . . . . Q . . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . Q . . . . . . . . 
. . . . . . . . . Q . . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . . . . . Q . . . . . . . 
. . . . . . . . . . . . . . . . . . . . . . . . . . . Q 
. . . . . . . . . . . . . . . . . . . . . . . . Q . . . 
. . . . . . . . . . . . . . . . . . . . . . . . . . Q . 
. . . . . . . . . . . . Q . . . . . . . . . . . . . . . 
. . . . . . . . . . Q . . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . Q . . . . . . . . . . . . 
. . . . . . . . . . . Q . . . . . . . . . . . . . . . . 
. . . . . . . . . . . . . . Q . . . . . . . . . . . . . 
. . . . . . . . . . . . . . . . Q . . . . . . . . . . . 
. . . . . . . . . . . . . Q . . . . . . . . . . . . . . 
```

### (end)
