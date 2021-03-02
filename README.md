# RPi_I2C_MCP23017_DTS_kernel_overlay
Device Tree overlay for Raspberry Pi 2/3/4/Zero exporting MCP23017 labels globally for use by other overlays/code

Overlay for MCP23017 I2C I/O Expander(and based on it boards such as IO Pi Plus). Adds 2 buses- 0x20 and 0x21.
The overlay solves the problem of MCP23017 expanded GPIOs, that can't be targeted by other overlays. The overlay exports MCP23017 labels globally using build-in i2c-gpio overlay.

# Install
## Load i2c-gpio overlay
Add a line in the end of /boot/config.txt

```
dtoverlay=i2c-gpio

```
Uncomment "dtparam=i2c1=on" /boot/config.txt and reboot.

## Load mpc2301_overlay
In directory, where spi-overlay.dts is saved:

```
$ dtc -@ -I dts -O dtb -o mpc2301_overlay.dtbo mpc2301_overlay.dts
$ sudo mpc2301_overlay.dtbo /boot/overlays

```
Add a line after i2c-gpio overlay (i2c-gpio needs to load 1st) in /boot/config.txt and reboot:

```
dtoverlay=i2c-gpio
dtoverlay=mpc2301_overlay

```
Check if i2c devices loaded:
```
$ i2cdetect -y 1

```

# Usage
All the overlays that loads after i2c-gpio overlay targets "&i2c_gpio" label.

```
fragment@0 {
    target = <&i2c1>;

```


