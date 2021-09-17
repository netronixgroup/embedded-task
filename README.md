Interviewer task: 

![alt_text](images/image1.png "image_tooltip")


The system consists of FAN(12V, 10500 rpm) + [MAX6650 FAN controller](https://datasheets.maximintegrated.com/en/ds/MAX6650-MAX6651.pdf) + Cortex M4 MCU(STM32F4) that is connected to PC through rs232 interface(rs232 to usb converter can be used).

On the PC system has an application that accepts user commands and reflects them to the MCU, MCU then converts the commands to the I2C communication to the MAX6650 IC. The result should be the fan speed changing.

Also there is one extra command that erases the firmware code. Before the command is processed the system  should worry the user about irreversibility of the action and ask for confirmation.

MAX6650 code should be placed in an external library and linked to the firmware code. There are some external functions should be provided by the firmware code: 



* bool setup_i2c(bool fast_speed)
* bool write_i2c(uint16_t slave, uint16_t reg, uint8_t* buf, uint16_t size)
* bool read_i2c(uint16_t slave, uint16_t reg, uint8_t* buf, uint16_t size)

Commands:



* “set_fan_speed,&lt;speed 0..100%>”
    * responds with actual speed or any error
* “get_fan_speed”
    * responds with actual speed or any error
* “self_erase”
    * responds with a worry message about irreversibility of the action and asks for confirmation. After confirming with the user the firmware erases the internal flash. After this firmware responds to all commands with “no functional”.

Use [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) for code building.

Below is the desired target system source code directory structure:



* firmware
    * libs
        * max6650
            * src
                * Makefile
                * max6650.c
            * inc
                * max6650.h
            * out
                * max6650.lib
    * src
        * Makefile
        * main.cpp
        * startup.s
        * linker.ld
    * out
        * max6650_test.elf
        * max6650_test.bin
* _application(is optional if terminal is using like control app)_
    * _control app source code_

Please use github repository to add and modify the source code, please add some short description of the project in the Readme.md
