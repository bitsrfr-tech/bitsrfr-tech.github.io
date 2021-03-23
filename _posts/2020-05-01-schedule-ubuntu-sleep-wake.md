---
title: Schedule Ubuntu to Sleep and Wake at Specific Times
date: 2020-05-01
---

# Schedule Ubuntu to Sleep and Wake at Specific Times

I use Ubuntu (Xubuntu, to be exact) for [the NAS/media server that I built out of an old ThinkPad](https://medium.com/@JoeKreydt/i-built-a-2-bay-nas-media-server-for-256-disks-included-1f4278cd36bc). Old ThinkPads work great for NAS devices, but sometimes the fan noise can be annoying, like when I’m trying to sleep. In order to solve that problem, I decided I would make the NAS sleep between 10:00pm and 6:00am every night.

Ubuntu comes with all the tools needed to accomplish this task, but the documentation on doing such a thing is limited. So here I am writing a post on how I did it, in hopes that it will help my fellow techies.

### Tools I Used

* Ubuntu or any of its flavors, like Xubuntu or Lubuntu

* rtcwake (comes with Ubuntu desktop)

* crontab (comes with Ubuntu desktop)

* bash (comes with pretty much every Linux distro ever made)

* nano editor (or any other text editor)

## How I Put Ubuntu on a Sleep/Wake Schedule

### Create Bash Script to Run Rtcwake

First, I created a bash script that would run *rtcwake*. *Rtcwake* is a simple command line tool that immediately suspends the system and then wakes it when specified. In my case, I set it to wake at 6:00am EST.

    #!/bin/bash

    #sleep, then wake at 6:00am tomorrow

    rtcwake -m disk -l -t $(date +%s -d ‘tomorrow 02:00’)

I saved that to my *home* folder as *sleepAndWake.sh*.

Then I made it executable by navigating to my *home* folder in a terminal session and entering:

    chmod +x sleepAndWake.sh

### Schedule Script with Crontab

Next, I ran crontab in a terminal. Crontab is a tool for scheduling tasks to run at a specific time/date. Scheduling tasks with crontab is as simple as adding a line of text to a file. To open the crontab file, I entered

    sudo crontab -e

The *sudo* part is necessary because *rtcwake* won’t run properly without super user permission. For scheduled tasks that don’t need to run with *sudo*, I use

    crontab -e

Each of the above *crontab* commands will open a different file. The “-e” switch tells *crontab* I want to edit the *crontab* file.

After entering the command for the first time, I was prompted to select a text editor. The options are all command line editors. I chose *nano*. In my opinion, it is the most intuitive.

When the crontab file opened, I scrolled to the bottom (with *nano*, I used the arrow keys to scroll) to enter my new *crontab* task.

*Crontab* follows a very specific format:

    minute hour day month weekday command

*Minute*, *hour*, *day*, *month*, and *weekday*, each take a number.

* Minute: 0–59

* Hour: 0–23

* Day: 1–31

* Month: 1–12

* Weekday: 0–6

It can also take an asterisk (*) as a wildcard in place of any of the numbers to run the command every minute, hour, day, etc.

I wanted my *sleepAndWake* script to run every day, every month, and every weekday, at 10:00pm. For that, I added this line to my *sudo crontab* file.

    0 22 * * * sudo /home/joseph/bin/sleepAndWake.sh

The command portion of the crontab line starts with sudo. Even though I added the line to my *sudo crontab* file, I still had to add *sudo* to the beginning of my command for it to run with super user privileges.

After “sudo”, I specified the location of the script, “/home/joseph/bin/”, and the script name, “sleepAndWake.sh”.

Finally, I saved the *crontab* file (with *nano*, I used *Ctrl+x *which prompts to save and then exits the file).

And that is it! My NAS now sleeps every night at 10:00pm and wakes every morning at 6:00am. *Crontab* starts my *sleepAndWake* script at 10:00pm, which runs *rtcwake*, which puts the computer to sleep and then wakes it up at 6:00am. Now, if only I could get my brain to sleep and wake on such an exact schedule…

## Helpful Resources

If you are having trouble, or don’t want to think too hard, figuring out what to put in your crontab file, [here is a neat little website that makes it easy](https://crontab.guru/).

If you’re interested in reading further, here are the resources I used to learn about *rtcwake* and *crontab*:

* [Rtcwake Examples on AskUbuntu](https://askubuntu.com/questions/1009684/sleep-wakeup-schedule-ubuntu-16-04-3-lts/1009688#1009688?newreg=0f946364396346dd91377f834a12c50e) (note: these examples aren’t quite accurate, but they point in the right direction)

* [Rtcwake Linux Man Page](https://linux.die.net/man/8/rtcwake)

* [Intro to Crontab Files on HowToGeek](https://www.howtogeek.com/101288/how-to-schedule-tasks-on-linux-an-introduction-to-crontab-files/)

* [When to Use Sudo with Crontab on ServerFault](https://serverfault.com/questions/817499/when-to-use-sudo-with-crontab)
