# InkTime — Open Source E-Paper Smartwatch

> Cheap, open-source smartwatch built around the Nordic nRF52840 SoC and an e-paper display.

---

## Block Diagram

```
                          ┌─────────────────────────────────────────────┐
                          │              nRF52840 (U$1)                  │
                          │                                              │
         ┌────────────┐   │  SPI (P0.02/P0.03/P0.05/P0.15/P0.16/P0.17) │
         │  E-Paper   │◄──┤  EPD power ctrl (P1.01)                    │
         │  Display   │   │                                              │
         │  (J2 FPC)  │   │  I2C (P0.06 SDA / P0.07 SCL)               │
         └────────────┘   │   ├─► BQ25180  (IC1)  Battery Charger       │
                          │   ├─► BMA423   (IC2)  Accelerometer/IMU     │
         ┌────────────┐   │   ├─► DRV2605  (IC3)  Haptic Driver         │
         │  Battery   │──►│   └─► MAX17048 (U1)   Fuel Gauge            │
         │  (LiPo)    │   │                                              │
         └────────────┘   │  GPIO                                        │
                          │   ├─► HAPTIC_EN  (P0.12)                    │
         ┌────────────┐   │   ├─► IMU_INT1   (P0.08)                    │
         │  Buttons   │──►│   ├─► IMU_INT2   (P1.08)                    │
         │  UP/ENT/DN │   │   ├─► PMIC_INT   (P0.11)                    │
         └────────────┘   │   ├─► SW_UP      (P0.13)                    │
                          │   ├─► SW_ENT     (P0.14)                    │
         ┌────────────┐   │   └─► SW_DN      (P1.02)                    │
         │ RT6160AWSC │◄──┤  I2C (shared bus)                           │
         │ Buck-Boost │   │                                              │
         │   (IC9)    │   │  SWD debug (SWDIO / SWDCLK / SWO)          │
         └────────────┘   │  32MHz XTAL (X1)  /  32.768kHz XTAL (X2)  │
                          │  2.4GHz Antenna (ANT1)                      │
         ┌────────────┐   └─────────────────────────────────────────────┘
         │  USB-C     │
         │  (J3)      │──► BQ25180 (charging)
         └────────────┘──► USBLC6-2SC6Y (ESD protection, D1)
```

---

## Bill of Materials (BOM)

