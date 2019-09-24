Commissioning of the experimental setup
=======================================

For RD53a Travelling Modules.
- For more details visit [RD53A Testing twiki page](https://twiki.cern.ch/twiki/bin/viewauth/RD53/RD53ATesting).
- The current [Travelling Module schedule can be found here](https://docs.google.com/spreadsheets/d/1uaxTqf-mSaBd6_UuOKpe0N2RLMPN3AFnNkIQ4CnIEUY/edit?usp=sharing).
- See also [Travelling Module participation table](https://docs.google.com/spreadsheets/d/1qtbo60B43QQgahlq0hG3hi0f7tpFaO4p044CMa54tGE/edit#gid=0).
- [x] Please subscribe to the [rd53-a-testing e-group](https://e-groups.cern.ch/e-groups/EgroupsSubscription.do?egroupName=rd53-a-testing) mailing list.

Xilinx software and drivers installation
---------------------------------
- [x] [Download Vivado Lab Solutions](https://www.xilinx.com/support/download.html). And extract tar.gz file with ```tar -zxvf filename.tar.gz```, or use 7z for Windows.
- [x] Install Vivado Lab Solutions by running ```xsetup``` (without sudo). [Vivado User Guide (pdf).](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2016_4/ug973-vivado-release-notes-install-license.pdf)
- [x] Install Xilinx cable driver by running script (with sudo) ```install_drivers``` from folder ```<Vivado Install
Dir>/data/xicom/cable_drivers/lin64/install_script/install_drivers/``` . [About Xilinx USB Cables (pdf)](https://www.xilinx.com/support/documentation/user_guides/ug344.pdf)

YARR software installation
--------------------------
- [x] [Make sure the gcc 7.0 or higher is installed.](https://yarr.readthedocs.io/en/latest/install/)
- [x] In order to use gcc 7.0 instead of your default one execute: ```source /opt/rh/devtoolset-7/enable```
- [x] [Download YARR release v1.0.0](https://gitlab.cern.ch/YARR/YARR/tree/v1.0.0)
- [x] [Build YARR (no specific toolchain needed for PC)](https://yarr.readthedocs.io/en/latest/install/)
- [x] [Build plotting tools](https://yarr.readthedocs.io/en/latest/rootscripts/)
- [x] [Install Kernel Driver for PCIe FPGA card](https://yarr.readthedocs.io/en/latest/kernel_driver/)
- [x] Check if the driver is installed by ```dmesg | grep spec```
- [ ] Familiarised yourself to some degree with the [ScanConsole](https://yarr.readthedocs.io/en/latest/scanconsole/) and making [graphs from analog/digital reads.](https://yarr.readthedocs.io/en/latest/rootscripts/)

Hardware
--------
- [x] Good quality single or dual channel laboratory power supply capable of providing 1.8V @ 1A on each channel.
- [x] Make sure you have Ohio Adapter with serial number >200, or to avoid double AC coupling on the CMD line, one should [replace the capacitors with jumpers and remove the termination](https://yarr.readthedocs.io/en/latest/pcie/)
- [x] Set Ohio Adapter jumper to 3V PCI pin.
- [x] Set Trenz TEF1001 4bit dip switch (S1) 1=off (CPLD JTAG off), 2=off, 3=ON, 4=off (for 1.8V JTAG power)
- [x] Install FMC Adapter Card into the PCIe card.
- [x] Install heat sink (optional cooler) into the Kintex-7 FPGA.
- [x] Install PCIe card into PC.
- [x] Get JTAG programmer. Tested at CERN: [Digilent JTAG HS3](https://www.digikey.com/product-detail/en/digilent-inc/210-299/1286-1047-ND/5015666), [Xilinx Platform Cable USB II](https://www.xilinx.com/products/boards-and-kits/hw-usb-ii-g.html). We use Platform Cable USB (model DLC9G) by WaveShare Electronics.
- [x] Get miniDisplayPort-DisplayPort cable 1-2 m long (1-4 pcs)
- [x] Get 1-2m long custom made cable with a 4-pin Molex connector to power the chip
- [ ] Get 20cm long custom made cable with 2-pins connector to measure NTC resistance or various voltages (2 pcs minimum)

Additional hardware
-------------------
- [x] [Download NTC thermistor datasheet](https://www.mouser.de/datasheet/2/362/ktthermistor-3035.pdf)
- [ ] Prepare Arduino thermistor temperature logger
- [ ] Make sure you have 1 Tb disk space

Flashing the FPGA firmware
--------------------------
- [x] Set up gcc 7.0: ```source /opt/rh/devtoolset-7/enable```
- [x] Set up the Xilinx environment. For Vivado Lab Solutions: ```source /opt/Xilinx/Vivado_Lab/2019.1/settings64.sh```
- [x] [Run correct flashing script](https://yarr.readthedocs.io/en/latest/pcie/) (```Yarr-fw/script/flash-trenz.py``` for Trenz card). Then [choose your firmware](https://yarr.readthedocs.io/en/latest/fw_guide/) organized first by type of Front-End, then by adapter card. For Trenz TEF-1001 card (with Xilinx Kintex-7 FPGA) with Ohio RD53A Multi Module Adapter for Single RD53A channel enabled on Port A it should be: ```Yarr-fw/syn/xpressk7/bram_rd53a_quad_ohio-trenz/rd53a_quad_ohio_160Mbps.bit```
- [x] If the python script does not work: [use the Vivado GUI](https://github.com/Yarr/Yarr-fw/blob/master/syn/xpressk7/README.md)
- [x] After flashing reboot PC and check firmware is loaded correctly with "lspci" command
- [x] Check the test programs runs successfully "Yarr/src/bin/specComTest"

Results uploading
-----------------
[Pixel Modules in the ITk Production Database (twiki documentation).](https://twiki.cern.ch/twiki/bin/viewauth/Atlas/PixelModulesInITkPD)
- [ ] Data of the travelling chips and modules should be stored in a common database. For the moment it is a CERNbox ([description](https://travelling-module.readthedocs.io/en/latest/datastorage/)). In order to read and write data to the corresponding CERNbox folder: sing in your [CERN account](https://account.cern.ch/account/) go to E‑Groups membership and subscribe to these e-groups (it takes some time to get approval):
 * cernbox-project-atlas-itk-pixel-module-readers
 * cernbox-project-atlas-itk-pixel-module-writers
 * atlas-itk-pixel-module
- [ ] To read and write data to the Production Database [register and read the short tutorial.](https://gitlab.cern.ch/jpearkes/itkpd_tutorial/blob/master/README.md)
- [ ] Upon receipt of the Travel Pack and after departure fill out the [ContactDetails table](https://twiki.cern.ch/twiki/bin/view/Atlas/ContactDetails).
