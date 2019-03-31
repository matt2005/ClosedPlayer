== The Closed Player ==

This project - still in early development - is an effort to create a very easy to use mp3 player for kids, based on open source and readily available cheap components (in short: an ESP32, an RFID module, an SD-card reader, an amplifier and a speaker).

The key idea of the player is that tracks are selected simply by placing an RFID-tagged object on / next to the player, thus creating a physical key to a file, rather than having to navigate complex menus.

== Status ==

I've figured out the basic hardware setup. The software is still *far* from useful. But I'll go with publish early, publish often, rather than uploading a finished product without development history.

=== Predecessors and similar projects ===

My inspiration for this project came from the https://www.voss.earth/tonuino/ . Essentially, the Tonuino does pretty much the same thing as this project, but based on an Arduino Nano (or Uno, or Pro Mini), and a "DFplayer mini" module. The latter is a ready to use diy-friendly mp3-player module, sporting its own SD card slot and amplifier. This choice of integrated hardware make the Tonuino *very* easy to build, however it also introduces a few annoying limitations: The first is that we cannot freely access the SD card, which is a shame, because it means that cannot simply keep the configuration on the SD-card as text files. In lieu of this, the Tonuino sports an elaborate audio menu, which is flexible enough, but reminds me of a support hotline, uncomfortably. As a further minor nuisance, the DFplayer does not allow to seek in any way: We cannot even store and resume the position inside an audio track. Still, if you are looking for a really easy, really cheap DIY solution, and in particular, if you are already familiar with the classic Arduinos, I recommend the Tonuino whole-heartedly.

Without a doubt, there are both DIY and commercial precursors to the Tonuino. (One of?) the earliest commerical products seems to be the "Toniebox" (https://tonies.com/). But even before that, the year 2010 "Anybook reader" is/was based on a rather similar idea, and I would not be surprised to be pointed at yet earlier implementations. Anyway, using the Toniebox as a point of reference, this is clearly an extremely smooth and cute product, and it will be hard to achieve anything close in DIY, wiht a reasonable effort (who's talking about reaonable efforts, though). However, I am a tinkerer, and I'd like to explore some concepts, myself, such as replacing the (cute) "ear buttons" for volume control with a time-tested analog dial. I'd like further flexibility for creating custom tags (toy figures are cool, but they won't cut it, when it comes to managing four dozen episodes of your kid's favorite series). And most importantly, when a company expects me not only to download *their* content from the cloud, but also to require *me* to upload my custom content to their cloud, in order to get it onto the box, I'll just decline to play along, as a matter of principle.

=== Design objctives ===

Thus my design objectives:
- Easy and intuitive to use
- Buildable from cheap commonly available components
- Managable without network access (and optionally with local network access)
- Text-based / form-based configuration
- Tinker-friendly
- Low power consumptions and instant on
- Ability to resume anywhere in a track
- Ability for basic seek

=== Hardware ===

Since the integrated DFplayer mini does not quite meet my requirements, I was looking for a cheap MCU/board that could handle the mp3 decoding on its own. I'm not usually a big fan of Espressif, with their half-baked documentation, and add-riddled forums, but their ESP8266 and ESP32 are hard to argue with in terms of power / cost. While you will find examples of ESP8266-based mp3 players, those are sitting on the edge in terms of RAM usage, and on their single SPI bus, it seems hard to impossible to accomodate both an SD-card with low latency, and a slow RFID reader module. While a bit more expensive, the ESP32 can handle both extremely well, bringing loads of RAM, two SPI busses, and two separate cores. So the ESP32 will form the core of this project. Beyond that, we'll need an RFID reader. The MFRC522 is widely available for 1$ and below at the time of this writing, so that's what we will use. Next, we'll need an SD-card reader, again available for a few cents (make sure to use a 3.3volt variant). For the audio output, for simplicity we'll rely on the ESP32 to provide analog output (the alternative would be to use a cheap I2S-DAC; PT8211), and then an audio amplifier (PAM8403 is one that sells on ready to use boards for mere cents), and finally a speaker. In this project I'll also be using a potentiometer with switch for volume control and on/off, and two buttons for seeking. I'll encourage you to experiment with your own controls, though.

=== Wiring ===

ESP32 to RFID-RC522:
- 3.3V -> 3.3V
- GPIO4 -> RST
- Gnd -> Gnd
- N/C -> IRQ
- GPIO19 -> MISO
- GPIO23 -> MOSI
- GPIO18 -> SCK
- GPIO5 -> SDA

ESP32 to SD-Card:
- 3.3V -> 3.3V
- Gnd -> Gnd
- GPIO13 -> MISO
- GPIO14 -> CLK
- GPIO27 -> MOSI
- GPIO15 -> CS

ESP32 to Amp (PDM output via I2S):
- GPIO22 -> in

== The name "The Closed Player" ==

- Refers to the fact that I'd like my files nearby (close), rather than in the cloud
- Hints that no media will have to be inserted (for the most part)
- Hints, ironically, that this player is open source, not closed
- Is a reference to the late 1960's "Close N'Play", which was quit a nice shot at a kid-friendly audio player in its time.