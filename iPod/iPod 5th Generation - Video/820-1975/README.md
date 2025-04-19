# 820-1975 - iPod Video Main Board
![iPod Video Logic Board - 820-1975](assets/820-1975_RenderView.png)

## Overview
This is an open-source, KiCad-based 820-1975 PCBA that has been reverse-engineered from a physical board.

## Disclaimer
The information contained in this repository has been generated via reverse-engineering of publicly available material, and is provided without warranty or guarantee of accuracy. [The Electron Vault](https://www.theelectronvault.com/) is not responsible for any losses or damages caused to equipment as a consequence of the information contained in this repository or associated documents.

## Setup
### Database Library
This project uses a local database library for all components. As the library is linked to the project through relative pathing, it should just work once the ODBC driver is installed.
1) Install an ODBC driver. I use this one http://www.ch-werner.de/sqliteodbc/
2) [Optional] Install an SQLite database editor. I use SQLiteStudio https://sqlitestudio.pl

### Theme
This KiCad project has a number of thematic customisations. These are however completely optional and everything *should* work fine with the default theme. If you however wish to install the theme, here is the procedure:
1) Install KiCad as normal.
2) Copy PREF/color/Black and White.json from the repo into the corresponding folder in your KiCad preferences.
3) Launch KiCad and open Preferences -> Colors.
4) The Black and White theme should now be available in Schematic Editor -> Colors and PCB Editor -> Colors.
5) Schematic font: Arial

## Known Limitations
As this is a reverse-engineered design, there are several limitations potentially affecting accuracy that must be acknowledged:
- **This board is not ready for production.** The intent was to make it visually accurate enough to assist with troubleshooting and repair. OpenBoardView (Landrex-BRD) compatible files (generated using [kicad-boardview](https://github.com/whitequark/kicad-boardview) exporter) are provided in the repo along with PDF schematics.
	- BoardView does not respect alphanumeric pin designation of packages like BGA/CSPs, and converts them to numeric only (nets are still correctly assigned). This is a known limitation of the kicad-boardview exporter. Either use the KiCad PCB file or net names to correlate pads with schematic.
	- Component placement is only photographically accurate - there may be mechanical interferences.
	- Board outline is best-effort. Use at your own risk.
	- Board stackup has not been recovered from the board, though the overall thickness should be representative. It's likely 6 layer. At the very least, there are blind vias on the top and bottom two layers, and potentially buried vias as well.
	- Only surface routing exists, primarily as a checklist aid/sanity check that nets have been assigned to the correct components. 
	- Trace widths are purely photographically scaled. Hand in hand with the lack of stackup definition, this also means none of it has been impedance controlled. Nevertheless minimum trace width appears to be 3 mil, so even if it were completed, you won't be able to send this through any old budget Chinese fab.
- Passive component (resistors, capacitors, inductors) values were measured with an LCR meter and rounded to the nearest standard value. Accuracy is not guaranteed.
- Ferrite beads/chip inductors are difficult to resolve as they look the same to a basic LCR and don't have physically unique features. A network analyser could be used to glean further information about frequency vs. impedance, however it still would not lead to a specific part number. Thus, for such parts (mainly found on dock connector signals and in front of switching regulators), educated substitutions have been made based on part size and expected maximum current on the line. Take the provided part numbers with a pinch of salt.
- The two 10uH wire-wound inductors found either side of the dock connector were not able to be identified, but are presumed to be Sumida brand due to stylistic similarity.
- The battery connector was not able to be confirmed, but is known to be manufactured by DDK due to the logo being embossed on the locking slider. Best guess based on an [EOL notice](https://www.ddknet.co.jp/English/order_stop_eng/list.html) on their website is that it is the FF15A series.
- The dock connector although a custom part, is clearly a Foxlink clone of the [JAE DD1 Series](https://www.jae.com/en/connectors/series/detail/id=64352). It has consequently been simply copied verbatim in this design.
- The BCM2722 (VPU) pinout is based off the sibling BCM2702 part. Thus, there may be incongruities.
- The PP5021C (MPU) pinout was assembled best-effort based on the PP5020 and PP5022 used in devices from other manufacturers. Thus, it likely has errors. Some pins such as those assigned to keyboard matrix or FireWire functions are entirely undesignated, since they're used as GPIOs in this design, and thus it's not possible to guess their specific functions based on context. The JTAG pin group has been identified, but the specific functions could not be assigned. It should not be difficult to resolve these with trial and error and an appropriate programmer.
- The pinout of the PCF50607 (PMU) is provided without guarantee. It has been speculatively assembled using context from surrounding circuits and sibling parts - PCF50605/6 and PCF50633.
- All net labels are named based on context, and thus their accuracy is not guaranteed.
- There are a handful of circuits (primarily involving FETs/BJTs) whose function was speculative or unable to be ascertained, though they are still captured faithfully in the schematic and accompanied with a note.
- The Click Wheel was reverse-engineered separately [here](https://theelectronvault.substack.com/p/ipod-video-5th-generation-click-wheel). Pin 7's function is entirely unknown currently, and there is some speculation around SDI (8) and SDO (11).
- Functions for pin 1 and 32 of the LCD connector (J2) are speculative. They go to I/O pins on the BCM2722, but are grounded on the LCD flex. Presumed therefore to be some sort of detect/interlock signal.