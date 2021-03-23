---
title: Stdout? More like Stdout of Order! (Rust)
date: 2020-09-01
---

use std::{time, thread};

    fn main() {
        let sleep_time: u64 = 4000;
        let sleep_duration = time::Duration::from_millis(sleep_time);

        print!(“pro”);
        thread::sleep(sleep_duration);

        print!(“gram”);
        thread::sleep(sleep_duration);

        println!(“ming”);
    }

### Why is Rust doing things out of order?

When running this Rust program, it seems to be doing things out of order — not line-by-line. Is the computer acting up? Is Rust doing things concurrently without my permission? Let’s find out!

After a quick glance at the little program above, you’d think it would print the string “pro,” then wait a couple seconds, then print the string “gram,” then wait a couple seconds, then print the string “ming,” then end.

Instead, when the program runs, there is a couple seconds delay, and then “programming” is printed to the console all at once. What’s up with that?!

### Here’s what’s happening behind the scenes.

The main function runs. The *sleep_time* and *sleep_duration* variables are given values. Then, we have our first print statement,

    print!(“pro”);

The string, “pro,” is passed to the *print!* macro. Then the *print!* macro writes the string to the stdout buffer. *It does not write to the console*, just a buffer. So basically the string “pro” is sitting in memory, not the console.

This happens because stdout is line-buffered. Line-buffered means it doesn’t write to the console until it is either given a new line character or explicitly told to write to the console.

So, where were we… The “pro” string was written, so the *print!* macro’s job is finished, but “pro” is not showing on the console yet.

The next statement,

    thread::sleep(sleep_duration);

tells the processor to just hang out and do nothing for 4000 milliseconds. The processor does what it is told.

After that delay, the next *print!* macro prints “gram” to the stdout buffer. Once again, it’s just sitting in memory, hiding from the user. And once again, the processor sees that the *print!* macro did its job (wrote the string to the stdout buffer), so it moves onto the next statement.

After another short sleep, the processor arrives at a slightly different macro, *println!*. *Println!* writes a given string followed by a newline character. That newline character causes the stdout buffer to flush. That flush shoots all of the strings to the console at once, in the correct order.
> Side note: in this particular program, we don’t really need *println!* to get stdout to write to the console. Stdout will flush everything to the console at the end of the program anyway. If *println!* was in the middle, anything written to the stdout buffer with the *print!* macro would flush to the console when that newline character came through.

To get this program to work as expected, we need to explicitly tell stdout to flush its buffer. We can do that with

    io::stdout().flush().unwrap();

Here’s the full program written so it will work as expected.

    use std::{time, thread};

    fn main() {
        let sleep_time: u64 = 4000;
        let sleep_duration = time::Duration::from_millis(sleep_time);

        print!(“pro”);
        io::stdout().flush().unwrap();
        thread::sleep(sleep_duration);

        print!(“gram”);
        io::stdout().flush().unwrap();
        thread::sleep(sleep_duration);

        println!(“ming”);
    }

When we run the program now, there are pauses after each time a string is printed to the console. Yay, it’s doing things the right way now!

So, when it comes down to it, as it always does, the computer was not misbehaving, and Rust was doing exactly what it was supposed to. We just needed to understand how *stdout*, *print!*, and *println!* work.

[Interestingly, many programmers have requested that the print! macro include a stdout flush so that it is more intuitive. Unfortunately, it doesn’t look like that is likely to happen.](https://github.com/rust-lang/rust/issues/23818)

### Resources

* [https://stackoverflow.com/questions/34993744/why-does-this-read-input-before-printing](https://stackoverflow.com/questions/34993744/why-does-this-read-input-before-printing)

* [https://doc.rust-lang.org/beta/std/macro.print.html](https://doc.rust-lang.org/beta/std/macro.print.html)

* [https://github.com/rust-lang/rust/issues/23818](https://github.com/rust-lang/rust/issues/23818)

* [https://doc.rust-lang.org/std/macro.println.html](https://doc.rust-lang.org/std/macro.println.html)

* [https://doc.rust-lang.org/std/io/trait.Write.html](https://doc.rust-lang.org/std/io/trait.Write.html)

* [https://docs.rs/futures/0.1.7/futures/sink/trait.Sink.html](https://docs.rs/futures/0.1.7/futures/sink/trait.Sink.html)
