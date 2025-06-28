| Reg/Bit |   7   |   6   |   5   |   4   |   3   |   2   |   1   |   0   |
|:-------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|    0    |   -   |   -   |   -   |   -   |   -   | ***M4***|   M3  |~~EXTVID~~ (0)|
|    1    |~~4/16K~~ (1)|  BL   | GINT  |  M1   |  M2   |   -   |   SI  |  MAG  |
|    2    |   -   |   -   |   -   |   -   |  PN13 |  PN12 |  PN11 |  PN10 |
|    3    | CT13  | CT12  | CT11  | CT10  |  CT9  |  CT8  |  CT7  |  CT6  |
|    4    |   -   |   -   |   -   |   -   |   -   |  PG13 |  PG12 |  PG11 |
|    5    |   -   | SA13  | SA12  | SA11  |  SA10 |  SA9  |  SA8  |  SA7  |
|    6    |   -   |   -   |   -   |   -   |   -   |  SG13 |  SG12 |  SG11 |
|    7    |  TC3  |  TC2  |  TC1  |  TC0  |  BD3  |  BD2  |  BD1  |  BD0  |

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


### Register $00 / VR0  – Mode Control 0

| Bit     |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0   |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---: |
| Function|  -  |  -  |  -  |  -  |  -  |  ***M4***  |  M3 |~~EXTVID~~ (0)|

