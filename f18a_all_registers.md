# F18A VDP Register Reference

This document describes all VDP registers for the TMS9918A-compatible F18A, including legacy VR0–VR7, extended VR8–VR63, and status registers SR0 and SR1.

---

## 📚 Index

- [VR0–VR7 (Standard Registers)](#vr0vr7-standard-registers)
- [VR8–VR23 (Reserved / Compatibility)](#vr8vr23-reserved--compatibility)
- [VR24 (Tile Palette Select)](#vr24-tile-palette-select)
- [VR30 (Enhanced Mode Register)](#vr30-enhanced-mode-register)
- [VR36 (ECM Select)](#vr36-ecm-select)
- [VR54–VR55 (GPU Program Counter)](#vr54vr55-gpu-program-counter)
- [VR57 (ERM Unlock Register)](#vr57-erm-unlock-register)
- [SR0 (Status Register 0)](#sr0-status-register-0)
- [SR1 (Status Register 1)](#sr1-status-register-1)

---

## VR0–VR7 (Standard Registers)

These are the original registers from the TMS9918A.

| Register | Bits        | Description                                      |
|----------|-------------|--------------------------------------------------|
| VR0      | 7–0         | Mode control, external video, M1                |
| VR1      | 7–0         | Interrupts, mode bits M2, memory size, blanking |
| VR2      | 7–0         | Name table base address                         |
| VR3      | 7–0         | Color table base address                        |
| VR4      | 7–0         | Pattern table base address                      |
| VR5      | 7–0         | Sprite attribute table address                  |
| VR6      | 7–0         | Sprite pattern table address                    |
| VR7      | 3–0         | Background color and foreground color           |

---

## VR8–VR23 (Reserved / Compatibility)

These registers are unused or reserved in the TMS9918A and F18A. They may be repurposed in future extensions but are not currently defined.

---

## VR24 (Tile Palette Select)

| Bit | 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0   |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|     | -   | -   | -   | -   | -   | -   | TPS1 | TPS0 |

- **TPS1:TPS0**: Selects which 16-color group to use in ECM0/ECM1.
- Default value: `00` (palette 0x00–0x0F)

---

## VR30 (Enhanced Mode Register)

| Bit | 7         | 6–0        |
|-----|-----------|------------|
|     | ECM Enable | Reserved  |

- **Bit 7**: Set to 1 to enable Enhanced Color Modes.
- **Bits 6–0**: Reserved (should be 0)

---

## VR36 (ECM Select)

| Bit | 7–3        | 2–0        |
|-----|------------|------------|
|     | Reserved   | ECM Mode # |

- **Bits 2–0**: Selects which ECM mode (0–5) to activate.
- **Bits 3–7**: Reserved.

---

## VR54–VR55 (GPU Program Counter)

| Register | Bit  | Description                      |
|----------|------|----------------------------------|
| VR54     | 7–0  | GPU PC MSB                       |
| VR55     | 7–0  | GPU PC LSB + **TRIGGERS GPU**    |

Writing both sets the PC and starts GPU execution.

---

## VR57 (ERM Unlock Register)

Special register used to unlock the Enhanced Register Mode.

- Write `0x1C` to VR57 **twice in a row** to unlock F18A.
- Any other write to VR57 after unlocking will **re-lock** the ERM.

---

## SR0 (Status Register 0)

| Bit  | 7     | 6        | 5        | 4         | 3     | 2     | 1       | 0       |
|------|-------|----------|----------|-----------|-------|-------|---------|---------|
| Name | 5th spr | Collision | Sprite overflow | VBlank | -     | -     | -       | -       |

- Standard 9918A status register.

---

## SR1 (Status Register 1) – F18A Identity Register

| Bit  | 7   | 6   | 5   | 4   | 3   | 2   | 1     | 0         |
|------|-----|-----|-----|-----|-----|-----|-------|-----------|
| Name | ID2 | ID1 | ID0 |  0  |  0  |  0  | blank | horz_int |

- **ID2–ID0** (bits 7–5): always `111` on F18A
- **Bits 4–2**: always 0
- **Bit 1**: vertical blanking
- **Bit 0**: horizontal interrupt
