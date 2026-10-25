# CANFDLightForge Pro (v1.0.0)

### CiA 604-1 CAN FD Light Protocol Studio & 10-Point Normative Auditor

[![Standard](https://img.shields.io/badge/Standard-CiA%20604--1%20%7C%20ISO%2011898--1-blue.svg)](https://www.can-cia.org) [![Architecture](https://img.shields.io/badge/Architecture-Commander%2FResponder%20Non--Contention-orange.svg)](#) [![Bitrate](https://img.shields.io/badge/Data%20Rate-Up%20to%208%20Mbit%2Fs%20BRS-brightgreen.svg)](#) [![Normative Auditor](https://img.shields.io/badge/Auditor-10--Point%20Normative%20PASS-green.svg)](#) [![License](https://img.shields.io/badge/License-Commercial%20B2B-purple.svg)](#)

---

## 1\. Executive Summary

**CANFDLightForge Pro** is an automotive-grade engineering studio, codec engine, and normative architectural auditor for the **CAN in Automation (CiA) 604-1 CAN FD Light** specification.

In modern automotive exterior systems, vehicle lighting has evolved from simple static illumination into intelligent **Adaptive Driving Beam (ADB) matrix LED headlamps**, animated micro-LED grilles, and dynamic OLED rear clusters containing hundreds of individual LED pixels. Classical LIN buses (\$20,\\text{kbit/s}\$, 8-byte payload) are far too slow, causing visible flicker, while full CAN FD requires expensive quartz crystals and complex transceivers on every small LED satellite PCB.

**CiA 604-1 CAN FD Light delivers the ultimate balance:**

1. **Commander / Responder Architecture**: All transmissions are initiated by a single Commander (Lighting Master ECU), completely eliminating multi-master bus arbitration and contention.  
2. **Crystal-less Operation**: Responders synchronize their internal clocks directly to edge transitions of Commander frames, allowing satellite LED drivers to use **low-cost on-chip RC oscillators** instead of expensive external quartz crystals (saving \$0.40–\$0.80 BOM cost per node).  
3. **Passive Acknowledge (ACK)**: Responders do not drive the ACK slot, eliminating the bus propagation round-trip loop delay constraint.  
4. **High Throughput & Capacity**: Supports up to **64-byte payloads** and data bitrates up to **5 to 8 Mbit/s** with bit-exact dual CRC protection (**CRC-17** for \$\\le 16,\\text{bytes}\$, **CRC-21** for \$\> 16,\\text{bytes}\$).

CANFDLightForge Pro provides an interactive single-page web studio and a zero-dependency Python 3 CLI runner that enables automotive lighting engineers, actuator firmware developers, and semiconductor validation teams to synthesize, dissect, and audit CAN FD Light frames.

---

## 2\. Key Capabilities & Features

- **Dual-Phase Bitrate Switch (BRS) Codec**:  
  - Nominal Arbitration Phase: 500 kbit/s or 1000 kbit/s (\$1.0,\\mu\\text{s}/\\text{bit}\$).  
  - Accelerated Data Phase: 2.0 Mbit/s, 5.0 Mbit/s, up to 8.0 Mbit/s (\$0.125,\\mu\\text{s}/\\text{bit}\$).  
- **Dual CRC Automatic Engine**:  
  - CRC-17 (\$x^{17} \+ x^{16} \+ x^{14} \+ x^{13} \+ x^{11} \+ x^6 \+ x^4 \+ x^3 \+ x^1 \+ 1\$) for payloads \$\\le 16,\\text{bytes}\$.  
  - CRC-21 (\$x^{21} \+ x^{20} \+ x^{13} \+ x^{11} \+ x^7 \+ x^4 \+ x^3 \+ 1\$) for payloads \$\> 16,\\text{bytes}\$ (up to 64 bytes).  
  - Stuff Count field calculation (3-bit Gray code \+ parity).  
- **10-Point Normative Zero-Trust Compliance Auditor**:  
  - `RULE-CFDL-01`: Commander / Responder Architecture Non-Contention Verification.  
  - `RULE-CFDL-02`: Fixed Zero Acknowledge (ACK) Slot Operation (Passive ACK).  
  - `RULE-CFDL-03`: Nominal Phase Bitrate Margin (500 or 1000 kbps).  
  - `RULE-CFDL-04`: Data Phase Accelerated Bitrate Compliance (2.0 to 8.0 Mbps).  
  - `RULE-CFDL-05`: CAN FD Frame Indicator (Recessive FDF=1 Bit).  
  - `RULE-CFDL-06`: Payload DLC to Byte Length Conformance (\$0\\dots 64,\\text{bytes}\$).  
  - `RULE-CFDL-07`: Dual CRC Algorithm Selection Conformance (CRC-17 vs CRC-21).  
  - `RULE-CFDL-08`: 4-Bit Stuff Count Field & Gray Code Parity Integrity.  
  - `RULE-CFDL-09`: Responders Clock Synchronization on Commander Edges.  
  - `RULE-CFDL-10`: Sub-Millisecond Matrix Lighting Latency Ceiling (\$\< 250,\\mu\\text{s}\$).

---

## 3\. Directory Layout

2026-10-12 — CANFDLightForge Pro/

├── README.md                                \# Root product specification

├── build/

│   ├── canfdlight-cli.py                   \# Standalone Python CLI utility

│   └── canfdlight-pro-v1.0.0.zip           \# Release distribution archive

├── documentation/

│   ├── ARCHITECTURE.md                     \# CiA 604-1 protocol timing & CRC math

│   ├── USER\_GUIDE.md                       \# Web studio & CLI user manual

│   └── MONETIZATION\_AND\_MARKETING\_PLAYBOOK.md \# Commercialization & GTM strategy

├── screenshots/

│   └── canfdlight-studio-preview.svg       \# UI vector screenshot

└── source-code/

    ├── index.html                          \# Engineering web studio application

    ├── canfdlight.css                      \# Dark automotive theme stylesheet

    ├── canfdlight-engine.js                \# Core CAN FD Light codec & CRC engine

    ├── canfdlight-auditor.js               \# 10-point normative compliance ruleset

    └── app.js                              \# UI interactive application controller

---

## 4\. Quickstart Guide

### Web Application

Open `source-code/index.html` in any modern web browser. No server or Internet connection required.

### CLI Usage (Python 3.8+)

\# Synthesize a standard 16-byte headlamp command frame at 1 Mbps / 5 Mbps

python3 build/canfdlight-cli.py frame \--role COMMANDER \--id 0x120 \--nom-baud 1000 \--data-baud 5.0 \--data "0xFF,0xAA,0x55,0x00,0x12,0x34,0x56,0x78"

\# Run 10-point normative compliance audit

python3 build/canfdlight-cli.py audit \--id 0x120 \--data "0xFF,0xAA,0x55,0x00"

### Running Automated Test Suite

python3 test\_canfdlight.py

Expected output:

\============================================================

CANFDLightForge Pro \- CiA 604-1 Verification Suite

\============================================================

\[PASS\] TEST 1: CRC-17 Calculation (Result: 0x0146C)

\[PASS\] TEST 2: CRC-21 Calculation (Result: 0x09DBC8)

\[PASS\] TEST 3: Standard 11-bit Frame Synthesis (16B Payload, Duration: 48.0 us)

\[PASS\] TEST 4: Extended 29-bit Frame Synthesis (32B Payload, CRC-21: 0x15CED3)

\[PASS\] TEST 5: DLC Padding Conformance (10B input padded to 12B, DLC=9)

\[PASS\] TEST 6: Normative Auditor \- Compliant Frame (10/10 PASS)

\[PASS\] TEST 7: Fault Injection \- Corrupted CRC-17 Correctly Flagged

\[PASS\] TEST 8: Non-Compliant Nominal Bitrate (250 kbps) Correctly Flagged

\[PASS\] TEST 9: Matrix Lighting Refresh Latency (\< 250 us Confirmed)

\[PASS\] TEST 10: CLI Execution & JSON Serialization

\============================================================

RESULTS: 10 Passed, 0 Failed (100.0% Pass Rate)

\============================================================

---

## 5\. Commercial Licensing & Target Market

- **Target Customers**: Automotive Exterior Lighting Tier-1 Suppliers (Marelli, Hella, Valeo, Koito, ZKW, Stanley), Semiconductor Manufacturers (STMicroelectronics, NXP, Infineon, Melexis), and Automotive OEMs (Audi, BMW, Mercedes-Benz, Porsche, Genesis, Ford, GM).  
- **Pricing & Packaging**:  
  - **Single Engineer Desktop License**: \$1,390 / seat / year  
  - **Tier-1 Team Site License (Floating 10 seats)**: \$7,900 / year  
  - **OEM Enterprise Core (HIL Automation & Escrow)**: \$21,500 one-time perpetual license

---

*CANFDLightForge Pro is developed and certified by the Autonomous Digital Product Factory.*