# swarmanoid
Avengers, Assemble!!!

# Installation

Use nix package manager with `flakes` enabled to utilize the power of declerative configuration `flake.nix`

If you are not using `nixOS`:

1. First install the nix package manager from the link below:
(It should be as easy as pasting the command and following the script.)

https://nixos.org/download

2. Run the given script `enable-flakes-non-nixos.sh` with superuser permissions:

```
# With great power comes great responsibilities.
sudo ./etc/enable-flakes-non-nixos.sh
```

3. Run `nix develop`. For the first time it will take some time to install `opencv`, because it should be compiled manually with the `enableGTK2` flag to displaying windows using the `opencv` library itself. The environment will be loaded instantly upon running `nix develop` the next time.

4. `help` to see the help text.

> Note: Since, the development is done in the nix environment you should enter the environment each time running `nix develop` if you exit or closed the terminal.

> using `flake.nix` to add the dependencies and environment variable is highly recommended.

# Setting up esp8266

Using esptool.py you can erase the flash with the command:
```
esptool.py --port /dev/ttyUSB0 erase_flash
```
And then deploy the new [firmware](https://micropython.org/download/ESP8266_GENERIC/) using:
```
# the following command flashes the firmware that I am using from the `etc` directory in of my repo.
# you might want to flash the latest firmware by downloading it from the link given above.
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 ./etc/ESP8266_GENERIC-20231005-v1.21.0.bin
```

```
# if you cant access the repl even after sucessfully flashing the firmware
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect --flash_mode dout 0 ./etc/ESP8266_GENERIC-20231005-v1.21.0.bin
```

## Connecting to the USB serial repl
```
screen /dev/ttyUSB0 115200
```

> Also checkout [CHEATSHEET](src/micropython/CHEATSHEET.md) for setting up essentials like wifi.

You can also make use of the [webrepl](https://learn.adafruit.com/micropython-basics-esp8266-webrepl/access-webrepl) which is also the primary way of sending files to the client.

To enable the webrepl just `import webrepl_setup` in the serial repl and you're good to go.

## NodeMCU v3 [pins and io](https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/)

![Pin Diagram](etc/ESP8266-Node-MCU.png)

| Label | GPIO   | Input         | Output                | Notes                                                           |
|-------|--------|---------------|-----------------------|-----------------------------------------------------------------|
| D0    | GPIO16 | no interrupt  | no PWM or I2C support | HIGH at bootused to wake up from deep sleep                     |
| D1    | GPIO5  | OK            | OK                    | often used as SCL (I2C)                                         |
| D2    | GPIO4  | OK            | OK                    | often used as SDA (I2C)                                         |
| D3    | GPIO0  | pulled up     | OK                    | connected to FLASH button, boot fails if pulled LOW             |
| D4    | GPIO2  | pulled up     | OK                    | HIGH at bootconnected to on-board LED, boot fails if pulled LOW |
| D5    | GPIO14 | OK            | OK                    | SPI (SCLK)                                                      |
| D6    | GPIO12 | OK            | OK                    | SPI (MISO)                                                      |
| D7    | GPIO13 | OK            | OK                    | SPI (MOSI)                                                      |
| D8    | GPIO15 | pulled to GND | OK                    | SPI (CS)Boot fails if pulled HIGH                               |
| RX    | GPIO3  | OK            | RX pin                | HIGH at boot                                                    |
| TX    | GPIO1  | TX pin        | OK                    | HIGH at bootdebug output at boot, boot fails if pulled LOW      |
| A0    | ADC0   | Analog Input  | X                     |


