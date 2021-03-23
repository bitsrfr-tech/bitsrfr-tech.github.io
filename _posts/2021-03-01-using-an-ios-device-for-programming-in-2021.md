---
title: Using an iOS Device for Programming in 2021
date: 2021-03-01
---

A couple of years ago, when someone asked if iOS could be used for software development, the answer was simply "no." That is no longer the case. While the answer is still not an emphatic, "yes," it is a solid, "it depends." In fact, you might be surprised to find out how far development on iOS has come in the past year or two.

For the majority of developers, iOS is a so-so development system - usable but not the best option. For developers who are just SSHing from server to server, or merging Git requests, iOS is great. For other developers, iOS just won't cut it. Programmers building 100,000-line Windows applications are still better off using Windows.

Beyond that, it comes down to personal preference. Some developers prefer iOS's multitasking, shortcuts, and configurations. Some don't.

Most people can get away with using an iPad as their daily driver at this point. However, a good portion of those people would still benefit by having a more traditional operating system (MacOS, Windows 10, Ubuntu, etc) for the times when an iPad doesn't cut it. Those times are becoming rare, but there are still users, for example, who might need to use an application that is only available on a desktop operating system. Another example, and this is a strange one, is iOS app development. It is one of the great paradoxes of modern technology that professional iOS developers cannot complete their work on iOS. Hopefully that will change soon.

Ultimately, without talking to you personally, I cannot say whether iOS would suit your development needs. I can, however, show you what the iPad has to offer. I will go over some general use cases. My hope is to help you choose the best system for you, or to at least provide some interesting insights.

----

# Five Reasons iOS is Suitable for Software Development
1. There are several compilers available right in the App Store. The most popular of these are [LibTerm](https://libterm.app/), [BASIC](https://apps.apple.com/us/app/basic-programming-language/id1540244170), and [SeeLess](https://seeless.app/). It is possible, with those mentioned and other App Store apps, to compile code written in C, C++, BASIC, Rust, Java, Swift, Go, and other languages, right within the iOS operating system.

2. In addition to all of those compilers, the App Store has some excellent and powerful scripting apps. You can write and run Python ([Pythonista app](http://omz-software.com/pythonista/)) and JavaScript ([Scriptable app](https://scriptable.app/)) scripts right on an iPhone or iPad. You can also use Apple's visual scripting app, [Shortcuts](https://support.apple.com/guide/shortcuts/welcome/ios).

3. **It is even possible, and dare I say easy, to emulate a full version of Alpine Linux on iOS.** The free and open source [iSH](https://ish.app/) app makes it possible. So, yes, you can use Vim on your iPad.

4. The App Store also has no shortage of Git clients, SSH clients, IDEs, and text editors. [Working Copy](https://workingcopyapp.com/) is a very popular Git client available on the App Store. [Blinkshell](https://blink.sh/) is a popular, and open source, SSH client available on the App Store. [Play.js](https://playdotjs.com/) is a Node.js and JavaScript IDE. [Textastic](https://www.textasticapp.com/) is a feature-rich text editor (that I absolutely love!), complete with Markdown and great file syncing features including an SFTP client and an SSH client.

5. [Developer Tools](https://sensortower.com/ios/rankings/top/ipad/us/developer-tools?date=2021-02-23) is a *Top Category* in the App Store. This bodes well for developers who want to use iOS. First, it means Apple is serious about getting developers onto the operating system. Second, it makes finding great development tools very easy. [Click here for instructions on viewing the *Developer Tools* category in the App Store](/2021/03/15/browse-dev-tools-in-app-store.html).

----

# Five Shortcomings of iOS for Software Development
1. The newness of development and programming on iOS is evident in the immaturity of many development tools. Most compilers in the App Store get not-so-great ratings, and many development apps have ugly or unintuitive user interfaces. The good news here is that there is a lot of opportunity for developers to build and sell new development apps.

2. It is good practice to develop on the operating system that you are developing for. If you are developing Windows apps, it is probably a bad idea to use iOS as your development environment.

3. If you are building 100,000+ line enterprise applications, you probably aren't going to feel quite at home if iOS is your main environment. On the same token, if you are developing enterprise applications, you probably have servers and central repositories that you are pushing to. In that case, it may very well suit you to develop on iOS.

4. iOS does not have good external monitor support. An external monitor can be connected, but it will only display in a 4:3 aspect ratio. However, the keyboard shortcuts and multitasking capabilities of iOS can, in many cases, reduce the need for multiple monitors.

5. The cost of an iPad plus a keyboard, and possibly a mouse/trackpad, is not low. If you are looking for an inexpensive development machine, an iOS device is probably not going to suit your needs.

Bonus shortcoming: iOS is not open source. Though, not everyone will consider this a shortcoming.

----

# Use Cases for Programming on iOS
## Professional Game Developer
For the most part, professional game developers will not be well served using iOS as their main development environment. Popular game development tools like Unity and Godot are not available on iOS.

## Hobbyist Game Developer
Hobbyist and amateur game developers are also generally better off with a desktop operating system. That said, there have been games developed using, among others, the Pythonista app on iOS. [Here is a nice GitHub repository of games made with Pythonista](https://github.com/Pythonista-Tools/Pythonista-Tools/blob/master/Games.md).

## Professional iOS Developer
This is where things get a bit awkward. Professional iOS developers are not only better off using MacOS for development, it is a requirement. Xcode, which is only available on MacOS (but rumored to be coming to iOS), is required for publishing iOS apps to the Apple App Store.

## Hobbyist iOS Developer
This is really the sweet spot for using iOS as a main development machine. Hobbyist developers will have a field day - a field year! - coding on iOS. There are many great tools to explore and there is lots of innovation happening on iOS right now.

## Network and/or System Administrator
A large portion of an administrator's job involves remotely accessing servers and clients. iOS has some great SSH tools. As long as admins are okay with doing that on an iPad screen, I think they can make a nice home on iOS. In fact, [Christopher Lawley, an iOS YouTuber](https://www.youtube.com/channel/UC8raOG7HXJoCUygx219fU4A) (great channel if you want to learn more about using iOS for productivity and scripting) says he used an iPad Pro when he worked as a network administrator.

## New Developer Learning to Program
For someone learning to program, I suggest using a desktop operating system like MacOS, Windows, or Ubuntu, because you are going to find better support and help online when you need to troubleshoot or figure out how to do something. Developing on iOS is still new, so there's not a lot of documentation out there. Plus, you are going to learn more about the inner-workings of a computer on a system that exposes more of its inner-workings, and iOS exposes the least of any of them. That said, if you really just want to do it on iOS, I say go for it! And if your only computer is an iPad, then by all means hack the heck out of it!

## Tech-Degree-Seeking College Student
While the iPad can make a great note-taking, task-tracking, and web-surfing device, I would not recommend it as a daily driver for college students who are seeking a degree in computer science or information systems unless you are living on-campus and are willing to use campus-provided computers/labs for your work. Most college programming courses will require the use of Windows or MacOS, and the ability to install various tools on those systems. iPad is not quite flexible enough for that situation.

----

Hopefully I have provided a few key pieces of information to help you decide whether your next development machine will be an iOS device. Will you or won't you? Can you think of any other pros or cons that I haven't mentioned? Let me know!

Until next time, happy hacking!
