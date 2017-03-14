## sync

This is used to make copying a directory of lua files to the ESP8266.  Requires luatool.py.  Make sure luatool.py is in your path.

## Everything below here are random notes...

miniterm.py /dev/ttyUSB1 74880

## Binaries

1. Starndard Libraries Included...
      1. nodemcu-master-7-modules-2016-09-01-04-30-49-float.bin
    1. nodemcu-master-7-modules-2016-09-01-04-30-49-integer.bin
    1. nodemcu-dev-7-modules-2016-09-13-06-51-00-float.bin
1. Others
    1. nodemcu_float_0.9.6-dev_20150704.bin
    1. v1111ATFirmware.bin



1. nodemcu-dev-11-modules-2017-03-11-22-16-21-float.bin (1-Wire)
    1. adc, file, gpio, http, net, node, ow, pwm, tmr, uart, wifi
1. nodemcu-dev-9-modules-2016-10-10-05-22-36-float.bin
    1. adc, dht, file, gpio, net, node, tmr, uart, wifi
1. nodemcu-dev-8-modules-2016-12-30-06-18-47-float.bin  (Small MQTT bin)
    1. file gpio mqtt net node tmr uart wifi

### Worked...
/usr/bin/python2.7 ~/bin/luatool-master/luatool/luatool.py --port /dev/ttyUSB0 -b 115200 --src init.lua 

esptool.py --port /dev/ttyUSB2 write_flash --flash_mode dio --flash_size 32m 0x0 v1111ATFirmware.bin
miniterm.py /dev/ttyUSB2 115200

esptool.py --port /dev/ttyUSB0  write_flash 0x00000 nodemcu_float_0.9.6-dev_20150704.bin

esptool.py --port /dev/ttyUSB0 write_flash --flash_mode dio --flash_size 32m 0x00000 nodemcu-master-7-modules-2016-09-01-04-30-49-float.bin 0x3fc000 esp_init_data_default.bin





#### Wed 09 Nov 2016 08:56:25 PM MST

```
/usr/bin/python2.7 ~/bin/luatool/luatool/luatool.py --port /dev/ttyUSB0 -b 115200 --src main.lua
miniterm.py /dev/ttyUSB0 115200
```

#### Mosquitto / MQTT On Sonoff
esptool.py --port /dev/ttyUSB0 write_flash --flash_mode dio --flash_size 32m 0x00000 nodemcu-dev-8-modules-2016-12-30-06-18-47-float.bin 0x3fc000 esp_init_data_default.bin
mosquitto_pub -t /home/1/bugzapper -m "ON"
mosquitto_pub -t /home/1/bugzapper -m "OFF"

