# ICs
- [STUSB4500](https://www.st.com/en/interfaces-and-transceivers/stusb4500.html)
  - $2 on JLCPCB
  - Open source project that uses it
    - [schematic](https://github.com/oxplot/fpx/blob/master/board/schematics.svg)
  - [Eval board](https://www.st.com/resource/en/data_brief/eval-scs001v1.pdf)
  - Main transistor on VBus_EN_SNK
    - How important is the power dissipation? Well very, ofc
    - Recommended on datasheet [STL6P3LLH6](https://www.st.com/resource/en/datasheet/stl6p3llh6.pdf)
      - -30V, -6A, 30mOhm, 3W
      - Max Vgs +-12
    - [AO3401A](https://datasheet.lcsc.com/lcsc/1810171817_Alpha---Omega-Semicon-AO3401A_C15127.pdf)
      - -30V, -4A, Rdson 100mOhm, 1W
      - Vth -1V
      - At
    - []()
    - [IRFR9024NTRPBF](https://www.mouser.ch/datasheet/2/196/Infineon_IRFR9024N_DataSheet_v01_01_EN-3363169.pdf)
      - -55V, -11A, Rdson 0.2O0hm, 38W
      - Vth max -4V
      - Max Vgs -20V, more confortable limit
      - But Rdson is high
        - At 5A, 0.2 Ohm = 5W, that's a lot of wasted power
    - [SSM3J332R](https://datasheet.lcsc.com/lcsc/1806140422_TOSHIBA-SSM3J332R-LF_C146367.pdf)
      - -30V, -6A, Rdson 0.050Ohm, 1W
      - Vth ~ -1V
      - Rdson @ -12V 0.05. At 5A = 0.25W, sounds good.
      - Max Vgs -12V, so we're close to the limit
    - [IRF9Z34NSTRLPBF](https://jlcpcb.com/partdetail/181165-IRF9Z34NSTRLPBF/C169780)
      - +- 20V Vgs, but 0.1 mOhm. At 5Amps, 2.5W, it's a lot
    - [DMP3028LFDE](https://jlcpcb.com/partdetail/DiodesIncorporated-DMP3028LFDE13/C150441)
      - [datasheet](https://www.diodes.com/assets/Datasheets/DMP3028LFDE.pdf)
      - Vgs +-20V, 5 Amps
      - 0.4 W only power dissipation
      - Rdson 0.04 Ohm. At 5 Amps, it's 1.25 W, above the 0.4 W limit
    - [AOD413A](https://jlcpcb.com/parts/componentSearch?searchTxt=AOD413A)
      - -40V, -12A, Rdson 0.045 Ohm, 1.5W
			- Vgs +-20V

# PD
	- Note that 12V is not part of the spec, but it's a common voltage. Anker apparently does not implement it?
  	- What if we request 15V @ 5A and add a buck converter to 12V?
	- Max 100 W (let's design for 100W but aim for max 50W for safety)
  	- At 12V, that's 8.3A
  	- But actually max current is 5A, only reaches 100W @ 20V
  	- So we can design with a 5A limit for the MOSFETs while observing the 100W limit
	- LED strips
  	- Depends on density of leds, ceiling is around 20 W/m, so we could power ~5 m tops. Realistic, lets aim for 3 m.
  	- At 12 V, that would be 20 * 3 / 12 = 2.5 A across however many channels we have

# ESP32-S3
- [Phil's design](https://www.youtube.com/watch?v=yxU_Kw2de08)
  - [Schematic](https://github.com/pms67/ESP32-USB-Dongle/blob/main/ESP32_USB_Dongle-Schematic.pdf)
- Regulator
  - [AP2114](https://www.diodes.com/assets/Datasheets/AP2114.pdf)
  - [HT7333](https://www.angeladvance.com/HT73xx.pdf)
    - Up to 12V
    - Max 250mA
  - [AMS1117](http://www.advanced-monolithic.com/pdf/ds1117.pdf)
    - Max 15V
    - Basic part at $0.13 on LCSC
    - Max 1A
  - [SPX3819](https://jlcpcb.com/partdetail/Maxlinear-SPX3819M5_L_3_3TR/C9055)
    - Phils uses this one
    - Max 16V (it would be nice to support 20V just in case)
    - Abolsute max +/-20V, which is ok
  - [SPX1117M3](https://jlcpcb.com/partdetail/Maxlinear-SPX1117M3_L_3_3TR/C6862)
		- Max 20V, 800 mA
		- Perfect
- [ESP32-S3-WROOM-1](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf)
  - Contains module schematics
- Design reference
  - [Hardware design guidelines](https://www.espressif.com/sites/default/files/documentation/esp32-s3_hardware_design_guidelines_en.pdf)
    - RF
			- LNA_IN source 35 + 10j page 16
			- Using pi network for 50 Ohm antenna
			- [Pi Network calculator](https://www.eeweb.com/tools/pi-match/)
			- Antenna
  			- [ANT3216LL00R2400A](https://jlcpcb.com/partdetail/Yageo-ANT3216LL00R2400A/C293767) sounds popular
    			- Lots of use on GitHub
		- Flash
  		- Phil uses [W25Q16JVSSIQ](https://jlcpcb.com/partdetail/WinbondElec-W25Q16JVSSIQ/C82317)
  		- Discontinued in favor of [W25Q16JVSSIQ](https://jlcpcb.com/partdetail/WinbondElec-W25Q16JVSSIQ/C131025)
  		- See [ESP32-S3-WROOM-1](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf) schematic for pins
  		- GPIO45
    		- [Default pull down (page 22)](https://cdn-shop.adafruit.com/product-files/5477/esp32-s3_datasheet_en.pdf) which makes VDD_SPI be 3.3 what we want

# USB-C
- [ESP dev board](https://www.youtube.com/watch?v=IML9c_MsyQU)
  - Male connector with tabs from JLCPCB
  - 3 A
- [C2982483](https://jlcpcb.com/partdetail/gswitch-GT_USB8012A/C2982483)
  - 24 pins
  - 5A
  - Tabs
  - Fixture required for soldering, not ideal
- [C2938596](https://jlcpcb.com/partdetail/XkbConnectivity-U261_241N4BS2SS/C2938596)
  - 5A
  - Tabs
  - [Datasheet]()
  - We'll only need the "easy to access" A* pins on the front row, convenient
  - Any board thickness, easier
- [C319150](https://jlcpcb.com/partdetail/XkbConnectivity-U261_241N4BS60/C319150)
  - PCB edge mount
  - No extra tabs
  - 5A
  -
  - [datasheet](https://datasheet.lcsc.com/lcsc/2002271811_XKB-Connectivity-U261-241N-4BS60_C319150.pdf)
    - PCB width? 0.9 mm?
      - Has to be 0.8
  - Projects using it:
    - https://github.com/tillitis/tillitis-key1
    - https://github.com/bschan9228/MInG/tree/f5a2087e46fd469d9a58bee00fa3ce4dc36beebd/Hardware/Prototype/ESP_SoC
      - Also has RF circuit
      - [PCB thickness 0.8 mm](https://github.com/bschan9228/MInG/blob/f5a2087e46fd469d9a58bee00fa3ce4dc36beebd/Hardware/Prototype/ESP_SoC/ESP_SoC/v1/PCB_OUTPUT/READ%20THIS%20FIRST.txt)
        - So it fits the USB-C connector?
        - But it will change the 4 layer impedance calculation

# Tactile Switch
- [](https://jlcpcb.com/partdetail/gswitch-GT_TC054A_H035L1/C778158)
  - Edge mounted

# Power Delivery
- 12

# LED Switching MOSFETs
- [AO3400A](https://datasheet.lcsc.com/lcsc/1811081213_Alpha---Omega-Semicon-AO3400A_C20917.pdf)
  - For low side switching the LEDs
  - But we can't really do low side given the common GND of the LED strips
  - Max Vgs -12V, but if we do low side switching, we're at 3.3V
- [AO3401A](https://datasheet.lcsc.com/lcsc/1810171817_Alpha---Omega-Semicon-AO3401A_C15127.pdf)
	- -30V, -4A, Rdson 100mOhm, 1W
	- Vth -1V
	- Vgs max -12, we'd be too close to the limit?
  	- Maybe use a voltage divider to bring it down to 10V?
  	- Or a transducer
	- At 4A, -12V Vgs, 0.05 Ohm = 0.8 W, so we're kinda good for power dissipation?
- A popular controller H801 uses a level shifter between the PWM signals and the P-channel MOSFET gates:
  - [https://tinkerman.cat/post/closer-look-h801-led-wifi-controller](teardown)
	- [HC245 datasheet](https://www.ti.com/lit/ds/symlink/sn74hc245.pdf?ts=1699776429860&ref_url=https%253A%252F%252Fwww.google.com%252F)
  	- [At JLCPCB](https://jlcpcb.com/partdetail/Toshiba-74VHC245FT_BE/C8096)
  	- 3.3, 5V compatible
  	-

# Barrel jack
- 5.5mm OD, 2.1mm ID
  - [Example](5.5mm 2.1mm)
  - Positive center

# Buck Converter ICs
- [TPS61175PWPR](https://jlcpcb.com/partdetail/TexasInstruments-TPS61175PWPR/C43276)
  - 3A
- [LM2596DSADJG](https://jlcpcb.com/partdetail/Onsemi-LM2596DSADJG/C92134)
  - 3A
  - SMT
- [AOZ1212](https://aosmd.com/res/data_sheets/AOZ1212AI.pdf)
  - Used by the popular H801 controller
  - 4.5V to 27V input
  -

# LDO
- [7812](https://datasheet.lcsc.com/lcsc/2304140030_STMicroelectronics-L7812CV_C105651.pdf)
  - 1.5 A
  - Heat?
- Options for up to 20V:
  - [Digikey query](https://www.digikey.ch/en/products/filter/voltage-regulators-linear-low-drop-out-ldo-regulators/699?s=N4IgjCBcpgTAnFUBjKAzAhgGwM4FMAaEAeygG1wAOWWAdgDYQjYwBmWAVlqZFlftqsALDzq0hYRs1oIOEImDDxaHAAwgAukQAOAFyggAyroBOASwB2AcxABfIl1ZIQqSJlyES5cBzmUnCvRgEtwKHPCwQqHg9MK08jEclPSwPJIq4mmUQuGUmjr6kEamlja29rzerAB0rABuAARYACakGrZAA)
  - [](https://jlcpcb.com/parts/componentSearch?searchTxt=LM1117MPX)
    - Max 24V
    - 500 mA
    - Perfect for us
  - [LM1117MPX-3.3/NOPB](https://jlcpcb.com/partdetail/TexasInstruments-LM1117MPX_3_3NOPB/C9662)
    - Max 20V
  - [NCP718BSN330T1G](https://www.digikey.ch/en/products/detail/onsemi/NCP718BSN330T1G/9169776)
    - Max 24V
    - But only 300mA

# ESD protection
- [ESDA25P35](https://www.mouser.ch/datasheet/2/389/esda25p35_1u1m-1848969.pdf)
  - Recommended by the STUSB4500 datasheet
  - Standoff voltage 22V
- [ESDA25W](https://jlcpcb.com/partdetail/TechPublic-ESDA25W/C3021136)
  - Operating voltage 24V
  - Dual

# TODO
- [X] Voltage divider & ADC to measure VBUS
- [X] Expose Vhigh on a connector (in addition to LED drivers), so we can use the board as a simple USB-C trigger board
- [X] Mounting holes
  - Got rid because of space
- [X] ESD protection
- [X] Barrel jack (backward voltage?)
- [X] Test pads
- [ ] Can we also make it control addressable LED strips?
- [X] Text in the back: max 12V, X watts/Amps per channel
- [X] I2C pullups
- [X] Expose I2C pins
- [X] More ground stitching vias close to the screw on connector
