# Welcome to my take on the great KLOR Split Keyboard Project (That is Rev 1.4)

# CHANGELOG
09/08/2024
- Correction of a 0.45 mm interference between hot swap socket and bottom of haptic buzz module. The module has been moved upwards by 0.6 mm to clear the interference for those willing to install the stock haptic module.
- An alternative design for a DIY haptic module is included if you can get a DRV2605LDGSR driver chip and a ELV1411A linear motor. Hot air/plate soldering is required. This module is 0.8 mm shorter so there will definitely be no interference whatever version of PCB you have. I recommend a thinner PCB (1.2 mm) for this module

07/08/2024
-  Production validation of the new design. Full build completed and tested. Current Paste Layers are functionnal for those willing to order stencils.

25/07/2024
-  Initial commit with refactored PCB design for SK6812 Mini-E South facing ARGB LEDs

30/07/2024
-  Correction to SMD footprints for Paste Layers for diodes, reset switch and speaker.
-  ZMK Firmware modifications in order to be able to set per rotation pulses for each encoder. Both encoders work as expected. RGB light has been enabled by default as well.

--> If you intend to generate your own gerbers instead of the ones provided, make sure to process drill files AND flag the "Substract soldermask layer from copper pads" option (otherwise the KLOR Logo will superimpose on some of the SMD pads rendering soldering impossible without manual scratching of the covered pads). Guess how I know...

# FABRICATION NOTES
-  Compared with 1.3 design you only need different LED modules (SK6812 Mini-E instead of WS2812 3535)
-  Beware that Gateron hot swappable sockets must be oriented properly with their rounded edge against the central hole. If mounted upside down you won't be able to insert the switch. Kailh sockets don't have the same symetry in their shape so no risk to mount them upside down.
-  Be careful when selecting your hot swappable sockets as all brands have normal and low profile options with different pin placements. You can only use normal sockets on this design. If there is some interest I may produce a low profile south facing version eventually.
-  I recommend you stick with recommended diodes. Too small diodes can be a pain to orientate properly without proper magnification
-  Choose your MCU wisely: for wireless connectivity, Nice Nano V2 is fine and kind of the only option at this moment. But you'll loose some functionalities such as speaker, haptic feedback and less display used for logos. On the bright side I can confirm that both encoders do work flawlessly with recent versions of ZMK. When you wish to make the extra steps, ZMK firmware is a bit tricky to setup properly, but with some time you may be able to implement what you want. Keep in mind that the current official Firmware is based on an older ZMK codebase and some significant changes have happned recently, in terms of syntax, making it almost mandatory to do some changes in the original config and device tree files (for instance in order to have precise pulses per rotation for each encoder or to enable RGB)
-  For wired builds I would go with RP3040 Pro Micro boards as it seems very compatible with QMK and much more powerful compared with 32U4 MCU wich is a bit outdated now. QMK is very good IMHO apart from wireless options which is a real bummer...
-  The stock haptic feedback modules are extremly expensive at around 15 Euros each before shipping and are only offered by one single manufacturer. I have designed a DIY version of this module that should fit nicely. This module requires some SMD skills and equipment to be assembled. Parts values can be found on TI documentation for the driver chip itself. I will be using 2.2k pullup resistors and 1uF capacitors. Diode is optional and will serve as a reverse polarity protection. Same modcels as the rest of the keyboard could be used. Voltage drop should not be an issue as the driver chip will work with VCC ranging from 2 to 5.5V.
-  There are no real proper options for socketed MCU mount. So far the best option is to use machined round pins female headers with some sort of wire or salvaged through hole part leg to act as male pins. Complementary male round pins headers end up beeing a bit too high IMO. I am considering getting some 0.6 mm gold plated wire to act as male pins. Dedicated machined pins in the correct size are outrageously expensive.

# FIRMWARE
My version of ZMK firmware is located here:
https://github.com/Lefuneste83/zmk-config-klor
It has both encoders working, precise pulse per rotation adjustments and RGB enabled by default

