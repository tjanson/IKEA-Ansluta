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
Unfortunately I’m no electrical engineer, far from it: I’m utterly clueless. Let’s just open this thing up and have a look. I have a 433 MHz remote control for reference.

![ikea-ansluta-remote](https://cloud.githubusercontent.com/assets/532114/7789412/9057bdc6-025f-11e5-99b5-0955478883b6.jpg)
![433-remote](https://cloud.githubusercontent.com/assets/532114/7789413/94ef9804-025f-11e5-81aa-23dbd05f2c6f.jpg)

The reference remote is extremely simple: a 16 pin IC labelled `HX2262`, a led, a few switches and button pads, and the [crystal oscillator](https://en.wikipedia.org/wiki/Crystal_oscillator) (round metal package) labelled `433.92`.

The *Ansluta* is more crowded, but we see another 16 pin IC, this time labelled *Holtex* `HT48R06A-1`, and a crystal (oblong metal package) labelled `T13.560` – I’m not sure whether there’s any significance to the fact that this is *not* a 433 MHz crystal.
