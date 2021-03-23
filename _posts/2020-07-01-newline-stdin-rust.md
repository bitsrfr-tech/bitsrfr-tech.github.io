---
title: There’s a Newline in Stdin, Watch Out! (Rust)
date: 2020-07-01
---

On my journey through the brain gymnasium that is the Rust programming language, newlines have made for some intense brain workouts. You can read about my first experience with the newline character playing tricks on me here. The issue in that post was caused by stdout flushing when it sees a newline. This post has to do with stdin’s newline character messing up a type conversion.

## The Broken Code

    use std::io::stdin;

    fn main() {

        let mut s = String::new();

        println!(“Give a number “);

        stdin().read_line(&mut s)
            .expect(“Did not enter a correct string”);

        let user_input: f64 = s.parse().unwrap();

        println!(“{:?}”, user_input)

    }

That code looks legit, right? Rust’s compiler compiles it without any issues. But when run, it panics immediately after the user enters a number.

### The Panic

    thread ‘main’ panicked at ‘called Result::unwrap() on an Err value: ParseFloatError { kind: Invalid }’, src/libcore/result.rs:999:5 note: run with RUST_BACKTRACE=1 environment variable to display a backtrace.

### The Troubleshooting

At first, I thought I wasn’t inputting my number correctly, but I definitely used a floating-point number, 39.99.

I also thought maybe the issue was caused by stdin’s input type not being a string, but I confirmed it was a string with [this quick little test](/_posts/see-values-type-rust).

Then I thought maybe the parse was returning a [result](https://doc.rust-lang.org/std/result/) instead of just returning the converted value, so I tried handling that in my code. Nothing was fixing the problem!

Finally, I assigned “39.99” to my “s” variable directly in the code, rather than getting it from user input, to see if that would convert. It did! The type conversion worked fine without stdin, so the stdin input was causing the error.

What was stdin doing that was messing up my string? It turns out the answer is in the Rust docs. It was just *really* hard to find. In fact, I didn’t find it until *after* [some smart folks on StackOverflow helped me find the issue](https://stackoverflow.com/questions/58567077/cant-parse-string-from-stdin-to-floating-point-rust) — a big thanks to them!

Here’s what the [Rust docs say about the read_line function](https://doc.rust-lang.org/std/io/trait.BufRead.html#method.read_line):
> Read all bytes until a newline (the 0xA byte) is reached, and append them to the provided buffer.
> This function will read bytes from the underlying stream until the newline delimiter (the 0xA byte) or EOF is found. Once found, all bytes up to, and including, the delimiter (if found) will be appended to buf.

### The Problem

“Read_line reads all bytes,” so it reads each character the user types,

“…until a newline.” Okay, that’s fine. No issues there.

“…This function will read bytes from the underlying stream,” yada yada. Great, makes sense.

“…Once found, all bytes up to, and including,” aha!

There it is: **“and including.”**

My program could convert a string of “39.99” to a floating-point just fine. **The problem was that it was actually trying to convert “39.99” along with an invisible newline character into a floating-point.** That won’t work because a newline character is not a number.

We can demonstrate the existence of the invisible newline character with the following two programs. The first program prints the user input before it is trimmed, and the second prints the user input after it is trimmed (trimming removes the newline character).

### Program that Does Not Trim the User Input

    use std::io::stdin;

    fn main() {

        let mut s = String::new();

        println!(“Give a number “);

        stdin().read_line(&mut s)
            .expect(“Did not enter a correct string”);

        println!(“{}”, s);

    }

### Output Before Trimming User Input

![There’s a blank line after the second “50.”](/images/NewlineInStdinRust_Img_1.png)*There’s a blank line after the second “50.”*

### Program that Trims User Input

    use std::io::stdin;

    fn main() {

        let mut s = String::new();

        println!(“Give a number “);

        stdin().read_line(&mut s)
            .expect(“Did not enter a correct string”);

        println!(“{}”, s.trim());

    }

### Output After Trimming User Input

![There is no blank line after the second “50.”](/images/NewlineInStdinRust_Img_2.png)*There is no blank line after the second “50.”*

See the added space in the first program? That bugger stumped me for a good while. But as with so many programming problems, it was easy to fix once I identified it. I just added a “trim()” when assigning my “s” string to the user_input variable. That removed the newline character and fixed the bug.

## The Fixed Code

    use std::io::stdin;

    fn main() {

        let mut s = String::new();

        println!(“Give a number “);

        stdin().read_line(&mut s)
            .expect(“Did not enter a correct string”);

        let user_input: f64 = s.trim().parse().unwrap();

        println!(“{:?}”, user_input)

    }

I must say, it feels great to finally solve issues like this one. It is almost worth the agony of trying to figure it out. Ultimately, I can’t complain. I learned a valuable lesson about stdin, read_line, and type conversion — watch out for newline characters.

However, the most valuable lesson I learned from both of my newline issues wasn’t to watch out for newlines. Though I will definitely do that. The most valuable lesson I learned is to make sure to thoroughly read every bit of Rust documentation I can find on each function I use.

That points to a broader truth when it comes to technology (and life in general). When we understand how something works, we are more effective at solving problems with it and better at fixing it when it breaks.

Resources:

* [https://stackoverflow.com/questions/58567077/cant-parse-string-from-stdin-to-floating-point-rust](https://stackoverflow.com/questions/58567077/cant-parse-string-from-stdin-to-floating-point-rust)

* [https://doc.rust-lang.org/std/io/trait.BufRead.html#method.read_line](https://doc.rust-lang.org/std/io/trait.BufRead.html#method.read_line)

* [https://doc.rust-lang.org/std/result/](https://doc.rust-lang.org/std/result/)

* [https://medium.com/@JoeKreydt/stdout-more-like-stdout-of-order-rust-1f9acc016e89](https://medium.com/@JoeKreydt/stdout-more-like-stdout-of-order-rust-1f9acc016e89)

* [https://medium.com/@JoeKreydt/see-a-values-type-in-rust-quick-and-dirty-218ca808c19d](https://medium.com/@JoeKreydt/see-a-values-type-in-rust-quick-and-dirty-218ca808c19d)