## CONTEXT
I have just completed my build of the 1.3 and found it a bit frustrating to assemble, despite my 13 year experience with this kind of project. Don't get me wrong the KLOR project is absolutely marvelous, but the PCB design starts to show its age IMHO, in the details of the layout in particular. Technology wise it is one, if not the best project out there hands down.

So here you will find a completely refactored PCB design of the KLOR project for 2024 including :

- Much better suited and easier to solder ARGB modules. I have used SK6812 Mini-E (aka 6028 size LED) version as it is the typical choice for this kind of keyboard application. I have thus kept the original paradigm of through hole design, with a symetrical front/back layout, but while using a LED module which is designed for this application, contrary to the original model WS2812 3535 which required a direct "hanged" soldering, with very likely damages to the plastic package as well as missalignment issues, as this part is designed for direct surface mount. I personnaly think that it would be even better to use the new WS2812 2020 package which is very neat, but as this is a strictly SMD part, there is no need for discrete cutouts anymore, so the design would look a bit different. Also you need to master SMD soldering. Let me know if you are interested in this type of LED package, I could make a forked design without too much fuss.

- As Hipyo Tech can confirm, we are in 2024 so no North facing LED are allowed anymore. I have moved to South facing LED.

- I have tried to keep the overall design paradigm so all the good stuff is still there!!

- I have very extensively refactored the PCB layout (3 days of intense work) to clean up the various little issues that were sprinkled around the original design including :

  
  Rerouting of 90% of the PCB. Healthier and more rational overall routing, at least that's my belief...
  
  Change to the footprint of the solder jumpers to triangular shapes much easier to bridge
  
  Correction of missing ground connections on the various switches bodies
  
  Dual ground planes thorough validation (this was painful)
  
  Very slight changes to the board cutout on the right side, to deal with reversed switch placement. Also very slight reduction of the cutout under the top of the MCU to catter some track celearance that would pop up in DRC. This should have no impact as this cutout serves no real purpose as far as I know. Cases and switchboards should mount the same, except for the ones using the puck cutout (see below).
  
  Very slight movement of the puck mounting holes downward (98.57 mm instead of 96.4 mm) by a couple milimeters to catter for reversed switches pad postions. I could not do otherwise without revisiting the switch layouts. I could not solve this by rotating the 3 mounting holes without interfering with other nets. It should not affect the mounting ability of the puck itself, but it WILL interfere with existing back cases using the puck cutout. So we will need specific cutouts for the case. As I don't have access to the original step model the best option would be to use some alternate case design provided with source files in order to lower the cutout by the same amount (2.17 mm).
  
  I managed a 0 error JLCPCB DRC, with only very few voluntary exceptions (down from 1000+). It took me several hours to clean up the various little issues that were sprinkled all around the orignial design. And I can confirm that Kicad on Linux is not a pleasure to use...
  
  Change of Logo with mine !

  This repo will be updated as I have new ideas for integrations of various tech or correction for errors.

  I have also included files for a working 1.3 design which passes the JLCPCB fabrication and is working as a real world product (REV 1.3)
  


<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/docs/images/klor-font-logo-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="/docs/images/klor-font-logo-bright.svg">
  <img alt="KLOR logo font" src="/docs/images/klor-font-logo-bright.svg">
</picture>


# KLOR SPLIT KEYBOARD

