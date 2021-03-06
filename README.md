# stm32-uart-dfu
Simple command-line program that works with stm32 microcontrollers
uart bootloader.

## Requirements:
pyserial

## Usage:
Available dfu operations:
 - get id: prints mcu id (2 bytes)
 - run: MCU jumps at specified address
 - erase: erase specified size of memory at address
 - dump: dump specified size of memory from address to a file
 - load: load binary file to memory at address.

### For available commands:
```
> python3 uart-dfu.py --help
```

### For specific command help:
```
> python3 uart-dfu.py load --help
```

### Example:
```
> python3 uart-dfu.py --port /dev/ttyUSB0 load --file firmware.bin --erase -m map.json
```

### Memory map file:
Memory map file contains mcu's flash memory sectors information (address, size).
Table with flash memory organization can be found in reference manual in
'embedded flash memory' section.  
json file with memory map example for stm32f407 can be found in memory_map directory.
Basically this file contains list of dicts like that:
``` json
[
  {
    "address": "0x8000000",
    "size": "0x4000"
  },
  {
    "address": "0x8004000",
    "size": "0x4000"
  },
  ...
]
```  
Without memory map only mass erase is available.

### Entering system bootloader
See [STM32 AN2606](https://www.st.com/content/ccc/resource/technical/document/application_note/b9/9b/16/3a/12/1e/40/0c/CD00167594.pdf/files/CD00167594.pdf/jcr:content/translations/en.CD00167594.pdf).
If you are communicating with the MCU using an UART USB adapter, it likely supports flow control signals which can be used reset the MCU and possibly even control BOOT0 pin in order to enter the bootloader.
Once you have these line(s) connected to respective pins, use `--reset-strategy [rtsdtr|dtrrts]` to reset into bootloader before executing any other command.
