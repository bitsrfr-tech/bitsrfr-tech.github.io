---
title: See a Value’s Type in Rust — Quick and Dirty
date: 2020-08-01
---

When working with a strongly typed language, like Rust, you are bound to run into compiler errors that may or may not be type-related. The Rust compiler is usually good about telling the user what type it found and what type it expected, but there are times when it doesn’t.

### Here’s a quick and dirty hack for those times when you need to see the data type of a value in a Rust program.

1. Declare a unit/nil type variable — that’s just parentheses. [If you’re interested, here’s some documentation on Rust’s unit type.](https://doc.rust-lang.org/std/primitive.unit.html)

1. Assign the variable whose type you want to check to the unit variable.

1. Run compile the program — *rustc* or *cargo run*.

**The compiler will throw an error that indicates the variable’s type.**

### Here’s an example

    fn main() {

        let my_var: u32 = 122500;

        let () = my_var;

    }

### Running that program in a terminal looks like this

![](/images/SeeValuesTypeInRust_Img_1.png)

**The red text that says “^^ expected…” is the key.**

You can translate Rust telling you what type it expected as Rust telling you the type of the variable you assigned to *()*.

That really is quick and dirty, eh? It’s the easiest way I have found to see a value’s type in Rust. I hope it helps you as much as it has helped me!

Do you know of or use any other methods for finding the type of a variable in Rust? Let us know!
