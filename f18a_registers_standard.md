| Reg/Bit |   7   |   6   |   5   |   4   |   3   |   2   |   1   |   0   |
|:-------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|    0    |   -   |   -   |   -   |   -   |   -   |   -   |   M3  |~~EXTVID~~ (0)|
|    1    |~~4/16K~~ (1)|  BL   | GINT  |  M1   |  M3   |   -   |   SI  |  MAG  |
|    2    |   -   |   -   |   -   |   -   |  PN13 |  PN12 |  PN11 |  PN10 |
|    3    | CT13  | CT12  | CT11  | CT10  |  CT9  |  CT8  |  CT7  |  CT6  |
|    4    |   -   |   -   |   -   |   -   |   -   |  PG13 |  PG12 |  PG11 |
|    5    |   -   | SA13  | SA12  | SA11  |  SA10 |  SA9  |  SA8  |  SA7  |
|    6    |   -   |   -   |   -   |   -   |   -   |  SG13 |  SG12 |  SG11 |
|    7    |  TC3  |  TC2  |  TC1  |  TC0  |  BD3  |  BD2  |  BD1  |  BD0  |


### Register 0 / VR0  – Mode Control 0

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
| Mode | M4 | M3 | M2 | M1 |
|:----:|:--:|:--:|:--:|:--:|
| GM1  | 0  | 0  | 0  | 0  |
| GM2  | 0  | 1  | 0  | 0  |
| MCM  | 0  | 0  | 1  | 0  |
| T40  | 0  | 0  | 0  | 1 (40 columns)|
| T80  |***1***  | 0  | 0  | 1 (80 columns)|

### Register 1 / VR1  – Mode Control 1

| Bit     | 7   | 6   | 5   | 4   | 3 | 2 | 1 | 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---: |
| Function|~~4/16K~~ (1)|  BL   | IE0  |  M1   |  M2   |   -   |   SIZE  |  MAG  |

#### 

    4/16K   VDP DRAM type mapping:
                           0 selects 4027 DRAM operation
                           1 selects 4108/4116 DRAM operation
            ***IMPORTANT*** 
            Default is 1.

    BL      Display Enable (0 = screen off, 1 = screen on)
    IE0     Enable Vertical Interrupt
    M1      Select Graphics Mode 1
    M2      Select Graphics Mode 2
    SIZE    Sprite size (0 = 8x8, 1 = 16x16)
    MAG     Sprite magnification (0 = 1X, 1 = 2X)
    –       Unused / Reserved (should be 0)
    0       Fixed to 0 (do not modify)


### Register 2 / VR2  – Name Table Base Address

| Bit     | 7   | 6   |  5  |  4  | 3 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  PN (13 .. 10) |

#### 

    PN       Name Table Base Address. 
             PN9 to PN0 are fixed to 0 so, the address has 1K boundaries
             768-bytes per table for 24 rows
             960-bytes per table for 30 rows
             
    –        Unused / Reserved (should be 0)


### Register 3 / VR3 – Color Table Base Address

| Bit     | 7 6 5 4 3 2 1 0 |
|:-------:|:---:|
| Function| CT (13 .. 6) |

#### 

    CT       Color Table Base Address. 
             CT5 to CT0 are fixed to 0 so, the address has 64 bytes boundaries
    
    –        Unused / Reserved (should be 0)


### Register 4 / VR4  – Pattern Generator Table Base Address

| Bit     |  7  |  6  |  5  |  4  |  3  | 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  -  |PG (13 .. 11) |

#### 

    PG       Pattern Generator Table Base Address. 
             PG10 to PG0 are fixed to 0 so, the address has 2K boundaries
    
    –        Unused / Reserved (should be 0)



### Register 5 / VR5  – Sprite Attribute Table Base Address

| Bit     | 7   | 6 5 4 3 2 1 0 |
|:-------:|:---:|:---:|
| Function|  -  |  SA (13 .. 7) |

#### 

    SA       Pattern Generator Table Base Address. 
             SA13 to SA7 are fixed to 0 so, the address has 128 bytes boundaries
    
    –        Unused / Reserved (should be 0)

### Register 6 / VR6  – Sprite Generator Table Base Address

| Bit     |  7  |  6  |  5  |  4  |  3  | 2 1 0 |
|:-------:|:---:|:---:|:---:|:---:|:---:|:---:|
| Function|  -  |  -  |  -  |  -  |  -  |SG (13 .. 11) |

#### 

    SG       Sprite Generator Table Base Address. 
             PG10 to PG0 are fixed to 0 so, the address has 2K boundaries
    
    –        Unused / Reserved (should be 0)

### Register 7 / VR7  – Text and Backdrop colors

| Bit     | 7 6 5 4  | 3 2 1 0 |
|:-------:|:---:|:---:|
| Function|  TC  | BD |

#### 

    TC       Text color. 

    BD       Backdrop/Border color.
    
