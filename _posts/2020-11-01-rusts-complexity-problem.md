---
title: Rust’s Complexity Problem — A Warning
date: 2020-11-01
---

> “Every application has an inherent amount of irreducible complexity. The only question is: Who will have to deal with it — the user, the application developer, or the platform developer?”[[1]](http://nomodes.com/Larry_Tesler_Consulting/Complexity_Law.html)
 — Larry Tesler

There has been a trend circulating the web in the past couple years called “Rewrite it in Rust” (RiiR). Many big companies are rewriting core parts of their applications in Rust — Discord [[2]](https://blog.discordapp.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f), Microsoft [[3]](https://msrc-blog.microsoft.com/2019/07/22/why-rust-for-safe-systems-programming), and Cloudflare [[4]](https://blog.cloudflare.com/tag/rust/), are just a few. The tech avante-garde’s positive reports on Rust’s resource efficiency and memory safety sound great, but those attributes come at a price, and they are far from being the only measures of software quality [[5]](https://www.tutorialspoint.com/software_quality_management/software_quality_management_metrics.htm). **Especially in applications that don’t require maximum resource efficiency and memory safety, adopting Rust in 2020 is irrational at best.**

I will not suggest programmers avoid Rust on account of its immaturity [[6]](https://blog.debiania.in.ua/posts/2020-03-01-rust-dependencies-scare.html), its often-false sense of security [[7]](https://medium.com/@shnatsel/smoke-testing-rust-http-clients-b8f2ee5db4e6), or its lack of true freedom [[8]](https://internals.rust-lang.org/t/rust-s-freedom-flaws/11533). However, I do suggest programmers avoid Rust because it makes the programmer manage complexity that can, in most cases, be sufficiently managed by the machine.

### The Problem of Complexity in Computing

A major purpose of any high level programming language is to manage the complexity of controlling a computer. Communicating a command directly to a computer requires machine code — a whole bunch of 1s and 0s. That gets complicated. Assembly language hides some of that complexity by converting English-like commands into the 1s and 0s of machine code. Higher level languages, like C, hide that complexity even further. The complexity is still there, but languages like Assembly and C manage the complexity so that the programmer doesn’t have to think about it. These well-established tools are much better at handling complexity than our human brains.

Computers are complex. Most of that complexity is essential to their existence, so it will always be there. If we are to use computers, their complexity must be dealt with. **When not managed properly, complexity leads to errors** [[9]](https://www.microsoft.com/en-us/research/publication/the-influence-of-organizational-structure-on-software-quality-an-empirical-case-study/).

There are a few ways a computer’s complexity can be managed. The tools used to manipulate computers can manage it (e.g. a high-level language translates English-like commands to machine code); the programmer can manage it (e.g. the programmer writes machine code directly); or the end-user can manage it (e.g. the user writes machine code directly). There can also be various combinations of these three.

### Who should manage complexity?

In all software projects, the end-user’s experience should be the main concern. That could mean that an astronaut’s space ship lands safely on the moon, or it could mean that an office administrator’s calendar application never loses data. The programmer’s goal is to give the end-user the tools necessary to accomplish a task. A part of that is making sure the end-user doesn’t have to think about the computer’s complexity. It is expected that the end-user has the least amount of understanding of a computer’s complexity. That means he/she is the most likely to cause errors when managing that complexity. Most programmers aren’t fond of fixing errors caused by end-users, and most end-users aren’t fond of managing a computer’s complexity.

That leaves the programmer and the tools to manage the complexity. Our tools are imperfect, so they are incapable of managing all of the complexity. However, some tools manage complexity better than others, leaving the programmer to manage varying degrees of complexity. The less complexity a human has to manage, the less room for error.

### Where does this leave Rust?

The brilliant thing about Rust, and the primary reason for its low resource usage, is that it enforces memory safety without the use of a garbage collector (a program that frees computer memory when it is no longer needed). Most programming languages either leave memory management up to the programmer or they use a garbage collector. The languages that leave memory management up to the programmer are less memory safe, but also less resource intensive. These include C, C++, and Assembly [[10]](https://wiki.c2.com/?LanguagesWithoutGarbageCollection). The languages that use a garbage collector are safer from a memory standpoint, but use more computing resources when the garbage collector is running. These include C#, JavaScript, and Python [[11]](https://www.memorymanagement.org/mmref/lang.html).

Rust neither uses a garbage collector nor leaves memory management up to the programmer. Instead, it uses its compiler to make the programmer manage the program’s memory in a specific way. Before a Rust program can be compiled, it is subjected to the scrutiny of the borrow checker which reviews the program for improper handling of memory [[12]](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html). A programmer can make the compiler allow an unsafe program, but that is not the [default [13]](https://kk.org/thetechnium/triumph-of-the/).

Rust’s way of managing memory is unique and clever. Unique and clever are generally seen as good things, but in software development they usually lead to difficulties with long-term maintenance, among other issues [[14]](https://softwareengineering.stackexchange.com/questions/25276/why-is-cleverness-considered-harmful-in-programming-by-some-people)[[15]](https://news.ycombinator.com/item?id=20386073)[.](https://softwareengineering.stackexchange.com/questions/25276/why-is-cleverness-considered-harmful-in-programming-by-some-people)(https://news.ycombinator.com/item?id=20386073).)

Rust does not manage the complexity for the programmer like garbage-collected languages, and it does not put the complexity solely in the hands of the programmer like most non-garbage-collected languages. Instead, the Rust compiler guides the programmer in managing a single aspect of the complexity, memory (which is responsible for a large portion of security vulnerabilities [[16]](https://msrc-blog.microsoft.com/2019/07/16/a-proactive-approach-to-more-secure-code/)), in a specific way. That works fine for memory management, but **in order to manage memory the way the borrow checker wants, the programmer is at best tempted, and at worst forced, to add complexity to the program.** That is the crux of the problem.

[Edited 17 March 2020] It is very difficult to give clear cut examples of Rust encouraging the programmer to add complexity to the program because a program written in language X by programmer X will look very different than a program written in language Y by programmer Y even if the two programs do the exact same thing. This means we can’t simply compare a program written in Rust with a program written in another language and see exactly where complexity was added. However, I received criticism for not providing an example here, and I think that is valid criticism, so I will offer one.

The example is discussed here [[18]](https://www.reddit.com/r/Amethyst/comments/emjqai/what_exactly_is_gamedata/). Basically, Rust’s borrow checker does not allow for circular mutable references, so the game engine programmers added a dispatcher to get around this. Maybe there is a better way around it, but the bottom line is that the borrow checker’s limitations encouraged the programmers to add another part (more complexity). In turn, the programmer using the game engine must comprehend and deal with this added complexity.

One might ask, “shouldn’t circular mutable references be avoided anyway?” In many cases, they should be avoided. However, there are a number of situations in which circular mutable references are not harmful and may be the best solution to a problem [[19]](https://stackoverflow.com/questions/1897537/why-are-circular-references-considered-harmful). With Rust, when a problem arises and the best solution is the use of a circular mutable reference, the programmer must either defy the borrow checker or choose a more complex solution.

The compiler helps the programmer handle the complexity of memory management, but it does so by forcing the programmer to write the program in a specific way. The Rust way limits the program and the programmer. In order to create a working program under Rust’s limitations, the developer often adds workarounds or extra moving parts. These are added so that memory can be managed safely, but at what cost? What other bugs or flaws arise from the new complexity that the programmer must handle? Sure, the added complexity probably won’t result in memory management problems, but it will result in other problems because humans are not good at managing complexity.

The brilliant thing about Rust is also the most dangerous. Many software developers take great pleasure in learning about new and interesting tools. Rust is certainly that, and *I will never hesitate to recommend that a programmer learn Rust.* At the same time, new means the tool has not stood the test of time, and interesting means it is probably unique and clever, and we know where that leads.

In the world of computer programming, simplicity results in fewer bugs and less downtime [[17]](https://www.gkogan.co/blog/simple-systems/), so complexity should be moderated as much as possible. Rust’s novel way of managing memory adds complexity that must be managed by the programmer. On the other hand, garbage collected languages manage memory for the programmer. For that reason, **if a program can run sufficiently using a garbage-collected language, choose the garbage-collected language.**

If a program needs to use as few resources as possible, Rust may be a viable option. But also keep in mind that Rust’s assurance of memory safety does not negate the need for careful planning and thoughtful design. I won’t try to argue whether Rust is better than C or C++ in resource-critical applications, but if I did, it would be a hard case to make on either side, and there would be many things to consider including team size, expected program size, and whether the program will be exposed to the internet.

Considering the facts that a good portion of software does not need to run at C’s close-to-the-metal speeds, and Rust obtains memory safety by encouraging the programmer to manage and add complexity, Rust is not a good choice for many programs.

### References

[1] [http://nomodes.com/Larry_Tesler_Consulting/Complexity_Law.html](http://nomodes.com/Larry_Tesler_Consulting/Complexity_Law.html)

[2] [https://blog.discordapp.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f](https://blog.discordapp.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f)

[3] [https://msrc-blog.microsoft.com/2019/07/22/why-rust-for-safe-systems-programming](https://msrc-blog.microsoft.com/2019/07/22/why-rust-for-safe-systems-programming)

[4] [https://blog.cloudflare.com/tag/rust/](https://blog.cloudflare.com/tag/rust/)

[5] [https://www.tutorialspoint.com/software_quality_management/software_quality_management_metrics.htm](https://www.tutorialspoint.com/software_quality_management/software_quality_management_metrics.htm)

[6] [https://blog.debiania.in.ua/posts/2020-03-01-rust-dependencies-scare.html](https://blog.debiania.in.ua/posts/2020-03-01-rust-dependencies-scare.html)

[7] [https://medium.com/@shnatsel/smoke-testing-rust-http-clients-b8f2ee5db4e6](https://medium.com/@shnatsel/smoke-testing-rust-http-clients-b8f2ee5db4e6)

[8] [https://internals.rust-lang.org/t/rust-s-freedom-flaws/11533](https://internals.rust-lang.org/t/rust-s-freedom-flaws/11533)

[9] [https://www.microsoft.com/en-us/research/publication/the-influence-of-organizational-structure-on-software-quality-an-empirical-case-study/](https://www.microsoft.com/en-us/research/publication/the-influence-of-organizational-structure-on-software-quality-an-empirical-case-study/)

[10] [https://wiki.c2.com/?LanguagesWithoutGarbageCollection](https://wiki.c2.com/?LanguagesWithoutGarbageCollection)

[11] [https://www.memorymanagement.org/mmref/lang.html](https://www.memorymanagement.org/mmref/lang.html)

[12] [https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)

[13] [https://kk.org/thetechnium/triumph-of-the/](https://kk.org/thetechnium/triumph-of-the/)

[14] [https://softwareengineering.stackexchange.com/questions/25276/why-is-cleverness-considered-harmful-in-programming-by-some-people](https://softwareengineering.stackexchange.com/questions/25276/why-is-cleverness-considered-harmful-in-programming-by-some-people)(https://news.ycombinator.com/item?id=20386073).)

[15] [https://news.ycombinator.com/item?id=20386073](https://softwareengineering.stackexchange.com/questions/25276/why-is-cleverness-considered-harmful-in-programming-by-some-people)(https://news.ycombinator.com/item?id=20386073).)

[16] [https://msrc-blog.microsoft.com/2019/07/16/a-proactive-approach-to-more-secure-code/](https://msrc-blog.microsoft.com/2019/07/16/a-proactive-approach-to-more-secure-code/)

[17] [https://www.gkogan.co/blog/simple-systems/](https://www.gkogan.co/blog/simple-systems/)

[18] [https://www.reddit.com/r/Amethyst/comments/emjqai/what_exactly_is_gamedata/](https://www.reddit.com/r/Amethyst/comments/emjqai/what_exactly_is_gamedata/)

[19] [https://stackoverflow.com/questions/1897537/why-are-circular-references-considered-harmful](https://stackoverflow.com/questions/1897537/why-are-circular-references-considered-harmful)
