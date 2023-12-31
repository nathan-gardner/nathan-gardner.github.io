---
layout: project
title: embedded-milestones
---

### Milestone lab assignments for Embedded System Design

This course used the NUCLEO-L452RE STM32 MCU which is part of the [ARM Cortex-M4](https://en.wikipedia.org/wiki/ARM_Cortex-M#Cortex-M4) family. The course used [libopencm3](https://github.com/libopencm3/libopencm3), which is a lightweight hardware abstraction library available on GitHub and maintained by the open source community. Embedded programming for this course was done in C. Milestones build on top of each other throughout the semester, each introducing a new topic from the course. Milestone 4 through Milestone 8 were completed in a group of four students. I tried to explain these milestones in a paragraph each, so a lot of technical details had to be brushed over, more detail can be given on any of the milestones listed below.

#### Milestone 1:

Getting environment setup...

#### Milestone 2:

_Key concepts: UART and hardware timers_


Individually completed milestone using [UART](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter) and ARM Cortex-M4 hardware timers. I wrote system hardware and software initialization code and defined I/O port configurations. This program did not use interrupts as requested by the instructions. This program used a gadfly synchronization technique, which periodically checked flags that indicated if UART data was being received. This program was a [Vigènere cipher](https://en.wikipedia.org/wiki/Vigenère_cipher) programmed to encrypt or decrypt characters sent to the board via the UART port. A supporting circular buffer library was developed for this milestone to store characters in a buffer. The key for this cipher was “TENNESSEETECH” and a button on the board set the operation mode for either encryption or decryption.

#### Milestone 3:

_Key concepts: hardware interrupt implementation with UART commands_


Individually completed milestone extending off the previous UART Vigènere cipher functionality but this time with an additional LED. This program used [interrupts](https://en.wikipedia.org/wiki/Interrupt) as requested by the instructions. The extension for this milestone was the ability to set and read the period of the LED in milliseconds. Two commands were created: !!!L and !!!S####, the first of which replaced !!!L and inserted the period of the LED in the UART stream sent back from the MCU, and the second of which set the period of the LED in milliseconds, with valid inputs being 0001 to 9999 ms. This required a routine to interface data coming in serially through UART and set the hardware timer registers in flight.

#### Milestone 4:

_Key concepts: embedded OS introduction_


This milestone implemented all of the functionality of milestone 3 but his time in a non-preemptive operating system, called [ESOS](https://github.com/jwbruce/esos32), developed by our professor. The OS allowed for threading in a lightweight embedded environment and used a concept similar to threads/ tasks in a traditional operating system. More details on operating system functionality is available in the [ESOS doxygen documentation](http://jwbruce.info/esos32/).
    
System code had either be in hardware/ software initialization or within tasks, and these tasks constantly passed control around in a round-robin fashion. Tasks were created in milestone 4 for receiving UART transmissions, sending UART transmissions, and processing data between the receiving and sending circular buffers using the Vigènere cipher. All of the following milestones used ESOS, so I am not going to explicitly state that for each milestone description following this one.

#### Milestone 5:

_Key concepts: Peripheral Target Board interface_


This milestone was an extension of milestone 4 but this time interfaced with the EduBase-V2 extension board. The EduBase-V2 board added peripherals around the NUCLEO-L452RE including: numerical keypad, four additional push buttons, four seven-segment displays, a buzzer, four additional LEDs, relays, potentiometers, pin headers for I/O, and more. EduBase-V2 board schematic is available [here](../files/BaseBoard_L452.pdf). All ports had to be defined by the group and read and write functions were written for every needed peripheral I/O port and pin combination on the extension board.

The additional complexity of this milestone also came from additional software features, which used all five available push buttons to change program operation states between: encrypt, decrypt, command, lower to upper case, and the removal of non-alphanumeric characters. Program operation was verified for each operation mode, sending UART characters to the board and receiving the correct UART response through serial communication software on a PC. This program implemented the same LED period commands as milestone 4 but his time for each individual LED, all four of which could be queried or set independently through the extension boards 4x4 keypad input in the command state. “Sx,nnnn” set LEDx to a period of nnnn milliseconds, and Lx inquired about the period of LEDx in milliseconds. An additional command was implemented for which Rhhhh would return the 16-bit representation of the state of the keypad where hhhh was a hex bit-mask for the keypads sixteen individual buttons, represented by a sixteen bit binary value. The keypad for the board was scanned to minimize I/O ports and based on the hardware schematic, so a keypad routine was also written for this milestone.

#### Milestone 6:

_Key concepts: simple user interface creation within embedded OS_


This milestone required implementation of a [simple user interface (SUI)](https://en.wikipedia.org/wiki/Simplified_user_interface ). We were tasked with developing a service that would make it simpler for a developer to use and configure I/O. A hardware specific service was created for this milestone which allowed hardware to be abstracted away and simplified interfacing with the boards peripheral devices and I/O. It was also a requirement for this lab to implement all of milestone 5 functionalities with the SUI service developed.

#### Milestone 7:

_Key concepts: Hitachi LCD character module and service_


This milestone was an extension of the previous milestone but this time using the [Hitachi LCD44780](https://en.wikipedia.org/wiki/Hitachi_HD44780_LCD_controller) included on the EduBase-V2 extension board. ESOS has a background service (covered in doxygen linked above) for configuring the LCD, writing characters, and setting the cursor, etc.. This background service was used to interface with the LCD for this milestone. The LCD acted as a user interface where the user could see each LCD period in milliseconds and cycle though the four periods with buttons on the keypad. The user could also still set the period with the keypad where D represented the edit command, set was represented by the * key, and the numbers pressed filled the LCD from right to left. Another new functionality added was that when the device was encrypting or decrypting using the cipher, the LCD screen would be populated with the characters received and sent through UART. The received normal characters were on the top row and the sent encrypted/ decrypted characters were on the bottom row, shifting from left to right.

#### Milestone 8:

_Key concepts: Seven-segment display service and SPI_


This milestone used the [seven-segment displays](https://en.wikipedia.org/wiki/Seven-segment_display), a refresh task which took care of updating the multiplexed display, and user API functions, all of which combined to create a seven-segment service used to display characters with the seven-segment displays on the EduBase-V2 board. API functions were implemented based on instructions for the following: clear screen, write character, write buffer, get display, write string, blink, write u8 decimal, write u8 hex, write i8 decimal, write i8 hex, write u16 decimal, and write u16 hex. The processor’s SPI peripheral was used to interface with the seven-segments for this milestone. The board schematic has a SPI master-slave combination connected to the DIG0-3 and QA-QH which were used to write to the seven-segment displays.