KLOR is a 36-42 key column-staggered split keyboard. It supports a per-key RGB matrix, encoders, OLED displays, haptic feedback, speakers, a Pixart Paw3204 trackball, the [SplitKB tenting puck](https://splitkb.com/products/tenting-puck), and four different layouts, through break-off parts.

***

## RENDERING OF THIS FORK (Rev 1.4)

![KLOR layouts](/docs/images/Klor_1.4_picture6.jpg)
![KLOR layouts](/docs/images/Klor_1.4_picture4.png)
![KLOR layouts](/docs/images/Klor_1.4_picture5.png)
![KLOR layouts](/docs/images/Klor_1.4_picture1.png)
![KLOR layouts](/docs/images/Klor_1.4_picture2.png)
![KLOR layouts](/docs/images/Klor_1.4_picture3.png)

***

## LAYOUTS

![KLOR layouts](/docs/images/klor-layouts.svg)

***

## IMAGES

![KLOR polydactyl](/docs/images/KLOR_polydactyl_DES01.jpg)

![KLOR polydactyl](/docs/images/KLOR_polydactyl_DES02.jpg)

▲ KLOR polydactyl with 3DP case and DES keycaps.

***

![KLOR saegewerk](/docs/images/KLOR_saegewerk_GRID01.jpg)

![KLOR saegewerk](/docs/images/KLOR_saegewerk_GRID02.jpg)

▲ KLOR saegewerk with stacked acrylic case and GRID keycaps.

***

## PCB 

[Here](/PCB/) you can find the KiCad files and Gerbers for the KLOR 1.3 (tested working with JLCPCB) and 1.4 (this version). 

***

## CASE

[Here](/case/) are the case files for the KLOR. There is an acrylic case and a 3D printed case (which uses acrylic too) for every layout.

***

## SWITCHPLATE

In every case directory you'll find the matching switchplates as **.dxf**, **.svg**, **.stl**, and a **.zip**-file, containing gerber files for a FR4 plate.

***

## KNOB

You can use pretty much any knob, which works with the EC11 encoder, but [here](/knob/) you can find one, I designed for the KLOR.

***

## BUILD GUIDE

Here you can find the build guides for the KLOR.\
[KLOR build guide 3DP case](/docs/buildguide_3DP.md) (QMK)\
[KLOR build guide stacked acrylic case](/docs/buildguide_acrylic.md) (QMK)\
[KLOR BLE build guide 3DP case](/docs/buildguide_3DP_ble.md) (ZMK)\
[KLOR BLE build guide stacked acrylic case](/docs/buildguide_acrylic_ble.md) (ZMK)

***

## FIRMWARE

[QMK config](https://github.com/GEIGEIGEIST/qmk-config-klor) (supports OLED, encoders, LEDs, haptic feedback and speakers)\
[ZMK config](https://github.com/GEIGEIGEIST/zmk-config-klor) (supports OLED, encoder, LEDs and Bluetooth)\
[KMK config](https://github.com/moritz-john/kmk-config-klor) (supports OLED, encoders, LEDs and speakers) made by [Moritz John](https://github.com/moritz-john)\
Thanks to [Manna Harbour](https://github.com/manna-harbour) you can find KLOR in [Miryoku ZMK](https://github.com/manna-harbour/miryoku_zmk)

***

## CHOC/KS27 VERSION

[Sadek Baroudi](https://github.com/sadekbaroudi) did an amazing job in creating a version of the KLOR which supports KS-27 and Choc V1 switches. Including a special case for it.\
[KLOR Sadek KS-27 and Choc V1 mod](https://github.com/sadekbaroudi/KLOR)

***

## CREDITS

The KLORs hardware is based on the [Sofle](https://github.com/josefadamcik/SofleKeyboard) by [Josef Adamcik](https://github.com/josefadamcik).\
Other inspirations are 
- [Kyria](https://splitkb.com/products/kyria-pcb-kit) by [Thomas Baart](https://github.com/splitkb)
- [Yacc46](https://github.com/1m38/keyboards/tree/main/yacc46) by [1m38](https://github.com/1m38)
- [Elephant42](https://github.com/illness072/elephant42) by [illness072](https://github.com/illness072)
- [Torn](https://github.com/rtitmuss/torn) by [Richard Titmuss](https://github.com/rtitmuss)
- [Claw44](https://github.com/yfuku/claw44) by [yfuku](https://github.com/yfuku)

I used the Pimoroni Haptic Buzz Footprint from the [Buzzard](https://github.com/crehmann/Buzzard) by [Christoph Rehmann](https://github.com/crehmann).

***

People who helped me create this board and fix stuff
- [KarlK90](https://github.com/KarlK90)
- [MangoIV](https://github.com/MangoIV)
- [OxCB](https://github.com/0xCB-dev)
- [MicroFortnight](https://github.com/microfortnight)
- [dreipunkteinsvier](https://github.com/dreipunkteinsvier)


If you build a KLOR I would be pretty happy to see some pictures. And if you want to leave me a tip you can do this [here](https://ko-fi.com/geigeigeist) (but please don't feel pressured)
