# IKEA Ansluta
IKEA *Ansluta* 433 MHz remote on/off switch reverse engineering

This is my humble attempt at analyzing and replicating the transmissions of the IKEA *Ansluta* remote on/off switch. (Note: There are at least two different versions of this remote; this is about the simple switch without dimming.)

Manuals:
 - http://www.ikea.com/en/en/manuals/ansluta-remote-control__AA-804078-1_pub.pdf
 - http://www.ikea.com/en/en/manuals/ansluta-remote-control__AA-855696-2_pub.pdf

The plan:
 - find transmitter on the PCB
 - capture `on`/`off` transmissions at the source
 - figure out the code (optional)
 - replicate the code with own transmitter

## Finding the transmitter
Unfortunately I’m no electrical engineer, far from it: I’m utterly clueless. Let’s just open this thing up and have a look. I have another 433 MHz remote control for reference.

![433-remote](https://cloud.githubusercontent.com/assets/532114/7789413/94ef9804-025f-11e5-81aa-23dbd05f2c6f.jpg)
![ikea-ansluta-remote](https://cloud.githubusercontent.com/assets/532114/7789412/9057bdc6-025f-11e5-99b5-0955478883b6.jpg)

The reference remote is extremely simple: an 18 pin IC labelled `HX2262`, a led, a few switches and button pads, and the [crystal oscillator](https://en.wikipedia.org/wiki/Crystal_oscillator) (round metal package) labelled `433.92`.

The *Ansluta* is more crowded, but we see a 16 pin IC, this time labelled *Holtex* `HT48R06A-1`, and a crystal (oblong metal package) labelled `T13.560` – I’m not sure whether there’s any significance to the fact that this is *not* a 433 MHz crystal.

After trying a few of the IC’s pins I see that a led attached to pin 9 (the fifth on the bottom row in the picture) flickers suspiciously suspiciously when pressing the `on`/`off` buttons – this must be it.

## Capturing the signal (and possibly decoding it)
Here’s what I got from that pin (when pressing `on`) — this was repeated seven times:

![ansluta-on](https://cloud.githubusercontent.com/assets/532114/7789557/01efedc2-0266-11e5-9d33-528f1fdb24f7.png)

Looks good, doesn’t it? An initial long `sync` pulse followed by a sequence with nice even spacing of about half a ms. More precisely, the periods of `high` voltage are slightly under half a ms, while a (single) `low` is slightly longer: `0.489±4%` vs `0.507±2%`.

Let’s call them `1` and `0` *bits* (even though they’re not) until the coding is figured out. Then the sequence looks like this:

 - `100000000000000000000000000000000000` (1 + 35 bits), then
 - `10101010100101010010010101001010010100100101001010100100101001010100100101010010100101001010010100101001001010010100101001010010` (128 bits).

Okay, I feel slightly uncomfortable with this because of the uneven lengths, but clearly it’s a sequence of `10` and `100`s. So let’s call those `1` and `0`, yielding this shorter representation:

 - `1111011001101010010110010110011010101010100101010101` (`on` code),
 - `1111011001101010010110010110011010101010100110101010` (`off` code).

Notice that all but the last eight bits are identical, and that those eight bits are precisely inverted: `01` (times four) for `on`, `10` for off.
