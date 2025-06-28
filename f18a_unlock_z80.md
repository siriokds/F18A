# Unlocking

The F18A defaults to a "locked" mode of operation to prevent legacy software from accidentally enabling any of the enhanced features.  
I added the lock because during testing some ColecoVision games were causing strange behavior which I discovered was due to the software writing to VDP registers over register 7. Since the 9918A only has 8 registers (0 to 7), it did not matter, and the higher values  were simply masked to a number between 0 and 7.

But, the F18A supports VDP register values from 0 to 63 which is how you take advantage of the new features. This is also how the 9938/58 add additional features, and the datasheets for the 9918A indicates registers over 7 are reserved. However, that didn't stop some software from not following the rules, and on the 9918A/9928/9929 the bad behavior did not have any impact. But, the F18A has  to protect itself from that old software, thus it powers up locked.

Since the unlocking sequence has to be performed "in band", i.e. using the standard 9918A registers, I had to come up with a way that  would would never happen on the real 9918A. VR1 is probably the most critical VDP register since it contains most of the mode bits plus the memory size bit, thus it is VR1 in the form of VR57 (VR57 the same as VR1 on a non-F18A system) that is used to unlock the F18A.

Unlocking is done by writing $1C to VR57 twice, which on a real 9918A VDP is the same as writing to VR1. The value $1C was chosen because it sets the bits in VR1 to something you would never do on a real 9918A, even accidentally because it makes the VDP almost  useless. And to write such a value twice, consecutively, is hopefully beyond all probability of happening accidentally.

Value $1C in VR1 looks like this:

REG1:
|4/16K|BLANK| IE0 | M1  | M2  |  -  |SIZE | MAG |
|-----|-----|-----|-----|-----|-----|-----|-----|
|  0  |  0  |  0  |  1  |  1  |  1  |  0  |  0  |


By writing $1C on the real 9918A you are setting 4K VRAM, blank the screen, no interrupts, both M1 and M2 to '1' which is an illegal mode, and a '1' to the unused bit-5 that the datasheet indicates should always be '0'. This would pretty much make the real 9918A  useless, and any working software would never operate with this combination of bits in VR1.

Writing to VR57 (hex: $39, binary: 0011 1001) is VR1 on the 9918A which only sees the low 3-bits "001", and must be done twice in a row with no other CPU-to-VDP access. On the F18A you will be writing to VR57, not VR1, and after two consecutive writes the ERM (Enhanced  Register Mode) will be unlocked. Any further writes to VR57 ($39) after being unlocked will re-lock the F18A.

Because writing $1C to VR1 on the real 9918A would mess up the video mode and other critical VDP configuration, a write to VR1 should immediately follow the unlock sequence if you care to detect the F18A and write software that works on both the 9918A and  F18A.

Thus you would have something like:

```z80
; === Unlock sequence for F18A ===
di                  ; Interrupts must be off

; Write #1 to VR57 (0x1C value)
ld a, 0x1C
out (0xBF), a
ld a, 0xE9          ; 0x80 + 57 = 0xE9
out (0xBF), a

; Write #2 to VR57 (must be consecutive!)
ld a, 0x1C
out (0xBF), a
ld a, 0xE9
out (0xBF), a

; Write a sane value to VR1 to restore normal operation
ld a, 0xE0
out (0xBF), a
ld a, 0x81          ; 0x80 + 1 = 0x81
out (0xBF), a

ei                  ; Re-enable interrupts (optional)
```

