# A500 Side Expansion Slot CPU Relocator with Flash based Kickstart
Forked from https://github.com/PR77/68000_Relocator_FLASH_Kickstart and has been modified to work as a relocator for Amiga 500's 'Side Expansion Port' (and potentially Amiga 1000).

# Warning
This design has not been completely tested yet. It may cause damage to your A500. I won't take any responsibility for any damage to any equipment that results from the use of this design and its components. IT IS ENTIRELY AT YOUR OWN RISK!

# Overview
This relocator will allow you to use a CPU-slotted accelerator, like the TF536, on your Amiga 500's 'Side Expansion Port' while the original 68000 CPU and Kickstart ROM still are socketed inside the Amiga.
The whole idea is to keep the Amiga 500 (and potentially Amiga 1000) in its complete and original state. So should you want to revert back to original mode you'll just have to disconnect/remove the 'Side Expenasion Slot CPU Relocator' (with accelerator), or by holding down CTRL-A-A for more than 1 second to use the motherboard Kickstart.
An optional socket can be utilized if you want to use an original Kickstart ROM (or a pre-programmed 27C400 EPROM) instead of the Flash EEPROMs (not tested).



### Appearance
3D model:

![3D Model](/Images/design.png)


IRL Rev.0:

![Real Deal Rev.0](/Images/irl_rev0.png)



### Pre-Requirements
This design will only work on a Rev.8A Amiga 500 (and A500+) out-of-the-box.

To get the Rev.5 to work, you'll have to modify it by soldering a wire from the "R103 via" to "Pin 7" on the 'Side Expansion Port' edge connector (see picture below). This is to add the 7MHz signal to the edge connector which is standard on Rev.8A boards.

![Solder Points Rev.5](/Images/solder_rev5.png)


To get the Rev.6 to work, you'll have to add a 0ohm or jumper wire on "JP6" (see picture below). This is to add the 7MHz signal to the edge connector which is standard on Rev.8A boards.

![Solder Points Rev.5](/Images/solder_rev6.png)



### BOM
For those wanting to build their own hardware, here is the BOM;

| Reference(s) | Value          | Footprint                                                      |
|--------------|----------------|----------------------------------------------------------------|
| C1           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C2           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C3           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C4           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C5           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C6           | 100nF          | Capacitors_SMD:C_0805_HandSoldering                            |
| C7           | 10uF           | Capacitors_SMD:C_1206_HandSoldering                            |
| C8           | 10uF           | Capacitors_SMD:C_1206_HandSoldering                            |
| J1           | Optional       | Pin_Headers:Pin_Header_Straight_1x10_Pitch2.54mm               |
| J2           | Reset          | Pin_Headers:Pin_Header_Straight_1x02_Pitch2.54mm               |
| J3           | Block          | Pin_Headers:Pin_Header_Straight_1x02_Pitch2.54mm               |
| J4           | JTAG           | Pin_Headers:Pin_Header_Straight_1x06_Pitch2.54mm               |
| J5           | ROM SELECT     | Pin_Headers:Pin_Header_Straight_1x03_Pitch2.54mm               |
| P1           | Side Exp. Slot | CONN EDGE DUAL FMALE 86POS 2.54mm                              |
| D2           | 30mA LED       | LED <color of your choice> CLEAR 0805 SMD                      |
| R1           | 470            | Resistors_SMD:R_0805_HandSoldering                             |
| R2           | 330            | Resistors_SMD:R_0805_HandSoldering                             |
| R3           | 10K            | Resistors_SMD:R_0805_HandSoldering                             |
| R4           | 10K            | Resistors_SMD:R_0805_HandSoldering                             |
| R5           | 10K            | Resistors_SMD:R_0805_HandSoldering                             |
| R6           | 10K            | Resistors_SMD:R_0805_HandSoldering                             |
| R7           | 10K            | Resistors_SMD:R_0805_HandSoldering                             |
| DIP64        | 68000D         | Housings_DIP:DIP-64_W22.86mm_Socket_LongPads                   |
| U1           | AT27C400       | Housings_DIP:DIP-40_W15.24mm_Socket                            |
| U2           | SST39SF040     | Housings_DIP:DIP-32_W15.24mm_Socket                            |
| U3           | SST39SF040     | Housings_DIP:DIP-32_W15.24mm_Socket                            |
| U4           | XC9572VQ44     | Housings_QFP:TQFP-44_10x10mm_Pitch0.8mm                        |
| U5           | LM1117-3.3     | TO_SOT_Packages_SMD:SOT-223-3_TabPin2                          |



### How It Works
A CPLD is used to switch between the Kickstart ROM on the Motherboard or the Flash Kickstart on the 'Side Expansion Slot CPU Relocator' board.
Switching is performed by an active /RESET (CTRL-A-A) without interruption for longer than 1 second. Shorter /RESET durations will simply just reset the Amiga.
After a POR (Power On Reset) the Flash EEPROMs on the CPU Relocator will be used by default.

There is a standard JTAG connector (J4) for programming the firmware to the Xilinx CPLD. The firmware can be found in the Logic section.



### Software
To program the Flash EEPROMs successfully, you'll need a valid Kickstart ROM. Either on the motherboard or in software form.
Software to inspect, dump, erase, program and save is provided in the Software section and looks like this:

![Flash Kickstart v1.0](/Images/flashkickstart.png)
(Screenshot taken from WinUAE and isn't reflecting information found on a real Amiga.)



#### Programming
Programming the EEPROMs can be done by using the 'FK' and/or 'FlashKickstart' programs located in the Software section.
With 'FlashKickstart' you'll just type the path and filename of your Kickstart ROM and click 'Program'.
Both programs work in WB1.2+/KS1.2+ environment.

![Programming the Flash EEPROMs](/Images/program.png)
(Screenshot taken from WinUAE and isn't reflecting information found on a real Amiga.)





### !!! IMPORTANT NOTE !!!
1 MB RAM is required to program the EEPROMs. This means either at least 1MB Chip or 1MB Slow RAM (512KB Chip + 512KB Slow RAM won't work).
Tips! If you have a KS2.05 or higher on your motherboard, you can use the relocator with an accelerator board to program the EEPROMs (if your accelerator board has more than 1MB+ RAM).




### Known Issues and Pending Items
This project is a work-in-progress:

1. Will update the software to not load the entire Kickstart content into RAM before programming; rather will load 64K chunks at a time (might fix issue 5 as well).
2. Support for ROMs based at address 0xF00000.
3. When using Kickstart 1.2 or 1.3 ROMs you'll not be able to program the EEPROMs when using a TF5xx accelerator board.
4. Programming the EEPROMs with Kickstart 1.2 or 1.3 may break compatibility. So use with caution!
5. Flashing tool requires 1MB "continuous" RAM (512KB Chip and 512KB Slow will not work). You'll need either 1MB+ Chip or 1MB+ Slow RAM.
