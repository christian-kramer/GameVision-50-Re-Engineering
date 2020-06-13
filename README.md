# GameVision-50-Re-Engineering
Turning a bizarre Russian/Taiwanese NES clone into something useful.
___

I've been definitely been looking in strange places for personal projects to do, lately. During my search I stumbled upon an old all-in-one game console/controller I used to play with when I was young.

This project is a continuation and implementation of my [STM32F103C8T6-Familiarization repo](https://github.com/christian-kramer/STM32F103C8T6-Familiarization)

![GameVision 50](https://i.imgur.com/y6f7k1Y.jpg)

It's called the **GameVision 50**, presumably because it has 50 games.

![Loading](https://i.imgur.com/X0x8wN1.jpg)

Here is the full list of games, in case anyone recognizes one.

![Page 1](https://i.imgur.com/kHe6Co5.jpg)
![Page 2](https://i.imgur.com/1G9vRXh.jpg)
![Page 3](https://i.imgur.com/htxYzJD.jpg)
![Page 4](https://i.imgur.com/NKpHRS6.jpg)

And, as an example, here's "Truck Race":

![Truck Race](https://i.imgur.com/qHOYugw.jpg)

The games aren't exactly sophisticated... not quite the level of immersion I'm looking for. The wheel is nice, though. I wonder how hard it would be to adapt it for use with a PC?

Let's open it up and have a look.

![Opened](https://i.imgur.com/zB3j0u8.jpg)

Seems a lot less complicated than I thought it would be, actually. Let's take a closer look at that mainboard.

![Close-up](https://i.imgur.com/Eze8Wvi.jpg)

Interesting layout, for sure. The big chip on the right is almost certainly a parallel flash IC, the chip-on-board in the lower-left is probably the main game processor (more on that in a bit), and quite honestly I have no idea what the "chip-on-board, on-board" thing in the top left is. It's gotta be a coprocessor of some sort, because it's thoroughly interconnected with both the game chip, and the flash chip. A converter between parallel flash and whatever interface the game processor requires, perhaps?

I'm interested in that connector on the far right of the board. It definitely doesn't appear to be a multiplexed analog button arrangement... the pin names like "SCK1", "OUT0", and "VCC" seem to indicate we've got some other device at the other end of this cable speaking a serial data language. I have never heard of "OUT0" in any serial data protocol I've worked with.

I googled some of these pinout names, and the first result on Google was [this webpage](http://xn----ctbgeuhdtdb2b.xn--p1ai/price/dendy/?parts&d=DW-MH-3):

![Webpage](https://i.imgur.com/fidm5OU.png)

Woah! That's a lot of Russian. I can read some Russian, but not stuff that's this technical.

![Translated](https://i.imgur.com/A5Q3zKA.png)

That's better. Looks like it's actually pretty straightforward. Dendy DW-MH-3? I've never heard of the company "Dendy Processors" before. I really want to know more about this chip.


VCC gets +5v, OUT0 is a common clock, SCK1 and SCK2 are clocks specific to each controller, and P1D0/P2D0 are the lines that the controllers transmit data on.


I've got a logic analyzer I can hook up to this and see if I can recognize a pattern.

![Logic Analyzer](https://i.imgur.com/iWwLtU6.jpg)

Next, let's fire it up and press some buttons!

![Output](https://i.imgur.com/y2ItXpD.png)

The top line is SCK1, and the next line down is P1D0. Looks like we've got a 2-byte "packet" clocked out by SCK1, with a little bit of a pause separating them. I'm curious, though... The webpage mentioned that OUT0 is a "common clock"... What does its output look like?

![OUT0](https://i.imgur.com/FRWRZtJ.png)

Ah, okay! OUT0 appears to delineate each byte. It also looks like no matter which button I press on the wheel, both bytes are identical. I wonder if this is some crude error-checking going on?

Also, interestingly enough, this output is from the wheel being turned all the way to the left, meaning that although there's a potentiometer attached to the steering column, there is no analog data coming back to the Dendy chip... turning the wheel is functionally equivalent to pressing the left or right d-pad buttons! This will need to be rewired if we want this to function as an analog axis.

At this point, I think I'm comfortable enough with the Dendy controller protocol to try hooking this up to an STM32 Blue Pill.

![Blue Pill](https://i.imgur.com/ONhLSSB.jpg)

What you see here is the steering wheel controller of the GameVision 50 bypassing the original mainboard and communicating entirely with the STM32. Interestingly, it's able to interface directly with the STM32 without level-shifting because STM32 inputs are 5v tolerant, and 3.3v is above the TTL "logic high" threshold. The potentiometer on the breadboard is a stand-in for the one that's built-in to the steering column. I'll use that potentiometer in the final version.

After I got it talking with my PC and being recognized as a "Game Pad" in Windows with a single "steering" axis, it was time to take this rat's nest of wiring and turn it into a PCB!

![PCB](https://i.imgur.com/mTZuEmP.jpg)


I decided to keep the same shape and general layout as the original mainboard so that it may be a drop-in replacement/conversion. It felt much more professional, that way. Additionally, I broke out the steering wheel connector into test-points so that I may easily connect my logic analyzer in the event that I needed to debug my firmware in the future. The header for that is labeled J4, to the left of J5: the controller connector.
