Why bootloader is important?
It allow the firmware update to the flash or EEPRAM with the serial interface online, you can set the bootloader load the  program in flash or EEPRAM. The customer don't have to send the mcu to you to update, they can download the firmware from internet and update automatic themself.

How Arduino upload the code in MCU with USB?
Why USB only support load application flash, not bootloader? how the fluse bite(control the programming mode) changed with the ICSP interface(it can programming all flash)?

After check the *Arduino Uno Rev3 schematic*. There are two mcus:Atmega16u2 and Atega328P. The Atmega16u will convert the UART to USB. The 3 singal line are import(only 3), the Rx and Tx(transfer the data), and the *DTR* (USB boot EN, to enter the programming mode), the DTR will reset the Atmega328P to enter the Programming mode, then it will program the Atmega328P flash.


Where the source code can find with the DTR control?
You can find the DTR reset code at Arduino-usbserial.c in the end of SetupHardware:

	/* Pull target RESET line high */
	AVR_RESET_LINE_PORT |= AVR_RESET_LINE_MASK;
	AVR_RESET_LINE_DDR  |= AVR_RESET_LINE_MASK;

You can also update the Atmega16u firmware, the Arduino-usbdfu is the bootloader code of Atmega16u, the Arduino-usbserial is the application to deal with the UART conver to USB and programming the Atmega328p.

How to entering programming mode refer to the datesheet *28.7.1 Memory programming*.

Atmega16u2 只负责UART 到USB 数据的转换发送,数据解析以及解析后使用stk500编程属于Atega328p负责
