## sync

This shell script makes copying a directory of lua files to the ESP8266 easier/faster.  Requires luatool.py (<https://github.com/4refr0nt/luatool>).  Make sure luatool.py is in your path.

## Binaries

These binaries are from <https://nodemcu-build.com/> or ones that have been found on the internet.  These are here for our convenience (and in case the service dies, or we can't wait).  It's probably best to use the above service to build your own newer versions to suit your needs as these may contain bugs (most are dev versions).

1. nodemcu binaries and the libraries they include...
    * nodemcu-dev-15-modules-2017-04-08-22-29-17-float.bin
        * adc, bit, enduser_setup, file, gpio, http, net, node, ow, rotary, spi, tmr, u8g, uart, wifi
        * u86: 128x128
    * nodemcu-dev-11-modules-2017-03-11-22-16-21-float.bin (1-Wire)
        * adc, file, gpio, http, net, node, ow, pwm, tmr, uart, wifi
    * nodemcu-dev-9-modules-2016-10-10-05-22-36-float.bin
        * adc, dht, file, gpio, net, node, tmr, uart, wifi
    * nodemcu-dev-8-modules-2016-12-30-06-18-47-float.bin  (Small MQTT bin)
        * file gpio mqtt net node tmr uart wifi
    * nodemcu-master-7-modules-2016-09-01-04-30-49-float.bin
    * nodemcu-master-7-modules-2016-09-01-04-30-49-integer.bin
    * nodemcu-dev-7-modules-2016-09-13-06-51-00-float.bin
    * nodemcu_float_0.9.6-dev_20150704.bin
1. non-nodemcu binaries
    * esp_init_data_default.bin
    * v1111ATFirmware.bin

## Everything below here are random notes
```
miniterm.py /dev/ttyUSB1 74880
miniterm.py /dev/ttyUSB2 115200
```

#### Worked...
```
/usr/bin/python2.7 ~/bin/luatool-master/luatool/luatool.py --port /dev/ttyUSB0 -b 115200 --src init.lua 

esptool.py --port /dev/ttyUSB2 write_flash --flash_mode dio --flash_size 32m 0x0 v1111ATFirmware.bin

esptool.py --port /dev/ttyUSB0  write_flash 0x00000 nodemcu_float_0.9.6-dev_20150704.bin

esptool.py --port /dev/ttyUSB0 write_flash --flash_mode dio --flash_size 32m 0x00000 nodemcu-master-7-modules-2016-09-01-04-30-49-float.bin 0x3fc000 esp_init_data_default.bin
```

#### Wed 09 Nov 2016 08:56:25 PM MST
```
/usr/bin/python2.7 ~/bin/luatool/luatool/luatool.py --port /dev/ttyUSB0 -b 115200 --src main.lua
miniterm.py /dev/ttyUSB0 115200
```
#### Mosquitto / MQTT On Sonoff
```
esptool.py --port /dev/ttyUSB0 write_flash --flash_mode dio --flash_size 32m 0x00000 nodemcu-dev-8-modules-2016-12-30-06-18-47-float.bin 0x3fc000 esp_init_data_default.bin
mosquitto_pub -t /home/1/bugzapper -m "ON"
mosquitto_pub -t /home/1/bugzapper -m "OFF"
```
#### March 2017 Session
It seems like we need to flash devkit devices with v1111ATFirmware.bin before the device will accept a nodemcu binary.  So for a new devkit device, it goes something like this...
```
esptool.py --port /dev/ttyUSB2 write_flash --flash_mode dio --flash_size 32m 0x0 v1111ATFirmware.bin
esptool.py --port /dev/ttyUSB0 write_flash --flash_mode dio --flash_size 32m 0x00000 nodemcu-master-7-modules-2016-09-01-04-30-49-float.bin 0x3fc000 esp_init_data_default.bin
miniterm.py /dev/ttyUSB2 115200
```
... then ...
1. reset the device
1. watch it create it's file system
1. escape out when done
1. Copy init.lua to the device via luatool (or sync) so that it doesn't enter a tight bricked loop of looking for the init.lua script.
