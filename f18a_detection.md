


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

This is the code to load the program to VRAM. I'm including all the support routines here too so it is a complete program:

# F18A GPU Detection in Z80 Assembly

This code loads a small GPU program into VRAM at address `0x3F00`, triggers it by setting the GPU's program counter, and then checks the result by reading back the value at `0x3F00`. If the GPU executed the code (i.e., F18A is present), the byte at `0x3F00` will be zero.

```z80
; === F18A GPU Detection Routine in Z80 Assembly ===
; Assumes: F18A already unlocked

; === Copy GPU program to VRAM address 0x3F00 ===
; GPU program (6 bytes):
;   0x3F00: 0x04, 0xE0   ; CLR @>3F00
;   0x3F02: 0x3F, 0x00   ; address
;   0x3F04: 0x03, 0x40   ; IDLE

ld hl, gpu_code         ; Source in memory
ld de, 0x3F00           ; Destination in VRAM
ld bc, 6                ; Length
call write_block_vram   ; Copy to VRAM using VDP ports

; === Trigger GPU: set PC to 0x3F00 ===
; VR54 = 0x36 → 0x3F (MSB)
ld a, 0x3F
out (0xBF), a
ld a, 0xB6              ; 0x80 + 0x36
out (0xBF), a

; VR55 = 0x37 → 0x00 (LSB)
ld a, 0x00
out (0xBF), a
ld a, 0xB7              ; 0x80 + 0x37
out (0xBF), a

; === Read back value at 0x3F00 ===
ld hl, 0x3F00
call set_vdp_read_addr  ; Set VDP read address
in a, (0xBE)            ; Read byte from VDP
cp 0x00
jp z, f18a_present
jp f18a_absent

; === GPU program as byte array ===
gpu_code:
  .db 0x04, 0xE0, 0x3F, 0x00, 0x03, 0x40

; === Subroutine: set VDP read address ===
; HL = VRAM address
set_vdp_read_addr:
  ld a, l
  out (0xBF), a
  ld a, h
  and 0x3F
  out (0xBF), a
  ret

; === Subroutine: write block to VRAM ===
; HL = source, DE = dest VRAM, BC = length
write_block_vram:
  push af
  ld a, e
  out (0xBF), a
  ld a, d
  or 0x40
  out (0xBF), a
.copy_loop:
  ld a, (hl)
  out (0xBE), a
  inc hl
  dec bc
  ld a, b
  or c
  jp nz, .copy_loop
  pop af
  ret

; === Labels for branching ===
f18a_present:
  ; Detected F18A
  ; Add your code here
  ret

f18a_absent:
  ; Standard 9918A detected
  ; Add your fallback code here
  ret
```
