earthq
======
```
from IO7FuPython import Device, ConfiguredDevice 
import json, time
import uComMgr32
from machine import Pin

sensor_pin = Pin(15, Pin.IN, Pin.PULL_UP)

sensor_enabled = True

nic = uComMgr32.startWiFi('AndroidHotspot3602')
device = ConfiguredDevice()
device.connect()

def on_message(topic, msg):
    handle_ommand(msg)

def handle_command(command):
    global sensor_enabled
    data = json.loads(command)
    if data['command'] == 'switch':
        sensor_enabled = data['state'] == 'on'



while True:
    device.loop()
    if sensor_pin.value() == 1:
        status = 'earthquake'
    else:
        status = 'not'
    device.publishEvent('status', json.dumps({'d': {'EQ' : status}}))

    
    time.sleep(1)



```
### About

This is your project's README.md file. It helps users understand what your
project does, how to use it and anything else they may need to know.