#### 

    M4    	Select Graphics Mode 4 (Available on F18A and 
    M3    	Select Graphics Mode 3
    EXTVID  External Video enabled (0 = disabled, 1 = enabled). Not used.
    –       Unused / Reserved (should be 0)
    0       Fixed to 0 (do not modify)
###

Combining Reg0 and Reg1 the screen modes are selected:
| Mode |***M4***| M3 | M2 | M1 |
|:----:|:--:|:--:|:--:|:--:|
| GM1  | 0  | 0  | 0  | 0  |
| GM2  | 0  | 1  | 0  | 0  |
| MCM  | 0  | 0  | 1  | 0  |
| T40  | 0  | 0  | 0  | 1 (40 columns)|
| T80  |***1***  | 0  | 0  | 1 (80 columns)|


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $01 / VR1  – Mode Control 1

| Bit     | 7   | 6   | 5   | 4   | 3 | 2 | 1 | 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---: |
| Function|~~4/16K~~ (1)|  BL   | IE0  |  M1   |  M2   |   -   |   SIZE  |  MAG  |

#### 

    4/16K   VDP DRAM type mapping:
                           0 selects 4027 DRAM operation
                           1 selects 4108/4116 DRAM operation (***DEFAULT***, use this!)

    BL      Display Enable (0 = screen off, 1 = screen on)
    IE0     Enable Vertical Interrupt
    M1      Select Graphics Mode 1
    M2      Select Graphics Mode 2
    SIZE    Sprite size (0 = 8x8, 1 = 16x16)
    MAG     Sprite magnification (0 = 1X, 1 = 2X)
    –       Unused / Reserved (should be 0)
    0       Fixed to 0 (do not modify)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $02 / VR2  – Name Table Base Address

| Bit     | 7   | 6   |  5  |  4  | 3 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  PN (13 .. 10) |

#### 

    PN       Name Table Base Address. 
             PN9 to PN0 are fixed to 0 so, the address has 1K boundaries
             768-bytes per table for 24 rows
             960-bytes per table for 30 rows
             
    –        Unused / Reserved (should be 0)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

NAME TABLE TABLE ADDRESSING
Register 2 in the VDP contains the starting address for the Name Table sub-block.
R2* 400(16) = START ADDRESS
| R2 | ADDRESS |
|:--:|:-------:|
|00|0000
|01|0400
|02|0800
|03|0C00 - MAXIMUM NUMBER FOR 4K RAMS
|04|1000
|05|1400
|06|1800
|07|1C00
|08|2000
|09|2400
|0A|2800
|0B|2C00
|0C|3000
|0D|3400
|0E|3800
|0F|3C00 - MAXIMUM NUMBER

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $03 / VR3 – Color Table Base Address

| Bit     | 7 6 5 4 3 2 1 0 |
|:-------:|:---:|
| Function| CT (13 .. 6) |

#### 

    CT       Color Table Base Address. 
             CT5 to CT0 are fixed to 0 so, the address has 64 bytes boundaries
    
    –        Unused / Reserved (should be 0)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;


NAME TABLE TABLE ADDRESSING
Register 3 in the VDP contains the starting address for the Color Table sub-block.
R2* 64 = START ADDRESS
<pre>
| R3 | 00 | 01 | 02 | 03 | 04 | 05 | 06 | 07 | 08 | 09 | 0A | 0B | 0C | 0D | 0E | 0F |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 00 | 0000 | 0040 | 0080 | 00C0 | 0100 | 0140 | 0180 | 01C0 | 0200 | 0240 | 0280 | 02C0 | 0300 | 0340 | 0380 | 03C0 |
| 10 | 0400 | 0440 | 0480 | 04C0 | 0500 | 0540 | 0580 | 05C0 | 0600 | 0640 | 0680 | 06C0 | 0700 | 0740 | 0780 | 07C0 |
| 20 | 0800 | 0840 | 0880 | 08C0 | 0900 | 0940 | 0980 | 09C0 | 0A00 | 0A40 | 0A80 | 0AC0 | 0B00 | 0B40 | 0B80 | 0BC0 |
| 30 | 0C00 | 0C40 | 0C80 | 0CC0 | 0D00 | 0D40 | 0D80 | 0DC0 | 0E00 | 0E40 | 0E80 | 0EC0 | 0F00 | 0F40 | 0F80 | 0FC0 |
| 40 | 1000 | 1040 | 1080 | 10C0 | 1100 | 1140 | 1180 | 11C0 | 1200 | 1240 | 1280 | 12C0 | 1300 | 1340 | 1380 | 13C0 |
| 50 | 1400 | 1440 | 1480 | 14C0 | 1500 | 1540 | 1580 | 15C0 | 1600 | 1640 | 1680 | 16C0 | 1700 | 1740 | 1780 | 17C0 |
| 60 | 1800 | 1840 | 1880 | 18C0 | 1900 | 1940 | 1980 | 19C0 | 1A00 | 1A40 | 1A80 | 1AC0 | 1B00 | 1B40 | 1B80 | 1BC0 |
| 70 | 1C00 | 1C40 | 1C80 | 1CC0 | 1D00 | 1D40 | 1D80 | 1DC0 | 1E00 | 1E40 | 1E80 | 1EC0 | 1F00 | 1F40 | 1F80 | 1FC0 |
| 80 | 2000 | 2040 | 2080 | 20C0 | 2100 | 2140 | 2180 | 21C0 | 2200 | 2240 | 2280 | 22C0 | 2300 | 2340 | 2380 | 23C0 |
| 90 | 2400 | 2440 | 2480 | 24C0 | 2500 | 2540 | 2580 | 25C0 | 2600 | 2640 | 2680 | 26C0 | 2700 | 2740 | 2780 | 27C0 |
| A0 | 2800 | 2840 | 2880 | 28C0 | 2900 | 2940 | 2980 | 29C0 | 2A00 | 2A40 | 2A80 | 2AC0 | 2B00 | 2B40 | 2B80 | 2BC0 |
| B0 | 2C00 | 2C40 | 2C80 | 2CC0 | 2D00 | 2D40 | 2D80 | 2DC0 | 2E00 | 2E40 | 2E80 | 2EC0 | 2F00 | 2F40 | 2F80 | 2FC0 |
| C0 | 3000 | 3040 | 3080 | 30C0 | 3100 | 3140 | 3180 | 31C0 | 3200 | 3240 | 3280 | 32C0 | 3300 | 3340 | 3380 | 33C0 |
| D0 | 3400 | 3440 | 3480 | 34C0 | 3500 | 3540 | 3580 | 35C0 | 3600 | 3640 | 3680 | 36C0 | 3700 | 3740 | 3780 | 37C0 |
| E0 | 3800 | 3840 | 3880 | 38C0 | 3900 | 3940 | 3980 | 39C0 | 3A00 | 3A40 | 3A80 | 3AC0 | 3B00 | 3B40 | 3B80 | 3BC0 |
| F0 | 3C00 | 3C40 | 3C80 | 3CC0 | 3D00 | 3D40 | 3D80 | 3DC0 | 3E00 | 3E40 | 3E80 | 3EC0 | 3F00 | 3F40 | 3F80 | 3FC0 |
</pre>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;



### Register $04 / VR4  – Pattern Generator Table Base Address

| Bit     |  7  |  6  |  5  |  4  |  3  | 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  -  |PG (13 .. 11) |

#### 

    PG       Pattern Generator Table Base Address. 
             PG10 to PG0 are fixed to 0 so, the address has 2K boundaries
    
    –        Unused / Reserved (should be 0)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $05 / VR5  – Sprite Attribute Table Base Address

| Bit     | 7   | 6 5 4 3 2 1 0 |
|:-------:|:---:|:---:|
| Function|  -  |  SA (13 .. 7) |

#### 

    SA       Pattern Generator Table Base Address. 
             SA13 to SA7 are fixed to 0 so, the address has 128 bytes boundaries
    
    –        Unused / Reserved (should be 0)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $06 / VR6  – Sprite Generator Table Base Address

| Bit     |  7  |  6  |  5  |  4  |  3  | 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  -  |SG (13 .. 11) |

#### 

    SG       Sprite Generator Table Base Address. 
             PG10 to PG0 are fixed to 0 so, the address has 2K boundaries
    
    –        Unused / Reserved (should be 0)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

### Register $07 / VR7  – Text and Backdrop colors

| Bit     | 7 6 5 4  | 3 2 1 0 |
|:-------:|:---:|:---:|
| Function|  TC  | BD |

#### 

    TC       Text color. 

    BD       Backdrop/Border color.
    
