# 🖱️ Reverse Engineering Report — Zebronics Wired USB Optical Mouse

> **L&T EduTech Assignment | Reverse Engineering Activity**
> Department of Electronics & Telecommunication Engineering
> SMVIT, Bengaluru | Semester 6 | March 2026

---

## 📋 Project Overview

This repository contains the complete reverse engineering analysis of a **Zebronics Wired USB Optical Mouse**. The objective was to disassemble the device, identify all electronic components, analyse the circuit architecture, and document the working principle from physical input to USB HID output.

---

## 🖱️ Product Details

| Field | Details |
|---|---|
| **Product** | Zebronics Wired USB Optical Mouse |
| **Manufacturer** | Zebronics India Pvt. Ltd. |
| **Interface** | USB Type-A, Full-Speed (USB 1.1) |
| **Navigation** | Optical — LED + CMOS Image Sensor |
| **Primary IC** | ADNS-2803 (Avago Technologies) |
| **Resolution** | 800 CPI |
| **Buttons** | Left, Right, Middle + Scroll Wheel |
| **PCB Date Code** | 20151022 |
| **Supply** | 5V via USB VBUS |

---

## 🔩 Components Identified

| Ref | Component | Value | Function |
|---|---|---|---|
| U1 | ADNS-2803 IC | 20-DIP | Optical sensor + USB HID controller |
| C1 | Electrolytic Capacitor | 47µF / 6.3V | Bulk VCC decoupling |
| C4 | Electrolytic Capacitor | 4.7µF / 6.3V | Local HF decoupling at IC |
| R1 | Resistor | 68Ω | LED current limiting |
| D1 | Red LED | ~625nm, Vf ≈ 2.1V | Surface illumination |
| SW1 | Micro Switch | Omron-style | Left Button (LB) |
| SW2 | Micro Switch | Omron-style | Middle Button (MB) |
| SW3 | Micro Switch | Omron-style | Right Button (RB) |
| ENC1 | Scroll Encoder | Quadrature | Scroll wheel position |
| J1 | USB Cable | Type-A, 5-wire | Host interface |

---

## 🧠 Primary IC — ADNS-2803

The **ADNS-2803** by Avago Technologies is a complete single-chip USB optical mouse solution. It integrates:

- 18 × 18 pixel CMOS imaging array
- Internal LED driver circuit
- Digital Signal Processor (DSP) for motion detection
- Full-Speed USB 1.1 SIE (Serial Interface Engine)
- Hardware button debounce logic
- Scroll encoder decoder

### Key Specs

| Parameter | Value |
|---|---|
| Supply Voltage | 4.5V – 5.5V |
| Active Current | 70mA typ / 100mA max |
| Resolution | 800 CPI (fixed) |
| Max Tracking Speed | 16 inches/sec |
| Frame Rate | 1,500 fps |
| USB Standard | USB 1.1 Full-Speed (12 Mbps) |
| USB Class | HID — no driver required |
| Report Rate | 125 Hz (8ms polling) |

---

## ⚡ LED Current Calculation

```
If = (VLED − Vf) / R1
If = (3.3V − 2.1V) / 68Ω
If ≈ 17.6 mA  ✅ within ADNS-2803 LED drive spec (10–20mA)

Power in R1: P = I² × R = (0.0176)² × 68 ≈ 21mW  ✅ safe (rated 250mW)
```

---

## 🔄 How It Works — Signal Flow

```
USB VBUS (5V)
      │
      ▼
PCB VCC Rail ──── C1 (47µF) ──── C4 (4.7µF)
      │
      ▼
ADNS-2803 IC
      │
      ├── LED+ ──── R1 (68Ω) ──── Red LED ──── GND
      │                (illuminates surface)
      │
      ├── CMOS Sensor (18×18 px) captures surface images at 1500fps
      │
      ├── DSP computes ΔX, ΔY between frames
      │
      ├── LB / MB / RB ←── Micro Switches (active LOW)
      │
      ├── SCL / SDA ←── Scroll Encoder (quadrature)
      │
      └── D+ / D− ──── USB HID Report ──── Host PC
```

### USB HID Report Format (4 bytes, every 8ms)

| Byte | Field | Format |
|---|---|---|
| 0 | Buttons | Bit0=LB, Bit1=RB, Bit2=MB |
| 1 | ΔX | Signed 8-bit |
| 2 | ΔY | Signed 8-bit |
| 3 | Scroll | Signed 8-bit |

---

## 📁 Repository Contents

```
├── README.md                          ← This file
└── Zebronics_Mouse_RE_Report_LT.docx ← Full reverse engineering report (L&T EduTech format)
```

---

## 📸 PCB Images

| View | Description |
|---|---|
| Image 1 | Bottom lens PCB (green) — LED assembly glowing |
| Image 2 | Main PCB (orange) — ADNS-2803, capacitors, switches visible |
| Image 3 | Side view — scroll encoder and switch assembly |
| Image 4 | Top view — LB, MB, RB micro-switch layout |

---

## 📚 References

1. Avago Technologies — [ADNS-2803 Datasheet](https://www.avagotech.com/docs/AV02-0202EN)
2. Omron — D2FC-F-7N Micro Switch Datasheet
3. USB Implementers Forum — [USB 1.1 Specification](https://www.usb.org/documents)
4. USB Implementers Forum — [HID Usage Tables v1.12](https://www.usb.org/hid)
5. Zebronics India — [Product Portfolio](https://www.zebronics.com)
6. Texas Instruments — Decoupling Capacitor Selection Guidelines (SZZA009)

---

## 👥 Team Details

| Field | |
|---|---|
| **Group Name** | *adwal* |
| **Members** | *6* |
| **Semester** | 6 — Electronics & Telecommunication |
| **Department** | Electronics & Telecommunication Engineering |
| **College** | SMVIT, Bengaluru |
| **Submitted To** | L&T EduTech |

---

> *Report submitted as part of the L&T EduTech Reverse Engineering Activity. GitHub link to be shared with L&T EduTech as per submission instructions.*
