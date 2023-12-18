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
    - [dmp3098lsd-13](https://datasheet.lcsc.com/lcsc/2009161504_Diodes-Incorporated-DMP3098LSD-13_C531177.pdf)
      - Dual P Channel
      - Used by Phil's design
      - Abs max Vgs +-20V, only 4.4 A?
    - [DMP3056LSS](https://datasheet.lcsc.com/lcsc/1806090929_Diodes-Incorporated-DMP3056LSS-13_C110502.pdf)
      - -30V, -6A, 0.045 Ohm, 1.5W
    - [SI4909DY-T1-GE3](https://www.digikey.ch/en/products/detail/vishay-siliconix/SI4909DY-T1-GE3/4439625)
      - 40V 8A 27mΩ@10V,8A 3.2W
      - [datasheet](https://datasheet.lcsc.com/lcsc/1806030436_Vishay-Intertech-SI4909DY-T1-GE3_C222523.pdf)
      -

# PD
	- Note that 12V is not part of the spec, but it's a common voltage. Anker apparently does not implement it?
  	- What if we request 15V @ 5A and add a buck converter to 12V?
	- Max 100 W (let's design for 100W but aim for max 50W for safety)
  	- At 12V, that's 8.3A
  	- But actually max current is 5A, only reaches 100W @ 20V
  	- So we can design with a 5A limit for the MOSFETs while observing the 100W limit
	- LED strips
  	- Depends on density of leds, ceiling is around 20 W/m, so we could power ~5 m tops at 100W. But at 12V, we're looking more at around 60-ish W. Realistic, lets aim for 3 m.
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
- [AOZ1284](https://aosmd.com/res/data_sheets/AOZ1284PI.pdf)
  -
- [digikey search2](https://www.digikey.ch/en/products/filter/voltage-regulators-dc-dc-switching-regulators/739?s=N4IgjCBcoBwOxVAYygMwIYBsDOBTANCAPZQDaIAzAJwBsNArCALqEAOALlCAMrsBOASwB2AcxABfQmABMVRCBSQMOAsTIhpABjA1p0kIS1h6VACwHwcaTCoULYKzVNypcGprjnXMaQwt64MCp9Q016GHo7UJ8KGH9TODhNGn8KaxoEQzpfCEN6WV1-GA9NOMNbKy9KTVMbFxBK0wgWEA4uXkFRCUJ6ODtoBTQsPEISSHIwTQp6UxCQKhgwagsFimd7Us85ydNTChTCDOptDdrTRikZdwvwHXpNeqOqTTmnqfsljM0PqkDMkDeVTADmkpm%2BUj2xSiDRgFDA53spgYOkRCQo4PAuzoN3hZjWiKs03s9DANjK4HoJNhxOCCXsNHhDhpEW2Jio9AO4HcHnqOjWgXp4UKUgyvSBMHOC3sMCck2lMEWvOeJIxQSMOLMEqBvwoSXsCzgtH8mheMmNxh8-mM6Ju0lJyX%2BduCZsMegocP80jWvWYbE4kB4-GEYkk4AecQGimUIzU40oXoZCBabQDHWD3RAAFp9JGoPwAK6qMbkRhMcShnPkABG%2BaQAGtfQD5AIACZcTOTOYpkAWdgAT1YuC4LewKHLQA)
- [digikey search](https://www.digikey.ch/en/products/filter/voltage-regulators-dc-dc-switching-regulators/739?s=N4IgjCBcpgTAnFUBjKAzAhgGwM4FMAaEAeygG0RYAWAdhoAYA2EI6gDjkVardgGZm3XlSotKfWGwZj%2BkxjRmNGkhayWxGENYz40tlJX3ixFjKmACsY9WcGULCZTLb0GbRfBdXWDcydbwunyiAbrUMoFsjO6sbEbBYnz0PPBcIDTUGWIZIvo0fGA6YlG8drzJ3iDwSppiYPSwFmD6NTS0da5mMeCWbGwhPRbw5nVUsMmIALpEAA4ALlAgAMpzAE4AlgB2AOYgAL5EFvlIIKiQmLiEJOTg9HwWY6N8LnzZcWBUlR%2BMTXYftElRlQlF8qMMEkQPhl7nUmmxPLC4a9IUNMnVNOZVOAhmwHLDUhY-ox6K40oVgnp0RYov5wP0cXUouZ6Iy%2BmAyfB6E0WZDjPRLHVhv0Buz8tJeVJqjISbA4DJLElKrKXPJ5cY5axYPwCjJ%2BJ8sVrNOFWM98t1qEZCTJgWA%2BMjKMDUvpGjQzLSXUpzdSjGlGtVHmowHEeQYkgifBwojIaH0ms5XbpEg15N1GNUENlbVR7f1JGlhjRqR0mJTpiB5osVhsdvsiABaEzQU5QNYAVyupEgFCskz2B0oNwARq3kABrEBl5hN9YAE0WdfqtIrkBAYjmAE8ZnhFjOcKg%2B0A)
- [AP64501](https://www.diodes.com/assets/Datasheets/AP64501.pdf)
  - 5A, Vin max 40V
  - Efficiency 90%

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
  - [SY8303](https://datasheet.lcsc.com/lcsc/1811151653_Silergy-Corp-SY8303AIC_C97054.pdf)
    - Buck converter input up to 40V
    - It's the one Phil uses

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

# Projects
- [PD Micro](https://www.crowdsupply.com/ryan-ma/pd-micro)
  - Uses PHY USB-C controller FUSB302, not all-in-one PD IC like STUSB4500
  - [Hackaday](https://hackaday.com/2021/01/16/usb-c-programmable-power-supply-for-any-project/)
  - [GitHub](https://github.com/ryan-ma/PD_Micro)
- [ZY12PDN](https://github.com/manuelbl/zy12pdn-oss)
  - Open source firmware for common PD trigger board ZY12PDN
    - FUSB302B IC
    - STM32F030F4 MCU
    - [Hardware analysis](https://github.com/manuelbl/zy12pdn-oss/wiki/Hardware-analysis)
  - Uses PPS fallback if fixed voltage not supported!
- [Brian Lough's PD trigger boards review](https://www.youtube.com/watch?v=iumAnPiQSj8)
  - [His post on IronOS](https://github.com/Ralim/IronOS/issues/24#issuecomment-651659470)
- [PicoPD](https://hackaday.io/project/192576-picopd-usb-c-pd-30-pps-trigger-with-rp2040)
  - PPS and PD capable
  - Uses the [AP33772](https://www.diodes.com/part/view/AP33772)
    - Capable of both PD and PPS!!
- [Phil's lab USB-C Power Delivery hardware design](https://www.youtube.com/watch?v=W13HNsoHj7A)
  - Uses [CYPD3177](https://www.infineon.com/dgdl/Infineon-EZ-PD_BCR_Datasheet_USB_Type-C_Port_Controller_for_Power_Sinks-DataSheet-v03_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0ee7ce9d70ad)
    - Does not support PPS, but [CYPD3171](https://www.infineon.com/dgdl/Infineon-EZ-PD(TM)_CCG3PA_Datasheet_USB_Type-C_Port_Controller-DataSheet-v09_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0ee438366ac0) does
    - Has built-in overvoltage protection and short to vbus protection

# PD / PPS ICs
- [STUSB4500](https://www.st.com/en/interfaces-and-transceivers/stusb4500.html)
  - The one I used
  - [datasheet](https://www.st.com/resource/en/datasheet/stusb4500l.pdf)
  - No PPS, but [alternatives](https://community.st.com/t5/interface-and-connectivity-ics/how-do-i-configure-the-stusb4500-as-a-pps-sink/m-p/251067/highlight/true#M3828)
- [STUSB1602](https://www.mouser.ch/datasheet/2/389/stusb1602-1852125.pdf)
  - [datasheet](file:///Users/rbaron/Downloads/stusb1602%20(1).pdf)
  - PPS support
  - Overvoltage protection and short to vbus protection
  - [P-NUCLEO-USB002](https://www.st.com/en/evaluation-tools/p-nucleo-usb002.html)
    - STM32 + STUSB1602 dev board
    - $60 on digikey
- [FUSB302](https://datasheet.lcsc.com/lcsc/1809111642_onsemi-FUSB302BMPX_C132291.pdf)
  - Popular, lower level
  - [OSS library](https://github.com/Ralim/usb-pd)
  - How does it work?
    - [BMC](https://en.wikipedia.org/wiki/Differential_Manchester_encoding) Power Delivery
      - Interface between the CC pins and the MCU through i2c
      - [BMC](https://en.wikipedia.org/wiki/Bi-phase_mark_code)
  - Code on [chrome-os](https://github.com/coreboot/chrome-ec/blob/master/driver/tcpm/fusb302.c)
- [CYPD3175](https://jlcpcb.com/partdetail/3338697-CYPD317524LQXQT/C2952419)
  - For source side?
  - [datasheet](https://www.infineon.com/dgdl/Infineon-EZ-PD(TM)_CCG3PA_Datasheet_USB_Type-C_Port_Controller-DataSheet-v09_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0ee438366ac0)
  - PD & PPS
  - Built-in Arm M0
  - Built-in overvoltage protection and short to vbus protection
- [CYPD3177](https://www.infineon.com/dgdl/Infineon-EZ-PD_BCR_Datasheet_USB_Type-C_Port_Controller_for_Power_Sinks-DataSheet-v03_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0ee7ce9d70ad)
  - Easy to setup in auto mode with fixed voltage
  - Also supports i2c interface -- see [BCR HPI Guide](https://www.infineon.com/dgdl/Infineon-EZ-PD_BCR_Host_Processor_Interface_Specification-Software-v01_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0f8c4313766b&da=t)
- [CYPD3176](https://www.infineon.com/dgdl/Infineon-CYPD3176-24LQXQ-DataSheet-v01_00-EN.pdf?fileId=8ac78c8c81ae03fc0181d79a2e4b67e2)
  - Unfortunately, documents related to HPI are only released under NDA. [link](https://community.infineon.com/t5/USB-EZ-PD-Type-C/CYPD3176-HPI-Host-Processor-Interface-Specification-for-PPS-control/m-p/324119#M6122)
- [IP2721](https://datasheet.lcsc.com/lcsc/2006111335_INJOINIC-IP2721_C603176.pdf)
  - Brian Lough uses in his trigger board (see [Github issue comment](https://github.com/Ralim/IronOS/issues/24#issuecomment-651659470))
  - Standalone, single resistor to set fixed voltage
- [STM32 chips](https://www.st.com/content/st_com/en/ecosystems/stm32-usb-c.html)
  - Some STM32 chips have integrated USB-C PD support
    - [Presentation](https://www.st.com/content/dam/ecosystems/stm32-usb-c/microcontrollers-stm32usb-c-pd-solutions-ove-babacarnguirane.pdf)
    - [STM32G071](https://datasheet.lcsc.com/lcsc/2304140030_STMicroelectronics-STM32G071RBT6_C432213.pdf)
      - $2 on JLC
    - STM32G0x1 support PD 3.0 + PPS (via BMC + PHY?)
  - [Getting started with USB-C And STM32G0 ecosystem](https://www.youtube.com/watch?v=-PjgWTHzGqM) by ST
  - [Video from ST](https://www.youtube.com/watch?v=-vsJhNIaHxE)
  - [STM32G071B-DISCO](https://www.digikey.ch/en/products/detail/stmicroelectronics/STM32G071B-DISCO/9925245?s=N4IgTCBcDaIMoBUCyBmMBxADAdgIwCEQBdAXyA)
    - USB-C PD board analysis
  - [X-NUCLEO-SNK1M1](https://www.st.com/en/ecosystems/x-nucleo-snk1m1.html)
    - Expansion board for nucleo boards that support UCPD (USB C Power Delivery)
    - 100 W solution supporting Programmable Power Supply (PPS) function
    - $25 on [digikey](https://www.digikey.ch/en/products/detail/stmicroelectronics/X-NUCLEO-SNK1M1/13971814?s=N4IgTCBcDaIBoFoByBVAwgGQKIHkEGUkBpARgFkSQBdAXyA)
    - Uses the [TCPP01-M12](https://datasheet.lcsc.com/lcsc/2211071530_STMicroelectronics-TCPP01-M12_C1121848.pdf) protection IC
      - It actually does the load switching?
    - Compatible nucleo boards: NUCLEO-G071RB, NUCLEO-G474RE and NUCLEO-G0B1RE
      - NUCLEO-G071RB [$10 on digikey](https://www.digikey.ch/en/products/detail/stmicroelectronics/NUCLEO-G071RB/9739925?s=N4IgTCBcDaIHIFUDCAZAogeQLQHEAMA7AIwBKAQiALoC%2BQA)
        - Target: [STM32G071RB](https://www.st.com/en/microcontrollers-microprocessors/stm32g071rb.html)
          - Has USB-C PD support
          - $2 on [JLC](https://jlcpcb.com/parts/componentSearch?searchTxt=STM32G071RB)
          - [Reference manual](https://www.st.com/resource/en/reference_manual/rm0444-stm32g0x1-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
            - Chapter 38: USB Type-C™ / USB Power Delivery interface (UCPD)
              - This is the info I WANTED!!!!
              - How to receive / transmit PD messages
              - 38.7.16 UCPD register map
          - [Reference design](https://www.st.com/resource/en/application_note/an5096-getting-started-with-stm32g0-series-hardware-development-stmicroelectronics.pdf)
          - [Explanation of USB pins](https://www.st.com/resource/en/product_training/STM32G0-Peripheral-USB-Type-C-Power-Delivery-UCPD.pdf)
  - Zephyr + STM32
    - [Zephyr USB-C Sink](https://docs.zephyrproject.org/latest/samples/subsys/usb_c/sink/README.html)
    - [nucleo_g071rb](https://github.com/zephyrproject-rtos/zephyr/tree/main/boards/arm/nucleo_g071rb)
      - [.dts] does not enable ucpd1?
      - but it's available at the SoC's [stm32g071.dtsi](https://github.com/zephyrproject-rtos/zephyr/blob/d34f725df81852b468dba2727a4d569022cb907d/dts/arm/st/g0/stm32g071.dtsi#L13)

# Docs
- [PD & PPS by Infineon](https://www.infineon.com/dgdl/Infineon-Universal_USB-C_Charger_Designs_Enabled_by_Programmable_Power_Supply-Whitepaper-v01_00-EN.pdf?fileId=5546d4627aa5d4f5017af5d1d5131db9&da=t)
  - Discusses PPS, PD
    - PD 2.0 introduces fixed voltages
      - Source sends Power Delivery Obj (PDO) messages with its capabilities
      - Sink selects one
    - PD 3.0 introduces PPS
      - Source sends message with its min/max voltage and current via Advanced Power Delivery Obj (APDO)
      - .. In addition to old 2.0 PDO
- [Low level PD @ hackaday](https://hackaday.com/2023/02/14/all-about-usb-c-talking-low-level-pd/)
  - Discusses PD 2.0?
  - Using the FUSB302 chip
  - Author references the [usb-pd lib](https://github.com/Ralim/usb-pd/tree/main/src/) used in IronOS (OSS soldering iron firmware?)
- [Protection for USB-C circuits](https://www.ti.com/lit/wp/slyy105/slyy105.pdf?ts=1700336340102&ref_url=https%253A%252F%252Fwww.google.com%252F)


# USB-C Protection ICs
- [TCPP01](https://www.st.com/resource/en/datasheet/tcpp01-m12.pdf)
  - Used in the nucleo boards
- [TPD8S300](https://www.ti.com/lit/ds/symlink/tpd8s300.pdf?ts=1700336435213&ref_url=https%253A%252F%252Fwww.google.com%252F)
  - $1 at JLC
- [NX20P0477](https://www.nxp.com/docs/en/data-sheet/NX20P0477.pdf)

# Ideas
## Fixed PD 2.0 LED driver
  - 5V (for some USB LED strips / lights) or 12V (for LED strips, but some adapters don't support 12V)
  - Max current A amps
  - Single channel
  - WIll use old ESP32 modules I have laying around
    - [Module ESP32­WROOM­32D](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32d_esp32-wroom-32u_datasheet_en.pdf)
    - [MCU datasheet](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)


# Chargers
## Small
- [Anker 313 Charger (Ace, 45W)](https://ankertechnologycompanyltd.my.salesforce.com/sfc/p/#5g000004DkWQ/a/5g000000gl2x/FfrEZyfYeV9sbibH8bd240NGukjA4ueyrzGEvX6Izq0)
  - $15 Black Friday
  - No 12V fixed
  - But does PPS 3.3V - 16V @ 3A
- [Anker Nano II 45W](https://ankertechnologycompanyltd.my.salesforce.com/sfc/p/#5g000004DkWQ/a/5g000000gIGl/o1f7o_kwmjfOJY.g2D_vWGIs_UWesHygdUO1opcu_EE)
  - $27
  - No 12V fixed
  - But does PPS 3.3V - 16V @ 3A
- [Anker 511 Charger (Nano 3, 30W)](https://cdn.shopify.com/s/files/1/0595/4034/0926/files/A2147_User_Manual.pdf?v=1664433371)
  - $20
  - No 12V fixed
  - But does PPS 3.3V - 16V @ 2A
- [Anker PowerPort III Pod 65W](https://ru.anker.com/upload/iblock/ef9/y89gi14yuq3fje76wizdko6uvebsrp2o/A2712_PowerPort%20III%2065W%20Pod.pdf)
  - The one I have
  - No fixed 12V
  - Seems to do PPS 3.3V - 21V @ 3A
- [Anker Prime 100W (A2343)](https://ankertechnologycompanyltd.my.salesforce.com/sfc/p/#5g000004DkWQ/a/5g000000gtSj/dTjkYk9rz10PL6VbDI9kjnFpAX6eNBTsteTWvMzwyGw)
  - The one I have
  - Fixed 12V @ 3A
  - No PPS listing, but says in the bottom that it does support PPS?


# Dev Containers
- [zmk](https://github.com/zmkfirmware/zmk)
  - Uses their own [Docker image](https://github.com/zmkfirmware/zmk-docker/blob/3.2-branch/Dockerfile)


# STM32 dev board
- [nucleo_g071rb](https://www.st.com/en/evaluation-tools/nucleo-g071rb.html)
  - [User manual](https://www.st.com/resource/en/user_manual/dm00452640-stm32-nucleo-64-boards-with-stm32g07xrb-mcus-stmicroelectronics.pdf)
  - [Pinout](https://os.mbed.com/platforms/ST-Nucleo-G071RB/)
  - [Zephyr board](https://docs.zephyrproject.org/latest/boards/arm/nucleo_g071rb/doc/index.html)
    - `nucleo_g071rb`
  - [How to build a simple USB-PD sink application](https://www.mouser.com/pdfDocs/enDM006635111.pdf)
  - [Introduction to USB Type-C® Power Delivery for STM32](https://www.st.com/resource/en/application_note/dm00536349-usb-type-c-power-delivery-using-stm32xx-series-mcus-and-stm32xxx-series-mpus-stmicroelectronics.pdf)
    - Great resource with overview of USB-C PD / PPS
  - [Middleware USBPD Device G0](https://github.com/STMicroelectronics/stm32-mw-usbpd-device-g0)

## Building
```
ZEPHYR_BASE=~/dev/zephyr/zephyr west build -b nucleo_g071rb
```

## Flashing
With openocd:
```
openocd -f interface/stlink-v2.cfg  -f target/stm32g0x.cfg -c "program build/zephyr/zephyr.elf" -c "reset run" -c "shutdown"
```

## EZ - PD
- [Comparison](https://www.rutronik.com/electronic-components/infineon/moving-to-usb-c-with-infineons-ez-pdtm-family)
- BCR
  - [datasheet](https://www.infineon.com/dgdl/Infineon-EZ-PD_BCR_Datasheet_USB_Type-C_Port_Controller_for_Power_Sinks-DataSheet-v03_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0ee7ce9d70ad)
  - CYPD3177
- BCR-Plus
  - [datasheet](https://www.infineon.com/dgdl/Infineon-CYPD3176-24LQXQ-DataSheet-v01_00-EN.pdf?fileId=8ac78c8c81ae03fc0181d79a2e4b67e2)
  - Offers PPS
  - CYPD3176
-
- BCR-Lite

### HPI
- HPI for BCR-Plus and BCR-Lite
  - [specification](https://www.infineon.com/dgdl/Infineon-EZ-PD_TM_BCR-PLUS_BCR-LITE_HOST_PROCESSOR_INTERFACE_SPECIFICATION_CYPD3176_CYPD3178-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8afe5bd0018b1d4908804b9e)
  - There's a PPS Request inside SINK_PDO_REQUEST
- HPI for BCR
  - [specification](https://www.infineon.com/dgdl/Infineon-EZ-PD_BCR_Host_Processor_Interface_Specification-Software-v01_00-EN.pdf?fileId=8ac78c8c7d0d8da4017d0f8c4313766b)


## USB-C PD
- Normal request
  - Page 259
- PPS request
  - Table 6.26 page 160
- PPS timers / keep alive
  - Page 290
  - Enough to keep requesting a PPS APDO as a keep alive every
    - tPPSRequest
      - Max 10s. We can do 5s
  - Page 491 has "steps for SPR PPS Keep Alive"
- Examples of messages
  - Changing PDOs, page 383