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
