# Braids - Macro Oscillator for Eurorack
A clone of the Mutable Instruments Braids module with the use of the STM32F103 Blue Pill board.

## Module Build and PCBs
If you want to build the module yourself, I uploaded firmware, schematic, BOM and Gerber files for the PCB.

NOTE:
Firmware and schematic differ from the original design due to restrictions on the Blue Pill board.
More details are given below in section Firmware.

There are two different versions for the control board, an "original" and a "Thonk" version.
Reason is that for my own module, I am using specific potentiometers - 16K4 series from Supertech Electronics - and 3.5mm jack sockets - MJ-355 from Marushin - available at my local electronics shop.

However, since most DIY projects for Eurorack modules out there are using potentiometers from ALPHA and so-called THONKICONN jacks, as they are provided by Thonk in the UK, I also created a version with footprints for those components.
Choose the one you need.

The reason is simply that I did not find any stereo type of the THONKICONN jacks.

The layout of the main PCB is the same for both versions.

I created the Gerber files with the online tool EasyEDA and ordered it at JLCPCB.
I cannot guarantee, if this set of zipped Gerber files works also for other providers, like e.g. PCBWay. I have not tried that. But I saw online, that others did it.

If you want to know about my DIY building process, take a look at those two YouTube videos:
- [How I design PCBs for my Eurorack Synth Modules](https://youtu.be/pXtuV9Pv-m4)
- [Eurorack Module Synth - Building an Electric Druid Wavetable Oscillator Module](https://youtu.be/ECpdo4HfqLg)

## Panel Layout
I added the information about hole coordinates for the front panel in the folder PanelLayout, refering to the component layout in the Gerber files.

## Additional Information about specific Components
I could not get the DAC8551 chip from the original design, not in stock.
Instead, I tried the still available DAC8501, which turned out to work just fine, and which is also 16bit, like the original one.

Most of the components are through-hole, including the microchip part, thanks to the available Blue Pill board. However, there are a few components, which I did not find a THT version, or which I got used to use them frequently (like bypass caps for ICs and NPN transistors.
The SMD components are:
- DAC8501 (DAC, SOP8 package, the most challenging component due to its size)
- LM1117-3.3 (voltage regulator, SOT223 package)
- LM4040B10 (voltage regulator, SOT23 package)
- MMBT3904 (SMD version of the 2N3904 transistor, SOT23 Package)
- 0.1uF bypass caps for ICs (1608 package)
- 24.9K resistors (1608 package)

## Firmware
I shared the .hex files for the STM32F103 chip (bootloader and main) in the folder Firmware.
Those files and the schematic differ from the originals from Mutable Instruments.
Reason is the use of the Blue Pill board, as the chip's pins C13, C14, and C15 are already occupied on the Blue Pill board.
But the original design from Mutable Instruments is using them for the encoder.

Therefore, I changed the coding in three source code files, so that instead pins A0, A1, and A2 are used, which are not in use in the original design.
I got the idea for that from Nicolas Toussaint (SoundForce), who published [another Braids version on his website](https://sound-force.nl/?page_id=3179). That version is entirely through-hole, as it is using a different DAC.

In my version here, the following original source code files have been adjusted:
- encoder.cc (in subfolder /drivers)
- encoder.h (in subfolder /drivers)

If you want to compile your own .hex files, you need to search the texts in the below table and replace them in the entire file, as stated.
| File name | Original | Replace with |
| --- | --- | --- |
| encoder.cc | Pin_13 | Pin_2 |
| encoder.cc | Pin_14 | Pin_0 |
| encoder.cc | Pin_15 | Pin_1 |
| encoder.cc | gpioc | gpioa |
| encoder.cc | GPIOC | GPIOA |
| encoder.h | Pin_13 | Pin_2 |
| encoder.h | GPIOC | GPIOA |

## STM32F103 Version
CAUTION! There are three different versions of the Blue Pill board available.
The difference is the version of the ST32F103 microchip on the board.
The versions differ in the flash memory size:
- STM32F103C6: 32kB flash memory
- STM32F103C8: 64kB flash memory
- STM32F103CB: 128kB flash memory

The code size requires the 128kB version.
However, that version is currently (Feb2023) difficult to find, if available at all.

I gave it a try and bought the 64kB version.
Surprisingly, the programmer showed 128kB available flash memory, and the code could be loaded.
I tried it with several boards.
So it seems STM3F103C8 is ok for this module.

If you want to see more about the chip programming process, you can check out my YouTube video.

![Braids](https://user-images.githubusercontent.com/97026614/215266608-331e4a45-db72-4f7e-a5bb-2ee86c1e5bd3.jpeg)

![BraidsLED](https://user-images.githubusercontent.com/97026614/215266616-8cbeeec5-7710-44f6-bf8f-1bc07d317315.jpeg)

![BraidsFront](https://user-images.githubusercontent.com/97026614/215266621-14e591da-91f2-47aa-876c-8161228491b0.jpeg)

![BraidsSide](https://user-images.githubusercontent.com/97026614/215266645-07561741-37bf-43a4-b94f-cf6aa23f8489.jpeg)

![BraidsBack](https://user-images.githubusercontent.com/97026614/215266661-c8909050-4783-45e8-a165-4537c76e37b3.jpeg)

<img width="495" alt="MainPCBBack" src="https://user-images.githubusercontent.com/97026614/215266697-00501da3-c50c-4a52-aa7a-fea21a24c836.png">

<img width="495" alt="MainPCBFront" src="https://user-images.githubusercontent.com/97026614/215266708-05e1f6a8-e29c-43b1-80e6-da9165a71302.png">

<img width="500" alt="ThonkCtrlPCBFront" src="https://user-images.githubusercontent.com/97026614/215266719-0676cbe8-5f73-4d37-a62f-86d374514e44.png">

<img width="500" alt="ThonkCtrlPCBBack" src="https://user-images.githubusercontent.com/97026614/215266725-a34ad9aa-2cdd-441f-b46a-91d50e3914d7.png">
