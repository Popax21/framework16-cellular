# Framework Laptop 16 M.2 Cellular Modem Expansion Bay Module

This repository holds the WIP design files for a module for the Framework Laptop 16's Expansion Bay allowing for the connection of a standard M.2 B-Key cellular modem, also featuring a microSIM connector for the insertion of a standard SIM card.
It was designed for usage with Quectel RM50X / RM5X0-series 5G modules, but other 3.7V-tolerant modules are expected to work as well.

> [!WARNING]
> This module provides the M.2 slot with a VCC of 3.7V, and not the standard 3.3V!
> This is the recommended voltage of the Quectel modems the module is designed around, but you may have to adapt the schematic in case you want to use a different module.


The module provides the M.2 slot with both a PCI-E x1 interface as well as an USB2 HS (max 480Mbps) interface, however routing priority has been given to the PCI-E lane, so the full bandwith may not be available over the USB port.
Note that no USB3.x interface has been routed to the slot!
This means that in practice you should be able to use any M.2 cellular module, however only PCI-E capable modules will be able to provide high-speed connectivity.

> [!NOTE]
> This design decision is rooted in the Expanion Bay's interface to the rest of the computer, which only provides a single USB2 port for modules to use.
> Providing a full USB 3.0 (or higher) interface for the modem would have required the addition of a PCI-E USB host controller, which would have in turn complicated the PCI-E logic since now two separate REFCLK signals would be required.
> While there is support for receiving two separate REFCLKs from the mainboard since hardware revision 7, no such support was implemented to reduce the design complexity of the module.

Some functionality exposed by the board (i.e. hardware rfkill/airplane mode, modem resets / power-on shutdowns) are exposed behind an I2C GPIO port, and will require support from the Framework Laptop 16's EC (Embedded Controller) MCU to fully operate.
No such support has been developed as of now, however such support is planned to be released at a future date in the form of firmware patches.

All Expansion Bay modules feature an EEPROM which holds a "descriptor" for the installed module, which contains, among other things, the module's serial number as well as GPIO configuration.
By default the module should be picked as a Dual M.2 Expansion module until an appropriate descriptor has been written onto the EEPROM.
The PCB has been designed to still operate when treated as such an expansion module, however a proper custom EEPROM descriptor will be developed at a future date as part of EC integration.

No pre-packaged schematics / Gerber files are available at the moment, but a GH Actions pipeline may be set up at a future date.

> [!IMPORTANT]
> If you still plan on ordering a PCB through e.g. JLCPCB, ensure that you enable Impedance Matching and specifically select stackup JLC041611H-3313 as part of the checkout process.
> This stackup is required for the sensitive PCI-E / USB / RF signals to be properly impedance matched and not be distorted.

This design uses modified versions of the template PCB and FX Beam connector symbol / footprint published by jyancat, found [here](https://github.com/jyancat/ExpansionBay/tree/main/Electrical/KiCad_templates/Expansion_Bay).
