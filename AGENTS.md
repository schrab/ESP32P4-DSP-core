# ESP32P4-M3-DEV KiCad Project - Agent Instructions

This document describes how to work with the ESP32-P4 MIDI/DSP carrier board KiCad project using the KiCad MCP Server.

## Project Overview

- **Working Directory:** `C:/Users/schra/Developer/JC-ESP32P4-M3-DEV/ESP32P4/`
- **Main Board File:** `ESP32P4.kicad_pcb`
- **Main Schematic:** `ESP32P4.kicad_sch`
- **Kicad MCP Server:** `C:/Users/schra/Developer/KiCAD-MCP-Server`

## KiCad MCP Server Status

The KiCad MCP Server at `C:/Users/schra/Developer/KiCAD-MCP-Server/` is available but has known issues with the `place_component` tool - parameter types may not serialize correctly (JSON booleans/objects get sent as strings). Use file editing for board modifications until resolved.

## Project Components

### Main Components (from kicad_pcb):

| Reference | Value | Footprint | Layer | Position | Notes |
|-----------|-------|-----------|-------|----------|-------|
| U1 | ESP32P4_QFN104 | JC-ESP32P4-M3:JC-ESP32P4-M3_V0.2 | F.Cu | (30, 42) | Main ESP32-P4 module |
| U2 | LM1117MP-3.3 | Package_TO_SOT_SMD:SOT-223-3_TabPin2 | B.Cu | (44, 64) | 3.3V regulator |
| U3 | IP5306 | Package_SO:SOIC-8_5.3x6.2mm_P1.27mm | B.Cu | (12, 71) | Power management |
| U4 | 6N137 | Package_SO:SOIC-8_3.9x4.9mm_P1.27mm | B.Cu | (37, 22) | Optocoupler (MIDI IN) |
| J1 | WM8978_BRK | Connector_PinSocket_2.54mm:PinSocket_2x05_P2.54mm_Vertical | B.Cu | (54, 42) | WM8978 codec breakout socket |
| J4 | USB-C_DEV | Connector_USB:USB_C_Receptacle_HRO_TYPE-C-31-M-12 | B.Cu | (30, 75) | USB-C device port |
| J5 | USB-C_DEV | Connector_USB:USB_C_Receptacle_HRO_TYPE-C-31-M-12 | B.Cu | (58, 66) | USB-C host port |
| J7 | BAT_1S | Connector_JST:JST_PH_B2B-PH-K_1x02_P2.00mm_Vertical | B.Cu | (3.5, 72) | Battery header |
| J9 | MIDI_OUT | Connector_Audio:Jack_3.5mm_CUI_SJ-3523-SMT_Horizontal | B.Cu | (7, 38) | MIDI OUT jack |
| J10 | MIDI_IN | Connector_Audio:Jack_3.5mm_CUI_SJ-3523-SMT_Horizontal | B.Cu | (7, 57) | MIDI IN jack |
| SW1 | BOOT | Button_Switch_SMD:SW_SPST_B3U-1000P | F.Cu | (88, 10) | Boot button |
| SW2 | RESET | Button_Switch_SMD:SW_SPST_B3U-1000P | B.Cu | (70, 6) | Reset button |
| SW3 | POWER | Button_Switch_THT:SW_PUSH_6mm_H4.3mm | B.Cu | (78.5, 65.75) | Power button |
| SW4 | BTN1 | Button_Switch_THT:SW_PUSH_6mm_H4.3mm | F.Cu | (88, 26) | Button 1 |
| SW5 | BTN2 | Button_Switch_THT:SW_PUSH_6mm_H4.3mm | B.Cu | (88, 40) | Button 2 |
| SW6 | BTN3 | Button_Switch_THT:SW_PUSH_6mm_H4.3mm | B.Cu | (88, 54) | Button 3 |
| SW7 | BTN4 | Button_Switch_THT:SW_PUSH_6mm_H4.3mm | B.Cu | (88, 68) | Button 4 |
| ENC1 | EC11 | Rotary_Encoder:RotaryEncoder_Alps_EC11E-Switch_Vertical_H20mm_CircularMountingHoles | F.Cu | (80, 18) | Encoder 1 |
| ENC2 | EC11 | Rotary_Encoder:RotaryEncoder_Alps_EC11E-Switch_Vertical_H20mm_CircularMountingHoles | F.Cu | (80, 34) | Encoder 2 |
| D1 | SS34 | Diode_SMD:D_SMA | B.Cu | (39, 69.5) | Schottky diode |
| D2 | PWR | LED_SMD:LED_0603_1608Metric | B.Cu | (78, 58) | Power LED |
| D3 | 1N4148W | Diode_SMD:D_SOD-123 | B.Cu | (47, 56.5) | Diode |
| L2 | 1uH | Inductor_SMD:L_Bourns_SRN6045TA | B.Cu | (20.5, 70) | Inductor |

