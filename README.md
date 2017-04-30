Pibeanie Stereo15
=================
a [Raspberry Pi] stereo amplifier [HAT] with 2 x 7.5 Watt HiFi output power.

The Problem
-----------
There are stereo amplifier hats on the market like the [Hifiberry Amp+] which work fine and are affordable. I own a Hifiberry Amp+ but this hat has some limitations which result in this little PCB project.

### Existing Limitations ###
- no possibility to mount the [Raspberry Pi 7" display] in an easy way together with the amplifier hat because the PCB has no cutout for the display flex-cable
- no pin header to supply the [Raspberry Pi 7" display] from the hat directly
- on-board 5 Volt DC/DC converter is limited to ~1.0 Ampere
- no infrared receiver

and last and least: as a professional PCB designer I was a little bit shocked by the PCB design of the [Hifiberry Amp+]. There are very small traces where high currents flow and the component placement of the audio output stage (HF filters) and step-down converter is ridiculous. I expect a lot of unwanted high frequency noise with that PCB design.

The Idea
--------
- a PCB which is kernel-driver-compatible to [Hifiberry Amp+]. That's easy because the audio IC is TI's [TAS5713] which is supported by the Linux kernel and there is only one possibility to connect this chip to the [Raspberry Pi].
- 5V / 3A step-down converter and the possibility to supply the [Raspberry Pi 7" display]
- infrared receiver connectable e.g. [Vishay TSOP32xxx Series]
- EEPROM for device tree BLOBs
- reverse polarity protection
- inrush current limiter

----------

The Realized Board
------------------
My Pibeanie Stereo15

![Pibeanie Stereo15][pibeanie-stereo15.jpg]

a cheap, proof of concept, 2-layer PCB which does work really proper

- [AO4803A] P-FET, with <46mOhm RDSon does the reverse polarity protection and in-rush current limiting.
- [TI TPS62133] 5V/3A step-down converter powers the Raspberry Pi and easily an additional Raspberry Pi 7" display ... and USB hard drives.
- [TAS5713] class D amplifier with a superb output filter design and superior 15000 hour low ESR (23mOhm!) polymer aluminum decoupling capacitors.
- solder-pads for optional IR receiver. Vishay TSOP32xxx series.
- solder-pads for device tree blob EEPROM (which is not tested nowadays)
- modular connector (I like them) and 5.5/2.1mm jack. Cheap screw terminals would work as well.

### Filter Design & EMI ###
My expectations: My PCB design generates less high frequency distortion on the audio output lines than my old [Hifiberry Amp+]. Measurements with a digital scope on left and right audio lines show the following signals when driven without any audio signal.

![left channel][pibeanie-stereo15_left.png] ![right channel][pibeanie-stereo15_right.png]

The base frequency of 375kHz can't be filtered out completely but it is too high to be heard.
The red line is the calculated differential signal between + and - (or vice versa) of a single audio channel. Instead of a speaker a resistive load of 8.91 Ohm was connected.

In comparison: The signal of my Hifiberry Amp+ shows high frequency switching noise with the same measurement setup ... and + and - connections are reversed which doesn't really matter.

![left channel][hifiberry-amp+_left.png] ![right channel][hifiberry-amp+_right.png]


### Downsides ###
This design is limited by the maximum input voltage of 17V which is defined by the [TI TPS62133] DC/DC converter. So the maximum audio output power is limited to around 2x10 Watt @ 8 Ohm. Personally, I supply this audio amplifier with a 12V/2A power supply an get only ~7.5 Watt @ 8 ohm per channel. I can life with that audio power.
Nowadays I would take an different DC/DC converter. [TI TPS54302] wouldn't limit the supply voltage anymore but needs a much bigger, more expensive, choke.

Component prices could make this Raspberry Pi hat a little bit questionable. I got most parts as free engineering samples. Building this hat in low quantities does you make a poor man. The most expensive parts are the coils from Coilcraft. 4 x XAL1010-153ME does cost around 25$ at Mouser and, honestly, they are heavily oversized. Take a look at the BOM for a complete overview.

I've created a 2 layers PCB instead of 4 layers because 2 layers PCBs are much cheaper especially when ordered in China. I've ordered a bunch of PCBs and a stencil at [ShenZhen2U]. The parts were placed by hand and a cheap reflow oven has done the rest.
Well, with a 2-layer PCB heat dissipation becomes an issue when running at the power limit but my design showed a lower maximum temperature than the 4-Layer Hifiberry Amp+ PCB under similar conditions. In my opinion, no wonder. My copper surfaces are exposed to the air, on the Hifiberry Amp+ PCB you have to search the copper areas.


[Raspberry Pi]: https://www.raspberrypi.org/
[Raspberry Pi 7" display]: https://www.raspberrypi.org/blog/the-eagerly-awaited-raspberry-pi-display/
[HAT]: https://www.raspberrypi.org/blog/introducing-raspberry-pi-hats/
[Hifiberry Amp+]: https://www.hifiberry.com/products/ampplus/
[TAS5713]: http://www.ti.com/product/TAS5713/description
[TI TAS5713 datasheet]: http://www.ti.com/lit/ds/symlink/tas5713.pdf
[TI TAS5713 user guide]: http://www.ti.com/lit/pdf/slou281
[Vishay TSOP32xxx Series]: http://www.vishay.com/docs/82489/tsop322.pdf
[AO4803A]: http://www.aosmd.com/pdfs/datasheet/AO4803A.pdf
[TI TPS62133]: http://www.ti.com/product/TPS62133
[TI TPS54302]: http://www.ti.com/product/TPS54302
[ShenZhen2U]: http://www.shenzhen2u.com/

[pibeanie-stereo15.jpg]: https://github.com/kungpfui/pibeanie-stereo15/blob/master/images/pibeanie-stereo15.jpg "Pibeanie Stereo15"
[pibeanie-stereo15_left.png]: https://github.com/kungpfui/pibeanie-stereo15/blob/master/images/pibeanie-stereo15_left.png "Zero audio signal, left channel"
[pibeanie-stereo15_right.png]: https://github.com/kungpfui/pibeanie-stereo15/blob/master/images/pibeanie-stereo15_right.png "Zero audio signal, right channel"
[hifiberry-amp+_left.png]: https://github.com/kungpfui/pibeanie-stereo15/blob/master/images/hifiberry-amp+_left.png "Zero audio signal, left channel"
[hifiberry-amp+_right.png]: https://github.com/kungpfui/pibeanie-stereo15/blob/master/images/hifiberry-amp+_right.png "Zero audio signal, right channel"