| Ref | Qty | Component | Description | Package | JLCPCB / Procurement | Datasheet |
|-----|-----|-----------|-------------|---------|----------------------|-----------|
| U$1 | 1 | nRF52840-QIAA | Main MCU — BLE 5.0 + ARM Cortex-M4F | AQFN-73 (7×7mm) | [JLCPCB](https://jlcpcb.com/partdetail/NordicSemiconductor-nRF52840_QIAA/C190794) | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.5.pdf) |
| IC1 | 1 | BQ25180YBGR | 1-cell LiPo charger, I2C, 1A | 8-DSBGA (1.6×1.1mm) | [Mouser](https://www.mouser.co.uk/ProductDetail/Texas-Instruments/BQ25180YBGR) | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) |
| IC2 | 1 | BMA423 | Triaxial accelerometer + step counter, I2C/SPI | LGA-12 (2×2mm) | [Mouser](https://www.mouser.co.uk/ProductDetail/Bosch-Sensortec/BMA423) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma423-ds000.pdf) |
| IC3 | 1 | DRV2605YZFR | Haptic driver for ERM/LRA motors, I2C | 9-BGA (1.44×1.44mm) | [Arrow](https://www.arrow.com/en/products/drv2605yzfr/texas-instruments) | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) |
| U1 | 1 | MAX17048G+T10 | 1-cell fuel gauge, ModelGauge, I2C | TDFN-8 (2×2mm) | [SnapEDA/Mouser](https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/) | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf) |
| IC9 | 1 | RT6160AWSC | Buck-boost DC/DC regulator, I2C | 15-WL-CSP (1.4×2.3mm) | [Mouser](https://www.mouser.co.uk/ProductDetail/Richtek/RT6160AWSC) | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf) |
| ANT1 | 1 | 2450AT18B100E | 2.45 GHz chip antenna | 3216 SMD | [Mouser](https://www.mouser.co.uk/ProductDetail/Johanson-Technology/2450AT18B100E) | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E.pdf) |
| J2 | 1 | Molex 503480-2400 | 0.5mm FPC connector, 24-pin, right-angle | SMD | [Mouser](https://www.mouser.co.uk/ProductDetail/Molex/503480-2400) | [Datasheet](https://www.molex.com/en-us/products/part-detail/5034802400) |
| J3 | 1 | KH-TYPE-C-16P | USB-C 16-pin receptacle | SMD | [JLCPCB](https://jlcpcb.com/partdetail/Kinghelm-KH_TYPE_C_16P/C2765186) | [Datasheet](https://www.kinghelm.net/product/kh-type-c-16p.html) |
| J1 | 1 | TC2030-IDC | Tag-Connect 6-pin SWD debug connector | PTH | [Tag-Connect](https://www.tag-connect.com/product/tc2030-idc) | [Datasheet](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-IDC_1.pdf) |
| X1 | 1 | 32 MHz Crystal | Main RF crystal | 2016 SMD | JLCPCB parts | Nordic ref design |
| X2 | 1 | 32.768 kHz Crystal | RTC crystal | 3215 SMD | JLCPCB parts | Nordic ref design |
| L1 | 1 | Wurth 744043680 (68 µH) | EPD boost inductor | 4828 SMD | [JLCPCB](https://jlcpcb.com/partdetail/WurthElektronik-744043680/C408334) | [Datasheet](https://www.we-online.com/catalog/datasheet/744043680.pdf) |
| L2 | 1 | 10 µH inductor | DC-DC inductor for nRF DCC pin | 0201 | JLCPCB parts | — |
| L3 | 1 | 15 nH inductor | RF impedance matching network | 0201 | JLCPCB parts | — |
| L7 | 1 | FTC252012SR47MBCA (0.47 µH) | Buck-boost inductor | 2016 SMD | [JLCPCB](https://jlcpcb.com/partdetail/6763488-FTC252012SR47MBCA/C5832368) | [Datasheet](https://product.tdk.com/en/search/inductor/inductor/smd/info?part_no=FTC252012SR47MBCA) |
| D1 | 1 | USBLC6-2SC6Y | USB ESD protection TVS diode array | SOT-23-6 | JLCPCB parts | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2sc6y.pdf) |
| D2, D4, D5 | 3 | MBR0530 | Schottky diode 30V/0.5A | SOD-123 | JLCPCB parts | [Datasheet](https://www.onsemi.com/pdf/datasheet/mbr0530t1-d.pdf) |
| Q1 | 1 | P-channel MOSFET | EPD 3.3V power switch | SOT-23 | JLCPCB parts | — |
| Q3 | 1 | SI1308EDL-T1-GE3 | N-channel MOSFET 30V/1.5A, EPD gate driver | SC-70 | JLCPCB parts | [Datasheet](https://www.vishay.com/docs/63401/si1308edl.pdf) |
| SW_UP, SW_ENT, SW_DN | 3 | EVP-AKE31A | Tactile push button, SMD | SMD | JLCPCB parts | [Datasheet](https://industrial.panasonic.com/ww/products/pt/light-touch-switches/models/EVPAKE31A) |
| R1–R18, various | ~20 | Resistors | Various values (10K, 5K1, 3K3, 7.68K, 2.2Ω, 0.47Ω) | 0201 | JLCPCB parts | — |
| C1–C43, EPD_Cx | ~60 | Capacitors | 1pF – 22µF decoupling/filtering | 0201 (≤100nF), 0402 (>100nF) | JLCPCB parts | — |
| TP_* | 14 | Test pads | GND, 3V3, VBAT, SDA, SCL, SWDIO, SWDCLK, SWO, RESET, etc. | TP20R | — | — |
| SJ1 | 1 | Solder jumper | Configuration jumper | — | — | — |

---

## Hardware Functionality

### Microcontroller — nRF52840

The nRF52840 is an ARM Cortex-M4F SoC from Nordic Semiconductor operating at 64 MHz with 1 MB Flash and 256 KB RAM. It integrates a Bluetooth 5.0 radio (BLE + 802.15.4), hardware AES/ECB/CCM/AAR, and a USB 2.0 full-speed controller.

Two crystals are required:
- **32 MHz** (X1) connected to XC1/XC2 pins — main clock for the RF subsystem.
- **32.768 kHz** (X2) connected to XL1/XL2 pins — low-power RTC clock for sleep mode timekeeping.

The DCC pin is filtered with a 10 µH inductor (L2) and decoupling capacitors as recommended in the Nordic hardware guidelines.

---

### E-Paper Display — SPI

The e-paper display connects via a **24-pin 0.5 mm FPC** connector (J2, Molex 503480-2400). It is driven over **SPI** with additional GPIO control lines.

The display power rail (EPD_3V3) is generated by a boost converter built around inductor L1 (68 µH) and switched by Q1 (P-channel MOSFET), controlled by the nRF pin P1.01. The EPD gate driver circuit uses Q3 (SI1308EDL N-MOSFET) and Schottky diodes D2/D4/D5 (MBR0530) to generate the positive/negative gate voltages required by the e-paper panel.

A capacitor bank (EPD_C1 through EPD_C12, all 1 µF/50 V, 0402) buffers the gate voltage generation transients. A type-selection resistor (R_TYPE_SEL, 2.2 Ω) sets the display panel type via J2 pin 6.

**Communication interface:** SPI (4-wire + DC + RST + BUSY)

---

### Battery Charger — BQ25180YBGR (IC1)

The BQ25180 is a compact single-cell Li-Ion/LiPo charger from Texas Instruments in an 8-pin DSBGA package. It is configured and monitored over **I2C**.

Key features used:
- Charge current set by R1 (10 kΩ on TS/MR pin).
- SYS output (VREG net) powers the RT6160 buck-boost regulator and other system rails.
- INT line (PMIC_INT) alerts the nRF on charge state changes.
- The battery connects directly to two test pads (TP_VBAT, TP_BAT_GND) without a JST connector to save space.

Decoupling: C24 (10 µF) and C23 (100 nF) on VREG; C39 (10 µF) on the SYS output.

---

### Fuel Gauge — MAX17048G+T10 (U1)

The MAX17048 is a 1-cell ModelGauge fuel gauge communicating over **I2C** (shared bus with BQ25180 and DRV2605). It directly measures battery voltage and estimates state-of-charge without a sense resistor. VBAT connects directly to the CELL pin.

---

### Buck-Boost Regulator — RT6160AWSC (IC9)

The RT6160A is a 15-pin WL-CSP I2C-programmable buck-boost DC/DC converter that generates a stable 3.3 V output (3V3 rail) from the variable battery voltage (3.0–4.2 V). It uses inductor L7 (0.47 µH). Input decoupling: C25 (22 µF) + C33 (22 µF). Output decoupling: C21 (4.7 µF) + C20 (4.7 µF).

The regulator is enabled by the BQ25180 SYS pin (VREG net) and configured over the shared I2C bus.

---

### Accelerometer / IMU — BMA423 (IC2)

The BMA423 is a Bosch 12-bit 3-axis accelerometer in a 2×2 mm LGA package with an integrated step counter, tilt, and activity recognition engine.

It communicates over **I2C** (CSB tied to VDD to select I2C mode). Two interrupt outputs (INT1 → P0.08, INT2 → P1.08) allow the nRF to wake from sleep on motion events. SDO pin sets the I2C address (pulled to GND via R3 → address 0x18).

Decoupling: C37 (1 µF) on VDDIO; C38 (1 µF) on VDD.

---

### Haptic Driver — DRV2605YZFR (IC3)

The DRV2605 drives an ERM or LRA vibration motor and includes an internal waveform library. It is controlled over **I2C** with an additional enable line (HAPTIC_EN → P0.12). The motor output pins are broken out to test pads TP_OP and TP_ON for debugging. A 100 nF decoupling capacitor (C34) is placed on VDD.

---

### USB Interface — USB-C (J3) + ESD (D1)

The USB-C connector (KH-TYPE-C-16P) carries VBUS for charging and USB D+/D- for the nRF's built-in USB 2.0 FS peripheral. CC1 and CC2 lines have 5.1 kΩ pull-down resistors (R1_USB, R2_USB) to signal a default USB power sink.

ESD protection is provided by USBLC6-2SC6Y (D1) on D+/D−. Bulk decoupling on VBUS: C42 (1500 pF) + C43 (1500 pF).

---

### Debug Interface — TC2030-IDC (J1)

A Tag-Connect TC2030-IDC footprint provides SWD (SWDIO, SWDCLK) + SWO + RESET access without needing a permanent through-hole connector. All signals are also available on labeled test pads.

---

### Push Buttons

Three Panasonic EVP-AKE31A tactile SMD buttons (SW_UP, SW_ENT, SW_DN) are connected with RC debounce filters (10 kΩ pull-up + 1 µF capacitor on each signal line).

---

### RF Section — 2.4 GHz Antenna

The Johanson 2450AT18B100E chip antenna is placed at the board edge with a PCB cutout underneath (no copper, no routing under the antenna area). The matching network consists of L3 (15 nH series) and shunt components (C3: 1 pF, C4: 1 pF, C9: 820 pF, C11: 100 pF) per the Nordic nRF52840 reference design for a chip antenna.

---

## nRF52840 Pin Assignment

| nRF52840 Pin | Net / Signal | Connected To | Interface |
|---|---|---|---|
| P0.00 / XL1 | XL1 | X2 pin 2 (32.768 kHz crystal) | LFXO |
| P0.01 / XL2 | XL2 | X2 pin 1 (32.768 kHz crystal) | LFXO |
| P0.02 / AIN0 | SCK | J2 pin 13 → EPD display | SPI clock |
| P0.03 / AIN1 | MOSI | J2 pin 14 → EPD display | SPI MOSI |
| P0.05 / AIN3 | EPD_CS | J2 pin 12 → EPD chip select | SPI CS |
| P0.06 | SDA | IC1 (BQ25180), IC2 (BMA423), IC3 (DRV2605), U1 (MAX17048) | I2C data |
| P0.07 | SCL | IC1 (BQ25180), IC2 (BMA423), IC3 (DRV2605), U1 (MAX17048) | I2C clock |
| P0.08 | IMU_INT1 | IC2 (BMA423) INT1 | GPIO input |
| P0.11 | PMIC_INT | IC1 (BQ25180) !INT | GPIO input |
| P0.12 | HAPTIC_EN | IC3 (DRV2605) EN | GPIO output |
| P0.13 | SW_UP | SW_UP button | GPIO input |
| P0.14 | SW_ENT | SW_ENT button | GPIO input |
| P0.15 | EPD_DC | J2 pin 11 → EPD data/command | GPIO output |
| P0.16 | EPD_RST | J2 pin 10 → EPD reset | GPIO output |
| P0.17 | EPD_BUSY | J2 pin 9 → EPD busy flag | GPIO input |
| P0.18 / RESET | RESET | J1 pin 6, TP_RESET | System reset |
| P1.00 | SWO | J1, TP_SWO | SWD trace output |
| P1.01 | EPD power ctrl | Q1 gate via R_PWR_EPD | GPIO output (EPD rail enable) |
| P1.02 | SW_DN | SW_DN button | GPIO input |
| P1.08 | IMU_INT2 | IC2 (BMA423) INT2 | GPIO input |
| SWDCLK | SWDCLK | J1 pin 4, TP_SWDCLK | SWD debug |
| SWDIO | SWDIO | J1 pin 2, TP_SWDIO | SWD debug |
| DCC | DC/DC | L2 (10 µH) | Internal DC/DC choke |
| XC1 / XC2 | RF crystal | X1 (32 MHz) | HFXO |
| ANT | RF | L3 → ANT1 (2450AT18B100E) | 2.4 GHz RF |
| VDD | 3V3 | RT6160AWSC output (3.3 V) | Power |
| VDDH | 3V3 | Same 3.3 V rail | Power (high-voltage I/O) |
| DEC1–DEC6, DEC3V3 | Decoupling | Various 100 nF / 12 pF caps | Power decoupling |

> **Pin selection rationale:**
> - **P0.02/P0.03** were chosen for SCK/MOSI because they are fully flexible GPIO with no alternate function conflicts. The nRF52840 SPI master (SPIM) can be mapped to any GPIO.
> - **P0.06/P0.07** are used for I2C because they sit close to the BQ25180 and BMA423 physical placement area on the PCB, minimising trace length for the 400 kHz Fast-mode I2C bus.
> - **P0.00/P0.01** are the dedicated crystal pins for the 32.768 kHz LFXO and cannot be repurposed.
> - Buttons (P0.13, P0.14, P1.02) have hardware RC debounce circuits so any GPIO works; these were chosen as free pins after all peripherals were assigned.
> - **P1.01** drives the EPD power MOSFET through a resistor divider — any output-capable GPIO suffices.

---

## Power Budget (Estimated)

| Mode | nRF52840 | EPD refresh | BMA423 | DRV2605 | Total (est.) |
|---|---|---|---|---|---|
| Active (BLE advertising) | ~15 mA | 0 mA | 0.13 mA | 0 mA | ~15 mA |
| EPD refresh | 3 mA | ~30 mA peak | 0.13 mA | 0 mA | ~33 mA |
| Haptic feedback | 5 mA | 0 mA | 0 mA | ~60 mA | ~65 mA |
| Deep sleep (System OFF) | 2.5 µA | 0 mA | 0 µA | 0 µA | ~3 µA |

With a 180 mAh LiPo battery and a typical duty cycle (BLE advertising every 1 s, time display refreshed once per minute), estimated battery life is **5–7 days**.

---

## Test Pads

All test pads are clearly marked in the silkscreen layer with the signal name:

| Pad | Signal |
|---|---|
| TP_3V3 / TP_3.3V | 3.3 V regulated rail |
| TP_VBAT | Raw battery voltage |
| TP_BAT_GND | Battery ground |
| TP_GND | System ground |
| TP_SDA | I2C data |
| TP_SCL | I2C clock |
| TP_SWDIO | SWD data |
| TP_SWDCLK | SWD clock |
| TP_SWO | SWD trace |
| TP_RESET | System reset |
| TP_ON / TP_OP | Haptic motor − / + |
| TP_VREG | BQ25180 SYS output |

---

## Design Notes & Decisions

- **PCB thickness is 1 mm** (not standard 1.6 mm) to fit within the mechanical constraints of the case.
- **All components are placed on the TOP layer only**, as required by the project specification.
- **Ground planes** are present on both TOP and BOTTOM layers; via stitching is applied between the two planes, especially around the RF section.
- **No copper or routing under the antenna** (2450AT18B100E, ANT1): the PCB is cut out under the antenna footprint and the area is excluded from the ground plane pour.
- **Decoupling capacitors** (100 nF, 0201) are placed as close as possible to each IC power pin.
- **Power traces** (VCC, 3V3, VBAT) are routed at **0.3 mm** width; data signals at minimum **0.15 mm**. Traces under BGA pads are reduced as required by the via fanout geometry.
- **No right-angle trace bends** — all routing uses 45° chamfers.
- **Battery connection** is made directly to TP_VBAT and TP_BAT_GND solder pads (no JST connector) to maximise vertical clearance inside the case.
- **ERC errors:** The "Only INPUT pins on NET ID" warning is accepted per project guidelines.
- **Dimension errors** caused by the three button cutouts and USB-C aperture in the board outline are accepted per project guidelines.

---

## Repository Structure

```
InkTime/
├── Hardware/
│   ├── InkTime.sch          # Eagle schematic
│   └── InkTime.brd          # Eagle board file
├── Manufacturing/
│   ├── Gerbers.zip           # Gerber + drill files
│   ├── InkTime_BOM.bom       # Bill of Materials
│   └── Inktime_PnP.cpl       # Pick & Place file
├── Mechanical/
│   ├── InkTime.f3z           # Fusion 360 full assembly
│   └── InkTime_Exploded.f3z  # Fusion 360 exploded view
├── Images/
│   └── (PCB renders, 3D renders)
├── LICENSE
└── README.md
```

---

## License

This project is released under the **Apache License 2.0**. See [LICENSE](LICENSE) for details.