### Board Status
- **Dimensions:** 100.1 × 80.1 mm
- **Layers:** 2-layer board (F.Cu / B.Cu)
- **Current State:** All footprints placed, **NO routing**, no zones/zones
- **Design Rule Check:** Not yet run after placement

## KiCad MCP Server Information

**Server Location:** `C:/Users/schra/Developer/KiCAD-MCP-Server`

### Available Tools Categories:
The server exposes tools via the MCP protocol. Categories include:
- Schematic tools (component placement, wiring, properties)
- PCB tools (component placement, routing, DRC)
- Library tools (symbol lookup, footprint registration)
- Generic tools (project info, server health)

### Common MCP Calls (when working):

#### List libraries:
`mcp__kicad__list_library_table` - Lists available footprint libraries

#### List footprint library contents:
```
DeferredToolCall({
  "name": "mcp__kicad__list_library_footprints",
  "input": {"library_name": "Card"}
})
```

#### Place component:
```
DeferredToolCall({
  "name": "mcp__kicad__place_component",
  "input": {
    "componentId": "Card:TFP09-2-12B",
    "position": {"x": 15, "y": 7.5, "unit": "mm"},
    "reference": "J6",
    "rotation": 180,
    "layer": "B.Cu",
    "boardPath": "C:/Users/schra/Developer/JC-ESP32P4-M3-DEV/ESP32P4/ESP32P4.kicad_pcb"
  }
})
```

### Direct File Editing (Fallback):
When MCP tools fail (they often fail with parameter type errors), edit `ESP32P4.kicad_pcb` directly. KiCad files are text-based S-expressions.

### Routing Trace Format:
```
(footprint "trace_name"
  (layer "B.Cu")
  (uuid "uuid-here")
  (at x y angle)
  ...
  (pad "pin_number" thru_hole circle (at x y 90) (size w h) (drill d)
    (layers "B.Cu" "B.Mask"))
  ...
)
```

## Common KiCad Project Files:
- `ESP32P4.kicad_pro` - Project configuration
- `ESP32P4.kicad_sch` - Schematic
- `ESP32P4.kicad_pcb` - PCB layout (primary work file)
- `ESP32P4.kicad_prl` - Project preferences
- `ESP32P4.kicad_dsn` - Design file (auto-generated)

## Key Files for Reference:
- `docs/` - Project documentation
- `fp-lib-table` - Footprint library table
- `sym-lib-table` - Symbol library table

## Development Workflow:
1. Modify schematic (`ESP32P4.kicad_sch`) if adding components
2. Sync schematic to board via `sync_schematic_to_board`
3. Place components using `mcp__kicad__place_component` or direct file editing
4. Route traces using routing tools or direct file editing
5. Run DRC via `run_drc`
6. Export fabrication files using `export_gerbers`, `export_pos`, etc.

## Common Issues:
- MCP `place_component` tool passes `position` as string instead of object - manual file editing may be needed
- `libraries` list 0 footprints if `.kicad_mod` files have internal name mismatches
- Always verify footprint names match library entries exactly