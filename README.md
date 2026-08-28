# hw_ic_k1 — K1 Master Gate Card (PCIe 2.1 x2 Injection)

Open-source hardware — KiCad design files for the **K1 Master Gate Card**,
the master-side converter card in the IOCONVERT V2.0 1-Master-to-4-Worker
chained NTB cluster.

## Overview

K1 sits at the **master side** of a PCIe NTB chain. It provides USB Type-C
management access, pre-authentication gating (SE050 via I2C), and a PCIe
2.1 x2 data path injected into the master host through an OCuLink connector.

```
KEY (secure vault) ──Type-C──► K1 Master Gate Card ──OCuLink──► Master Host
                                  │
                                  ├─ USB management (SBU I2C ↔ SE050 pre-auth)
                                  ├─ PCIe 2.1 x2 lane injection
                                  └─ 5V pre-auth power rail (TLV61046 boost)
```

## Key Features

- **Form factor**: 80 × 38 mm, 4-layer PCB
- **Interface**: USB Type-C (KEY), OCuLink SFF-8612 (host), 12V DC-in
- **Security**: STM32G0B1 MCU gatekeeper + SE050 pre-authentication via SBU I2C
- **Data path**: PCIe 2.1 x2 with PI3DBS16412 lane-swap MUX
- **Power**: 3.3V_AUX → 5V boost (TLV61046), LDO 5V→3V3, 12V→3V3 DC-DC
- **Management**: OOB UART passthrough, opto-isolated power button (LTV-356T)

## Repository Contents

| Path | Description |
|---|---|
| `k1_v1.kicad_pro` / `.kicad_pcb` | KiCad 10 project + PCB (self-contained) |
| `sch/` | Schematics: connectors, MCU & sideband, power (12V/5V/3V3), decoupling |
| `lib/IOCONVERT.kicad_sym` | Symbol library (80 symbols) — required for schematics |
| `lib/ForgeOS.pretty/` | Footprint library (19 footprints) |
| `boards/` | Board/nets configuration (YAML) |

## Getting Started

1. Install [KiCad](https://www.kicad.org/) 10.x
2. Clone this repository
3. Open `k1_v1.kicad_pro`
4. If symbols/footprints show as missing, add `lib/IOCONVERT.kicad_sym`
   to the symbol library table and `lib/ForgeOS.pretty/` to the footprint
   library table (global or project-level)

## License

AGPL-3.0 (see `LICENSE`)

---

*Part of the IOCONVERT V2.0 family: [hw_ic_k2](https://github.com/dataxcash/hw_ic_k2) (worker data-plane card) · [hw_ic_key](https://github.com/dataxcash/hw_ic_key) (secure key vault)*
