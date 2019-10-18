RD53a testing process.
======================

##### 1. Visual inspection
- [ ] Note down (and photograph) any scratches or damage on the board.
- [ ] Check the wire bond connections under a microscope to make sure there is no detachment during transport.

##### 2. Jumpers configuration
Check that SCC is configured in LDO mode. Check the links below:
* [Short instruction](https://yarr.readthedocs.io/en/latest/rd53a/)
* [Complete SCC configuration](https://twiki.cern.ch/twiki/pub/RD53/RD53ATesting/RD53A_SCC_Configuration.pdf).

##### 3. Connections

- [ ] Connect DisplayCable to **DP1** connector on SCC card and to **Port A** of Ohio Adapter card (leftmost when viewed from the back of the computer).

- [ ] Connect LV power cable to **PWR_IN** molex connector of SCC card (upper wires should be + and the bottom wires should be -)
- [ ]  Set LV power supply limits:
 * Voltage: **1.8 V**
 * Current: ~1.5 A would be ok

##### 4. YARR config
- [ ] First of all download chip config file [rd53a_TravellingChip.json](https://travelling-module.readthedocs.io/en/latest/files/rd53a_TravellingChip.json) and change **"Name"** parameter value to your chip number (for example "0x1F72").

For ScanConsole you need to specify 3 files via corresponding flag:
- [ ] Controller (-r): use default configs/controller/specCfg.json
- [ ] Connectivity (-c): change default configs/connectivity/example_rd53a_setup.json as follows:
```
{
    "chipType" : "RD53A",
    "chips" : [
        {
            "config" : "configs/rd53a_TravellingChip.json",
            "tx" : 0,
            "rx" : 0,
            "enable" : 1,
            "locked" : 0
        }
    ]
}
```
Where configs/rd53a_TravellingChip.json is the path to your chip config file.
- [ ] Scan (-s): use corresponding scan configs from configs/scans/rd53a/

##### 5. Environment preparation
- [ ] Switch to gcc 7.0: ```source /opt/rh/devtoolset-7/enable```
- [ ] Vivado env: ```source /opt/Xilinx/Vivado_Lab/2019.1/settings64.sh```
- [ ] Check driver: ```$ dmesg | grep spec``` If the drive is not loaded use: ```sudo modprobe -v specDriver```

##### 6. Digital scan
- [ ] Try simple digital scan ```bin/scanConsole -r configs/controller/specCfg.json -c configs/connectivity/travell_rd53a_setup.json -s configs/scans/rd53a/std_digitalscan.json```

If the program can't configure the chip, it may be because the internal voltages supplied from the regulators are too low.

##### 7. IREF trim to 4 mkA
- [ ] Configure KEITHLEY 2410 to measure current with 10 mkA limit.
- [ ] Remove IREF_IO jumper from SCC card and connect current measure probes to that pins.
- [ ] Turn on measurements.
- [ ] Turn on LV power supply (**1.8V** and ~1.5A)
- [ ] The IREF value should be as close to 4mkA as possible. If not:
 * switch the power off
 * change IREF_TRIM register valuer (binary value, least significant bit first)
 * turn the power supply On and check new value

##### 8. VREF_ADC trim to 0.9V
- [ ] 
- [ ]
- [ ]
