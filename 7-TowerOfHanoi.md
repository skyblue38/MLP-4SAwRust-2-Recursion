# 7. Tower of Hanoi
The Tower of Hanoi is a puzzle consisting of three rods and a number of disks of various diameters, which can slide onto any rod. The puzzle begins with the disks stacked on one rod in order of decreasing size, the smallest at the top. The aim is to move the entire stack to the last rod, obeying the following rules:

1. Only one disk may be moved at a time.
2. Each move consists of taking the upper disk from one of the stacks and placing it on top of another stack or on an empty rod.
3. No disk may be placed on top of a disk that is smaller than it.

With 3 disks, the puzzle can be solved in 7 moves. The minimal number of moves required to solve a Tower of Hanoi puzzle is 2^n − 1, where n is the number of disks.

![Tower of Hanoi](https://upload.wikimedia.org/wikipedia/commons/6/60/Tower_of_Hanoi_4.gif)__Credit: https://commons.wikimedia.org/wiki/User:Aka__

## Workflow
### 1. Create project and `main` function
The project was created using command:
``` bash
[sysadmin@centos8s ~]$ cargo new MLP/FSAwRust-2-Recursion/m7-tower
[sysadmin@centos8s ~]$ cd MLP/FSAwRust-2-Recursion/m7-tower/src
```
The `main` function was populated as shown below:
``` rust
// Tower of Hanoi puzzle by Chris Freeman (14AUG23)

const NUM_DISKS: usize = 3;

fn main() {
    // Make three posts with NUM_DISKS entries, all set to 0.
    let mut posts = [[0; NUM_DISKS]; 3];

    // Put the disks on the first post in order, smallest first (on top).
    for i in 0..NUM_DISKS {
        posts[0][i] = i + 1;
    }

    // Draw the initial setup.
    draw_posts(&mut posts);

    // Move the disks.
    move_disks(&mut posts, NUM_DISKS, 0, 1, 2);
    println!("Ok");
}

```
### 2. Write `draw_posts` function
Here is the `draw_posts` function:
``` rust
// Draw the posts by showing the size of the disk at each level.
fn draw_posts(posts: &mut [[usize; NUM_DISKS]; 3]) {
	for i in 0..NUM_DISKS {
		print!("{} ", posts[0][i]);
		print!("{} ", posts[1][i]);
		println!("{}", posts[2][i])
	}
	println!("-----")
}
```
### 3. Write `move_disk` function
The rust code to move a single disk between posts is shown below:
``` rust
// Move one disk from from_post to to_post.
fn move_disk(posts: &mut [[usize; NUM_DISKS]; 3], from_post: usize, to_post: usize) {
    // Find the first non-empty row in from_post.
	let mut f_row = 0usize;
	for i in 0..NUM_DISKS {
		if posts[from_post][i] != 0 { f_row = i; break }
	}
    // Find the first non-empty row in to_post.
	let mut t_row = 0usize;
	for j in 0..NUM_DISKS {
		if posts[to_post][j] != 0 { t_row = j; break }
	}
    // Swap the values at those positions.
    (posts[from_post][f_row], posts[to_post][t_row]) =
        (posts[to_post][t_row], posts[from_post][f_row]);	
}
```
### 4. Write `move_disks` function
Here's the recursive ode for moving stacks of disks:
``` rust
// Move the disks from from_post to to_post
// using temp_post as temporary storage.
fn move_disks(posts: &mut [[usize; NUM_DISKS]; 3],
	    num_to_move: usize, from_post: usize, to_post: usize, temp_post: usize) {
    if num_to_move > 1 {
		move_disks(posts, num_to_move - 1, from_port, temp_post, to_post)
	}
	move_disk(posts, from_post, to_post)
	if num_to_move > 1 {
		move_disks(posts, num_to_move - 1, temp_post, to_post, from_post)
	}
}
```
### 5. Test Run
The combined program was run with a stack of 3 disks, and the results are shown below. A complete copy of the Rust program is available [HERE](TowerOfHanoi_main.rs.md):
``` bash
[sysadmin@centos8s ~]$ cd MLP/FSAwRust-2-Recursion/m7-tower/
[sysadmin@centos8s m7-tower]$ cargo run --release
   Compiling m7-tower v0.1.0 (/home/sysadmin/MLP/FSAwRust-2-Recursion/m7-tower)
    Finished release [optimized] target(s) in 0.53s
     Running `target/release/m7-tower`
1 0 0
2 0 0
3 0 0
-----
0 0 0
2 0 0
3 1 0
-----
0 0 0
0 0 0
3 1 2
-----
0 0 0
0 0 1
3 0 2
-----
0 0 0
0 0 1
0 3 2
-----
0 0 0
0 0 0
1 3 2
-----
0 0 0
0 2 0
1 3 0
-----
0 1 0
0 2 0
0 3 0
-----
Ok
```

After changing `NUM_DISKS` to 16, this was run again and a partial output is shown below. The number of disk exchanges required in this case was 65535, which concurs with the expected number of 2^16 -1:
``` bash
sysadmin@centos8s m7-tower]$ cargo run --release
1 0 0
2 0 0
3 0 0
4 0 0
5 0 0
6 0 0
7 0 0
8 0 0
9 0 0
10 0 0
11 0 0
12 0 0
13 0 0
14 0 0
15 0 0
16 0 0
-----
... (omitted)
-----
0 1 0
0 2 0
0 3 0
0 4 0
0 5 0
0 6 0
0 7 0
0 8 0
0 9 0
0 10 0
0 11 0
0 12 0
0 13 0
0 14 0
0 15 0
0 16 0
-----
Ok
[sysadmin@centos8s m7-tower]$ ./target/release/m7-tower|grep "\-\-\-\-\-"|wc -l
65536
```
## References
* [Wikipedia - Recursive solution](https://en.wikipedia.org/wiki/Tower_of_Hanoi#Recursive_solution)
* 

### (end)
