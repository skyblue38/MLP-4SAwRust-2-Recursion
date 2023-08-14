# Tower of Hanoi - Rust code
``` rust
// Tower of Hanoi puzzle by Chris Freeman (14AUG23)

const NUM_DISKS: usize = 3;

// Draw the posts by showing the size of the disk at each level.
fn draw_posts(posts: &mut [[usize; NUM_DISKS]; 3]) {
	for i in 0..NUM_DISKS {
		print!("{} ", posts[0][i]);
		print!("{} ", posts[1][i]);
		println!("{}", posts[2][i])
	}
	println!("-----")
}

// Move one disk from from_post to to_post.
fn move_disk(posts: &mut [[usize; NUM_DISKS]; 3], from_post: usize, to_post: usize) {
    // Find the first non-empty row in from_post.
	let mut f_row = 0usize;
	for i in 0..NUM_DISKS {
		if posts[from_post][i] != 0 { f_row = i; break }
	}
    // Find the last empty row in to_post.
	let mut t_row = NUM_DISKS - 1;
	for j in 0..NUM_DISKS {
		if posts[to_post][j] != 0 { t_row = j - 1; break }
	}
    // Swap the values at those positions.
    // println!("from:{},{} to:{},{}", from_post, f_row, to_post, t_row);
    (posts[from_post][f_row], posts[to_post][t_row]) =
        (posts[to_post][t_row], posts[from_post][f_row]);
	// draw the new post layouts
	draw_posts(posts);
}

// Move the disks from from_post to to_post
// using temp_post as temporary storage.
fn move_disks(posts: &mut [[usize; NUM_DISKS]; 3], num_to_move: usize, 
		from_post: usize, to_post: usize, temp_post: usize) {
    if num_to_move > 1 {
		move_disks(posts, num_to_move - 1, from_post, temp_post, to_post)
	}
	move_disk(posts, from_post, to_post);
	if num_to_move > 1 {
		move_disks(posts, num_to_move - 1, temp_post, to_post, from_post)
	}
}

// START HERE
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