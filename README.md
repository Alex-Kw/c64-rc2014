# c64-rc2014 Keyboard Adapter

![gOofLT3](https://user-images.githubusercontent.com/20172602/211600565-83e920cd-84e0-4166-a966-7464574937f7.jpeg)
![9QHPLzM](https://user-images.githubusercontent.com/20172602/211600421-cd21f051-5454-4a4b-aee5-e41381c1cbc0.jpeg)

This is an Adaptor to use a Commodore 64 Keyboard with a RC2014 Serial input. It's intended to make using BASIC and CP/M from a physical terminal a reasonably good experience.

Features:

- SOFT CAPSLOCK: F1 and F2 toggle a soft CAPSLOCK mode. This was added because the SHIFT LOCK key is mechanical and affects the number row. The soft caps lock [which affects just alpha characters] is very useful for programming in BASIC in uppercase. The keyboard boots in mixed case, F1 enables this special CAPSLOCK mode, and F2 (SHIFT+F1) disables it.

Additional mappings and macros:

- Left arrow as ESC
- Up arrow mapped as ^
- RUN STOP mapped as Ctrl-C which will break out of BASIC loops.
- CLR/HOME mapped as Ctrl-L which will clear screen at CP/M Prompt
- F3 - macro to type 'CPM' and a carriage return
- F4 - Type escape char for qterm (ctrl-y). Doesn't display, press ? after for help.
- F5 - macro to type 'MBASIC' and a carriage return
- F7 - macro which types out a command to clear the screen in MBASIC, and a carriage return.
- F8 - macro to type 'OUT 0 , 0' and a carriage return (turn off digital output 0 in MBASIC) 

Booting an RC2014 with SCM and getting to MBASIC is then a matter of a few keypresses. Power on the system, press 'RESTORE' to reset it, and then pressing F3, F5, F7 in order will launch CPM, MBASIC, and clear the screen (on a system with Compact flash and the necessary software). These macros are in clear text strings and easily changed.

This project is currently using P-LAB Appledore PCB; A serial output header matching the RC2014 is present on Ver 1.1a of this PCB. Serial Baud is hard coded currently at 115200 but you can set it as you see fit in the arduino sketch if your system is running at a different clock speed or if you are using the dual clock module to adjust a secondary baud rate. This baud rate of 115200 assumes 'defaults' of the RC2014 Pro. 

PCB and original project/firmware is for using C64 keyboards on Apple1/Apple II and can be found here:
https://p-l4b.github.io/appledore/

If using the 1.0 version of the PCB, you could add a serial header like I did in prototyping. Photos of the modification to add a serial header:
https://i.imgur.com/wXjShyj.jpg

The above mod is not needed on the version 1.1a Appledore PCB. 

RESETTING THE RC2014, AND A NOTE ABOUT RESET USING RESTORE KEY: 
DON'T CONNECT A RESET WIRE UNLESS YOU KNOW WHAT YOU ARE DOING. When you connect the Appledore 1.1a to your RC2014, the Appledore will have reset on pin 6, but the RC2014 will not have anything on its side wired to that pin. To install a reset wire, connect the reset pin of the bus (pin 20 on a RC2014 Pro) to an unused pin on the serial header (pin 6) of the RC2014 PCB you use for serial input. This could be a Dual SIO or Pi terminal for example. On the Appledore, the reset pin goes to the reset output OF THE ASCII TTL header. If you are wiring things yourself, Do -not- connect the bus pin to the Nano's RST pin. You can also in concept simply connect the ASCII TTL RST pin on the Appledore PCB straight to pin 20 of the bus (RST) of your RC2014, and the RESTORE key on the C64 keyboard will pull the pin low to trigger a reset. Ver 1.1a of the Appledore PCB provides the correct RESET signal on pin 6 of the serial header, an unused pin on both the pi and dual sio modules as far as the schematics report. 

In the firmware, several negative values for special keys are converted to positive values. I left these in the comments so you may move things like the ctrl-c (RUN STOP) function around. RESTORE/RESET cannot be moved on the key map, it uses an electical circuit on the appledore, not code. Version 1.1 of the Appledore PCB has a reset button present on the actual PCB but I have not used that revision for an RC2014 yet (No reason to believe it would not work).

Full album of photos (prototype):
https://imgur.com/a/SZrAeKl

Photos of Finished 1.1a Ver:
https://imgur.com/a/DErxG2J

Video of functionality:
https://www.youtube.com/watch?v=GENigrzKh0U

Instructions:
Build version 1.1a of the AppleDore PCB and install the firmware from this repo. Keep in mind this intends for the Arduino NANO to get power from the RC2014 Serial port. On the Pi serial module +5 is present by default, on the dual SIO you must install a jumper to enable its presence. Specifically For the RC2014 Pro with a Pi serial module, reset functionality is added by a jumper wire on the Pi terminal card. Wire bus pin 20 to pin 6 of the serial header on the pi terminal PCB.  

Programming:
In the Arduino software simply load the sketch, select the correct port/device, and use Arduino as ISP. Nothing fancy, it should 'upload' rather quickly

Connecting:
You built a RC2014... Connect it to the serial bus using all 6 pins of the serial headers (in order).

hint: The NANO pins are misleadingly labelled, RX0 goes to RX not TX, at least on my Pi serial module. If you get no input from the keyboard after resetting your RC2014 try swapping RX/TX. 
