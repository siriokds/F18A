


# Detection

To test for the F18A, I'm going to use Tursi's idea of using the GPU, which should make for a smaller test. Assuming the F18A unlock sequence has been performed, a small GPU program will be loaded to the VRAM and executed that will change 1 byte in VRAM. If the byte changed, the F18A is present, otherwise the system is running a stock VDP.  
The GPU is a slightly modified 9900 CPU so you can use any standard 9900 assembler to write code for the F18A's GPU. Since the GPU is inside the VDP it can only access the VRAM, plus an additional 2K of memory above the normal 16K of VRAM. The GPU's memory  map looks like this:

```
VRAM 14-bit, 16K   $0000 to $3FFF (0011 1111 1111 1111)  
GRAM 11-bit, 2K    $4000 to $47FF (0100 x111 1111 1111)  
PRAM 7-bit, 128    $5000 to $5x7F (0101 xxxx x111 1111)  
VREG 6-bit, 64     $6000 to $6x3F (0110 xxxx xx11 1111)  
current scanline   $7000 to $7xx0 (0111 xxxx xxxx xxx0)  
blanking           $7001 to $7xx1 (0111 xxxx xxxx xxx1)  
32-bit counter     $8000 to $8xx6 (1000 xxxx xxxx x110)  
32-bit rng         $9000 to $9xx6 (1001 xxxx xxxx x110)  
F18A version       $A000 to $Axxx (1010 xxxx xxxx xxxx)  
GPU status data    $B000 to $Bxxx (1011 xxxx xxxx xxxx)
```

"GRAM" means GPU-RAM.  
PRAM is the palette RAM in the F18A, and VREG is the VDP registers to which the GPU has full read/write access.


## GPU Test Program

The program will be loaded up high in VRAM. I like $3F00 for no particular reason, other than it is 256 bytes from the top of VRAM and probably unused.  
This is the code that will be loaded into VRAM for the GPU to execute:
```
0000 3F00 DEF MAIN
     AORG >3F00
MAIN
3F00 04E0 CLR @>3F00
3F02 3F00 
3F04 0340 IDLE
3F06 0000 END
```

That is a total of 6 bytes of assembly, which is pretty small for the test. The GPU will clear the word at $3F00, which in this case is the CLR instruction's opcode itself. You have to love self modifying code. :-) After the code runs, the value at VRAM $3F00 will be $00 if the F18A is present, otherwise it will be $04 on a stock VDP.
