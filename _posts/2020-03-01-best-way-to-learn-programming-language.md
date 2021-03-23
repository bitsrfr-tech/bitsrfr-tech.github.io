---
title: The Best Way to Learn a Programming Language
date: 2020-03-01
---

# Learning Rust by Hacking Asteroids

In much the same way that GNU is a recursive acronym (**G**NU is **N**ot **U**nix), learning a programming language is a recursive process. We learn the language as we program in the language. Which comes first, the learning or the programming? Neither, really. It’s recursive.

The best way to learn programming is by doing programming. Realizing that, when I decided to learn Rust I did not buy any books or take any courses. Instead, I hacked a simple game — an Asteroids clone. It was challenging, but it was the best language-learning experience I have ever had. Making my own game was fun, and having concrete examples of complex Rust concepts made the learning process fast and painless.

### Why Not Just Read a Book?

There are many ways to learn a programming language. You can read a book, you can take a course, you can watch a video, you can build something from scratch, or you can find a pre-existing program and hack it up. Each method of learning has its place, but you won’t *really* learn to program until you actually start writing code.

Let’s say you want to learn Rust, like I have, so you pick up [“The Book” ](https://doc.rust-lang.org/book/)— Rust’s de facto user guide. You get through the “Hello, World” sections, which do not teach you much about the language. You learn a bit from the guessing game example chapter. Then, you get to the parts that actually teach Rust, and you read this:
> “As mentioned in Chapter 2, by default variables are immutable. This is one of many nudges Rust gives you to write your code in a way that takes advantage of the safety and easy concurrency that Rust offers. However, you still have the option to make your variables mutable. Let’s explore how and why Rust encourages you to favor immutability and why sometimes you might want to opt out.”

After reading that, you are probably either confused or falling asleep. The stuff is dry, and, if you aren’t familiar with systems level programming, almost incomprehensible.

The good news is, to start programming in Rust, you don’t really need to know the definition of immutability or all the different data types. I’ve found it best to just learn that stuff as I go. Otherwise, it is just a bunch of meaningless words.

### Why Not Build Something from Scratch?

Watching videos and taking courses are similar to reading. At the end of the day, you have to write some code to really learn the language. At that point, the options are building something from scratch or hacking something. If you have something simple to build, starting from scratching isn’t the worst thing in the world. If not, hacking is the way to go.

Learning to program by digging into a program’s source code has some distinct advantages. With a structure already in place, it is easy to get some quick victories. Those victories make learning the language fun and exciting. Hacking a pre-existing program also has a low barrier of entry. Building a program from scratch comes with some hurdles right from the start. You have to get the basic structure, the foundation, in place. That is not the case when the program has already been built.

I have tried every method of learning to program. I have suffered through the incomprehensible chapters of books, boring lectures, and tedious videos, and I have trudged through the mud of building a program from scratch with little knowledge of the language. So when I decided to learn Rust, I chose to do it by hacking. I had the most fun I’ve ever had learning a programming language, and I was shocked at how fast I learned it.

## How I Learned Rust by Hacking an Asteroids Clone

My experience of learning Rust by hacking a game was like ripping off a band-aid. I was careful as I pulled up the edge — I read a couple blog posts to get a feel for the language. Then, I got right in there and ripped it off. I found a simple game engine, ggez, that has some example games on its GitHub repository. I chose the coolest of the engine’s example games, Astroblasto. I learned how to compile and run it with Cargo, Rust’s amazing package manager. Then, I started hacking it into my own game. That was when the serious learning happened.

Hacking provided a lot of early victories that motivated me to keep going. By hacking the Asteroids clone, I was dealing with traits and structs (foundational, but confusing, features of the Rust language) within the first couple hours. I was making concrete, visible progress right away.

### Finding a Program to Hack

Aside from just diving in, another useful way of actually coming to understand something is to watch someone else do it. I found [this talk by Lisa Passing about how she made a game in Rust](https://www.youtube.com/watch?v=Ktwl97Ph-SI). That talk is really what inspired me to make a game with Rust in the first place. It also made me aware of my new favorite game engine, ggez (good games easy).

I disregarded the ggez game engine at first. Instead, I chose Amethyst which seemed to be the most actively developed Rust game engine. I got Amethyst set up and tried compiling one of the example games, and it hard-failed. It turns out the Amethyst pong tutorial example game uses the Vulkan graphics driver, and my virtual machine is not capable of Vulkan-ing. After wasting some time figuring that out, I decided to try ggez. I decided to hack its example game, an Asteroids clone called Astroblasto.

### My First Rust Pull Request on GitHub

I had a small problem compiling ggez’s Astroblasto example game. I needed to add a dependency or two to my cargo.toml file. That was an easy problem to solve, and I submitted a pull request to ggez on GitHub to fix it for others. The pull request got a fast and kind response from the developer. At that point, I was glad I ended up with ggez as a partner on my Rust learning journey.

### A Few Simple Hacks

The real language learning started with a simple hack, changing the player graphic. It was just a matter of changing the image file in the code, so I knew I could succeed. Instead of an arrow-shaped spaceship (try saying that 10 times in a row), I created a red car in GIMP and used that.

Then, I turned the spaceship’s bullets into pizzas, another easy hack to get the ball rolling. After changing the bullets into pizzas, I realized the pizza speed was too fast. It wasn’t easy to tell that the bullets were supposed to be pizzas, so I slowed down the shot speed. Slowing the object’s speed caused another issue. The pizzas disappeared before they got to the edge of the game window. To fix that, I adjusted the lifetime length of the bullet/pizza.

### Hacking Deeper

Each change created new bugs and challenges, yet each change brought the game closer to what I had envisioned. Most importantly, each new change forced me to learn more about Rust. I changed where the asteroids spawn and learned about variable declarations and types. I added a big green planet and learned about functions and the nalgebra Rust library. I added and removed some collision detection and learned about loops and immutability.

In almost no time at all, I had experience with several of Rust’s major features, including structs, traits, enums, and crates. I also gained familiarity with the layout of Rust’s documentation pages (docs.rs). As a bonus, I learned about game development, Git, and ggez. Some of the changes were more challenging than others, but the difficult ones taught me the most.

### Looking Back on My Journey

It was not an easy journey. There were compiler errors and new bugs every step of the way, and I am not at the finish line yet. But having something to build, a reason to use the language, has really made learning Rust a wonderful experience. I have come to enjoy the challenges presented by Rust’s strict compiler, I found a great community around the language, and soon I will have a cool game to show all my friends.

If you are looking to learn a new programming language, I highly recommend finding some example code (with a free license like MIT) and hacking it into your own creation. Hacking Astroblasto into my own game was one of the best learning experiences I’ve ever had, and I couldn’t believe how fast I learned complex Rust programming concepts by having concrete examples in front of me.

If you’re interested in seeing the code or contributing to my game, or hacking it into your own game, you can [find it on my GitHub](https://github.com/joekreydt). It’s called Pizzaboy. If you just want to see a play-by-play of how I hacked the game, all of my commits are clearly labelled. If you want to work on it with me, I have a wide variety of Issues (basically GitHub’s version of a to-do list) that are great for anyone interested in Rust, pixel art, or game development. Stop by, try out the game, hack the code, and have fun!
