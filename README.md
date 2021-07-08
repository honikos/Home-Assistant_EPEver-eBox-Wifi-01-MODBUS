# Home-Assistant EPEver eBox-Wifi-01 using MODBUS over TCP
How to extract data from the EPEver TRACER-AN using the eBox-Wifi-01 with Home Assistant.
+ Make sure you're able to connect your eBox-Wifi-01 to your WiFi network, so your Home Assistant can reach it!
+ Follow my quick guide here: https://github.com/gabrielpc1190/eBox-Wifi-01-hacks
+ Some of the registers that worked came from this PDF: http://www.solar-elektro.cz/data/dokumenty/1733_modbus_protocol.pdf
+ More info on registers available: https://files.i4wifi.cz/inc/_doc/attach/StoItem/7068/MODBUS-Protocol-v25.pdf

# modbus (inclusive sensor) integration section on configuration.yaml ("new style" as announced in homeassistant os 2021.4, the “old style” YAML was deprecated and now removed for homeassistant >= 2021.7)
Here is my working home-assistant yaml code for EPEver-eBox-Wifi-01-MODBUS for Tracer-AN
```
#EPEver eBox-Wifi-01 modbus
modbus:
  - type: tcp
    host: 172.16.10.98
    port: 8088
    name: hub1
    delay: 1
#  - type: serial
#    name: hub2
#    method: rtu
#    port: /dev/ttyUSB0
#    baudrate: 115200
#    stopbits: 1
#    bytesize: 8
#    parity: N
  - type: rtuovertcp
    host: 172.16.10.98
    port: 8088
    name: hub1
    sensors:
    - name: EPEver_Temperatur 
      device_class: temperature
      unit_of_measurement: C°
      slave: 1
      address: 12560
      input_type: input
      scale: 0.01
      precision: 2
    - name:  Spannung_Batterie_V # EPEver   #331A
      device_class: voltage
      unit_of_measurement: V
      slave: 1  # slave, which request, 247 possible
      address: 13082
      input_type: input
      scale: 0.01
      precision: 2
    - name: Batterie_Ladestand_SOC #311A State of Charge — hier wohl aus Volt berechnet??
      device_class: battery
      unit_of_measurement: Percent
      slave: 1
      address: 12570
      input_type: input
    - name: PV_Leistung_W # 3102 (L) und 3103 (H)
      device_class: power
      unit_of_measurement: W
      slave: 1
      address: 12546
      input_type: input
      scale: .01
      count: 2
      precision: 2
      swap: word
    - name: PV_Eingangsspannung #3100
      device_class: voltage
      unit_of_measurement: V
      slave: 1
      address: 12544
      input_type: input
      scale: 0.01
    - name: PV_Strom_A # 3101
      device_class: current
      unit_of_measurement: A
      slave: 1
      address: 12545
      input_type: input
      scale: 0.01
      precision: 2
```
# More registers for future testing:

see docs / links, mentioned in the head section
