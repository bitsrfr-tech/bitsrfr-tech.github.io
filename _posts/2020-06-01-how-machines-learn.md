---
title: How Machines Learn
date: 2020-06-01
---

# How Machines Learn

Machine learning isn’t magic.

A human gives a machine (computer) an answer (a number or piece of data) that the machine needs to come up with. Then, the machine goes through every possible scenario (pattern) that could come up with the answer it was given.

With each pattern, it remembers the whether the outcome was the correct answer. It repeats all the scenarios many times because sometimes there are weird coincidences. After many repetitions, the machine has statistics indicating which pattern or patterns yield which answers most often. At that point, the machine has learned what patterns are needed to come to the correct answer.

Here is a very simplified example of how a robot might learn to safely cross a road.

## Meet Bob the Learning Robot

Bob is a robot. He is learning how to cross the road safely.

![](/images/HowMachinesLearn_Img_1.png)

Bob starts on sidewalk tile 7. Bob crosses the road. Bob gets hit by a car. He doesn’t die because robots do not die. Instead, Bob remembers he was hit when he tried to cross at tile 7.

After a little repair work on his legs, Bob moves forward one tile, to sidewalk tile 6. Bob crosses the road. Bob gets hit by a car. Bob stores this in his memory. Bob moves back to sidewalk tile 7.

Bob moves forward two tiles, to sidewalk tile 5. Bob crosses the road. Bob is successful! Bob stores this in his memory. Bob moves back to sidewalk tile 7.

Bob tries sidewalk tile 5 again. Okay, maybe robots can die. Bob is seriously injured. That’s okay, a little welding will fix him up. Bob stores this in his memory. Bob moves back to sidewalk tile 7.

Bob moves forward three tiles, to sidewalk tile 4. Bob crosses the road. Bob gets hit by a car. Bob stores this in his memory and moves back to sidewalk tile 7.

![](/images/HowMachinesLearn_Img_2.png)

Bob moves forward 4 tiles, and crosses the road. He does not get hit by a car. He stores this in his memory and moves back to sidewalk tile 7.

Bob repeats this process many times. Every once in a while, Bob gets hit by a car even when he crosses at tile 3, and every once in a while he gets across unscathed when he crosses at a different tile. Over time, Bob builds up a large sampling of data and finds the pattern that allows him to cross the road with high success: move from sidewalk tile 7 to sidewalk tile 3, then cross.

You see, there is no real magic in machine learning. Computers are much faster than humans at calculating things. That makes them great at figuring out how to solve mathematical problems that we haven’t figured out yet.

The computer goes through every possible pattern in a short amount of time, keeps track of the answers that the patterns lead to, and has eventually learned something new. That’s how machines learn.
