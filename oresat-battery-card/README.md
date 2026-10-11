---
title: "**OreSat Battery Card**"
subtitle: |
  **Fabrication and Assembly Information**\
  For build TIME-STAMP
fontsize: 10pt
geometry:
  - margin=0.5in
toc: true
toc-depth: 2
colorlinks: true
urlcolor: blue
---

\newpage

# About this Board

## Board Description

The OreSat Battery Card carries two packs of 2S1P 18650 Lithium Ion cells. It operates completely autonomously. The C3 card
can, however, turn on the Battery Card's OPD, which will enable the onboard microcontroller and the battery card will begin
sending telemetry over CAN, including from a fuel gauge for each battery pack.

The battery card has two heater strips, which firmware uses to heat the batteries to >= 5 C.

Their are four inhibit switches as well - two for the OreSat !SHUTDOWN circuit, one for positive battery disconnect, and 
one for negative battery disconnect.

## Documentation Links

- Git repository: <https://github.com/oresat/oresat-batteries>
- [Design Notes](https://docs.google.com/document/d/1yaPGtJSM5RQG5k7sVpsDxBkVu4CHtm4LMNnisgOaNRg/edit?usp=sharing)


## Documentation Files

| Filename                        | Notes                                    |
| ------------------------------- | ---------------------------------------- |
| README.pdf                      | This README file                         |
| PROJECT-NAME-outline.dxf        | Board outline (with holes) in DXF format |
| PROJECT-NAME-pcba.step          | 3D model of PCBA (with components)       |
| PROJECT-NAME-render-bot.jpg     | Render of the top of the 3D model        |
| PROJECT-NAME-render-bot.jpg     | Render of the bottom of the 3D model     |
| PROJECT-NAME-schematic.pdf      | PDF of board schematics                  |

## Contact Information

- Website: <https://www.oresat.org/>
- Email: <oresat@pdx.edu>
- Instagram: @pdxaerospace

## Board Renders

![Render of the top of the 3D model](./build/documentation/PROJECT-NAME-render-top.jpg){width=50%}
![Render of the bottom of the 3D model](./build/documentation/PROJECT-NAME-render-bot.jpg){width=50%}

\newpage

# Printed Circuit Board (PCB) Fabrication Information

## Board Info

- 4 layer board
- Bounding box is 99.8 x 94.6 mm
- Board thickness is 1.59 mm

## Board Requirements

- Design Rules
    - Minimum Trace / Space design rules
       - Outer layers: 0.127 mm (5.0 mil) / 0.127 mm (5.0 mil)
       - Inner layers: 0.127 mm (5.0 mil) / 0.127 mm (5.0 mil)
    - Outer dimension router tolerance: +/- 0.254 mm (10.0 mil)
    - Hole placement tolerance: +/- 0.075 mm (3.0 mil)
    - Inner tab routed slot tolerance: +/- 0.254 mm (10.0 mil)
- Drills
   - Drill Positional Tolerance: 0.051 mm (2.0 mil)
   - Drill Size tolerance: +/- 0.064 mm (2.5 mil)
- Plated/Un-plated holes
  - Via/PTH minimum diameter: 0.254 mm (10 mil)
  - Via/PTH minimum annulus: 0.102 mm (4 mil) radius
- Outline/Routing
  - This board requires a 1 mm router bit.
- Slots
  - There are 16 slots.
- Cutouts
  - There are no cutouts
- There are 3 fiducials on the top layer.
- Panel tabs ("mouse bites")
   - Card edges must be smooth; no mouse bites or other intrusions into the card outline.
   - If external mouse bites are required, minimize and customer will remove by hand before assembly.
- If not otherwise specified, fabricate PCB to IPC 6012 Class 2 or better.

## Stackup /  Materials

- Outside copper layers is 1 oz Cu after plating (0.043 mm / 1.7 mil)
- Inside copper layers are 0.5 oz Cu (0.018 mm / 0.7 mil)
- There are no requirements on the prepreg or core of this four layer stackup.
- Board Surface treatment should be ENIG, althogh immersion Silver is acceptable.
- White silkscreen on top and bottom surface
- Taiyo PSR-4000 or equivalent soldermask on top and bottom, no requirements for color.

## Array / Panel Information

- Coordinate with Contract Manufacturer (CM) for optimal size of this panel.
- If no feedback from CM, then produce single boards (no panel).

## Fabrication Files

### IPC-2581 File

| Filename                 | Notes                                   |
| ------------------------ | --------------------------------------- |
| PROJECT-NAME-ipc2581.xml | IPC-2581 board information file         |

### Legacy PCB Files

| Filename                      | Notes                                         |
| ------------------------------| --------------------------------------------- |
| PROJECT-NAME-Edge_Cuts.gbr    | RS274X file for the dimension (outline) layer |
| PROJECT-NAME-F_Silkscreen.gbr | RS274X file for the top silkscreen            |
| PROJECT-NAME-F_Mask.gbr       | RS274X file for the top soldermask            |
| PROJECT-NAME-F_Cu.gbr         | RS274X file for the top copper layer          |
| PROJECT-NAME-In1_Cu.gbr       | RS274X file for the layer 2 copper            |
| PROJECT-NAME-In2_Cu.gbr       | RS274X file for the layer 3 copper            |
| PROJECT-NAME-B_Cu.gbr         | RS274X file for the bottom copper layer       |
| PROJECT-NAME-B_Mask.gbr       | RS274X file for the bottom soldermask         |
| PROJECT-NAME-B_Silkscreen.gbr | RS274X file for the bottom silkscreen         |
| PROJECT-NAME-NPTH.drl         | Excellon file for non-plated through holes    |
| PROJECT-NAME-PTH.drl          | Excellon file for plated through holes        | 

\newpage

# Printed Circuit Board Assembly (PCBA) Information

## Assembly Info

- All components are on the top side of the board.
- This PCBA uses both surface mount (SMT) and through-hole (THT) components.

## Assembly Requirements

- Assemble to IPC Class 2 or better
- Bake components that are not moisture sealed to appropriate levels as required.
- Any solder paste (leaded or RoHS) is acceptable. RoHS solder is slightly preferred.
- Any no-clean or aqueous wash flux is acceptable.
- No conformal coating.

## Component Specific Assembly Information

- ASSEMBLY OF THIS BATTERY CARD IS SAFETY CRITICAL. CAREFULLY FOLLOW THE ASSEMBLY SOP,
[PSAS SOP - OreSat Battery Card Assembly](https://docs.google.com/document/d/1YFvptCjQO5j7sZraa8bQpTQ43OzNet4JrRba6-A-SFA/edit?usp=sharing).

## Assembly Files

### IPC-2581 File

| Filename                 | Notes                           |
| ------------------------ | ------------------------------- |
| PROJECT-NAME-ipc2581.xml | IPC-2581 board information file |

### Bill of Materials (BOM)

| Filename             | Description                            |
| -------------------- | -------------------------------------- |
| PROJECT-NAME-bom.csv | BOM in Comma Separated Variable format |

### Solder Paste Stencils

| Filename                 | Notes                                            |
| ------------------------ | ------------------------------------------------ |
| PROJECT-NAME-B_Paste.gbr | RS274X file for top/front solder paste stencil   |
| PROJECT-NAME-F_Paste.gbr | RS274X file for bottom/back solder paste stencil |

### Mounting/Placement Location

| Filename            | Description                                 |
| ------------------- | ------------------------------------------- |
| PROJECT-NAME.pos | Pick and place locations for components     |

