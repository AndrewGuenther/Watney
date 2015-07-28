# Watney
An attempt to replicate the rover hack described in Andy Weir's *The Martian*

# *The Martian*: TL;DR
**WARNING contains spoilers through Chapter 12!**

Mark Watney, presumed dead, is left behind by his fellow astronauts alone on
Mars. Injured, but alive, he returns to the crew's 92 sq. ft. base, known as
the *Hab*. Using his engineering know-how, he manages to survive for the next
97 days and make contact with NASA. Then things get interesting.

Watney retrieves an old rover called *Pathfinder* and its more mobile little
brother *Soujourner*. In an attempt to communicate with Earth, he successfully
repairs *Pathfinder*, which begins to transmit to NASA.

Watney's initial method of communication with NASA is through a set of
notecards, each with a hex value (0-F) written on it, placed in a circle around
an *Pathfinder's* camera. Watney observes which card the camera is pointing at,
makes a note, and converts the hex into ASCII by hand. Since this is obviously
not the most effective form of communication, the guys at NASA come up with an
idea to hack Watney's rover computer to be used as an interplanetary
not-so-instant messenger. Things now get even more interesting.

# The Hack
(If you own the book, you can read a more detailed description of the hack on
pages 121-128)

Thankfully, NASA's plan in the book is actually fairly detailed. Enough so,
that I intend to replicate it. Here is goes.

*Pathfinder* can send signals and camera images to NASA as well as communicate
with *Soujourner*. NASA has a way to remotely update *Pathfinder's* software,
but not Watney's rover. The idea is to update *Pathfinder* so that rather than
communicating on *Soujourner's* frequency, it will communicate with Watney's
rover by spoofing the signal beacon signal trasmitted by the *Hab*.

Since the rover has no way to interpret these new signals, nor does it have a
way to respond to them, the rover will also require a software update.
Watney could manually patch the rover's software, but NASA
says that the full patch is 20MB. No good. Instead, Watney will simply modify
a shared object file on the rover to write out data it receives to a log file
which he will then execute, a 141 byte patch. Just barely too much for a tweet.
Damn.

# The Challenge
Let's pretend to be NASA and write this rover patch ourselves. Here are the
challenge parameters:

* Write a shared library which receives messages over a TCP socket, and checks
    for a magic value (The value sent by the *Hab's* beacon).
* Write the smallest possible patch in assembly which can write the received
    data to a file. (**Bonus** make sure the patch is 141 bytes or less)
* Use an instruction set which NASA has actually used in the field.

# The Technical Details

1. **What architecture will be targeted?**
  * The Curiosity rover uses a RAD750 processor which runs the PowerPC v1.1
      architecture. So I guess we'll go with that.
1. **How long is the Hab beacon's magic value?**
  * The book does not specify, pending consulation.
1. **If the *Hab* beacon is constantly transmitting, do we need to filter it
    out first?**
  * We'll call this a bonus. Technically, yes, but I highly doubt we can meet
      our goal patch size taking this into account.
1. **Does the final patch have some form of termination sequence that we need
   to watch out for?**
  * Again, we'll call this a bonus.
