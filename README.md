# TPMS_SX1262
The TPMS board is designed to measure the pressure of the 4 wheels of a race car. It is designed to communicate with the Telemetry system on the car. Visit the ```Telemetry_SX1262_STM32``` repository for further documentation. 

The file ***TPMS-Report.pdf*** contains a brief description of the Hardware, the Embedded code and the communication policy used.

The code is written for the STM32 MCU family (STM32L412)
TPMS_Code includes SX1262_Radio_Transceiver Drivers (Based on the Semtech drivers) to transmit the sensor measurements wirelessly.


The custom libraries are included in the following paths:
- TPMS_Code/Core/Inc
- TPMS_Code/Core/Src


The SX1262 Drivers are:

* ```SX1262.c``` / ```SX1262.h``` : They include the basic structs / functions for the operation of the SX1262. They can be used by any MCU.

* ```SX1262_hal.c``` / ```SX1262_hal.h``` : They include low level functions pertaining the SPI communication with the HAL libraries. They can be used by any STM32.

* ```SX1262_board.c``` & ```SX1262_board.h``` : They include functions that have to be initialized according to the specific PCB design of each application.


Also, there are other libraries concerning the communication of the temperature-pressure sensor:

* ```PressSens.h``` / ```PressSens.c``` : Include the functions to initialize and read from the digital sensor MS5837-30BA. Visit the ```MS5837-30BA_Temperature_Pressure_Sensor``` repository which includes only these libraries.


Since the radio transceiver is always the SX1262, the same libraries are used for the Telemetry System and the TPMS. 

In order to differentiate between the different functionalities, the variable ```FUNCTION``` inside the ```SX1262-board.h``` file has to be defined. The possible values for a TPMS are:

* ```TPMS_DEBUG```: The code is intended for the TPMS MCU (STM32L412). The consumption is not down to minimum because diagnostic / debugging functions are still running
* ```TPMS_RELEASE```: The code is intended for the TPMS MCU and the consumption is brought down to absolute minimum. Only essential functions run



## STEPS TO KEEP POWER CONSUMPTION TO MINIMUM
* Internal DC-DC converter at SX1262 used
* Use of internal oscillator in the radio transceiver when possible
* STM32L4 used at a clock frequency of 1MHz from MSI and Low Power Run mode when running
* Stop2 mode at STM32L4 with all peripherals de-initialized and Sleep at SX1262 when system enters sleep mode
* Use of RTC and Low Power timer to control the sleep mode of the system
* Radio Tx power brought down to 0dBm
* Tx packet has a minimum data length of 5 Bytes
* Minimized time in Rx windows
* Minimization of SPI commands (STM32L4 <----> SX1262)
* Sampling time of the sensor reduced to lowest possible
* Data from the RH/T sensor are transmitted without calibration (Telemetry calibrates them)
* RH/T sensor supplied by MCU only when measuring

* Use of 0.22F Super Capacitor to supply pulse currents and reduce stress on the TPMS battery (CR2032W used) 
