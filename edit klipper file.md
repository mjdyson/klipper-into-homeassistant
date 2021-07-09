# Edit klipper.yaml

Start of the file needs to contain
`#klipper.yaml`

Indentations are really important. The first section here has a single tab indent.

In the following section I'll demo and try to explain what I think is going on (p.s. happy to be corrected by someone that knows better).
I'm going to assume your HA server is running at 192.168.1.10 - replace this with whatever IP addresss your HA server has.

```
    - platform: rest
      name: voron_extruder
      resource: "http://192.168.1.10:7125/printer/objects/query?extruder"
      value_template: "OK"
      json_attributes_path: "$.result.status.extruder"
      json_attributes:
        - pressure_advance
        - power
        - target
        - temperature
    - platform: template
      sensors:
        voron_hotend_actual:
          friendly_name: 'Hot End Actual'
          value_template: "{{ state_attr('sensor.voron_extruder', 'temperature') | float | round(1) }}"
          device_class: temperature
          unit_of_measurement: '°C'
```
We have 2 different things going on
1) We get the data out of moonraker using a rest sensor called `voron_extruder` with a value template. This is a neat way of storing the data for later processing. Whilst it's possible to include more data in this, I've decided to limit it to only data from the extruder query of moonraker api. I've had issues with the returned json string being too large for HA to handle. Note:
* The `resource:` is the IP address of your klipper / printer server
* The end of `resource:` is what data we're looking for from moonraker (we can change this to other values)
* The `json_attributes_path:` needs to follow a hierarchy structure (see example below)
2) We create a sensor called `voron_hotend_actual` that reads the information from the `voron_extruder` sensor and formats it appropriatel. 
 
Notice that `sensor.voron_extruder', 'temperature'` is referencing the json attribute in the first section. If you want to get different data into another template you need to create a new sensor (from 2 above) and replace `temperature` with `target`. It also needs a different name, e.g. `voron_hotend_target`.

## Other data outputs

As I mentioned above, you can get all sorts of data out of Moonraker using the API. Documentation is available here https://moonraker.readthedocs.io/en/latest/web_api/#list-available-printer-objects

Unfortunately, this bit will be a bit of trial and error for you, but it follows a pattern.

http://192.168.1.10/printer/objects/list

The returned data on my printer is the following:

```{"result": {"objects": ["webhooks", "configfile", "mcu", "mcu z", "heaters", "temperature_host raspberry_pi", "temperature_sensor raspberry_pi", "gcode_move", "print_stats", "virtual_sdcard", "pause_resume", "display_status", "heater_fan hotend_fan", "fan", "heater_fan controller_fan", "heater_fan nevermore", "heater_bed", "probe", "idle_timeout", "quad_gantry_level", "bed_mesh", "menu", "neopixel fysetc_mini12864", "output_pin caselight", "neopixel case_lights_right", "neopixel case_lights_left", "gcode_macro lights_off", "gcode_macro lights_white", "gcode_macro lights_red", "gcode_macro lights_green", "gcode_macro lights_blue", "gcode_macro lights_purple", "gcode_macro G32", "gcode_macro PRIME_NOZZLE", "gcode_macro PRINT_START", "gcode_macro PRINT_END", "gcode_macro PAUSE", "gcode_macro RESUME", "gcode_macro CANCEL_PRINT", "query_endstops", "system_stats", "toolhead", "extruder"]}}```

Notice that one of these items is `extruder` as we've used in the example query of the first sensor.

http://192.168.1.10:7125/printer/objects/query?**extruder**

Replace `extruder` with `heaters`, `fan` or any other value you want to pull into HA. 
If you want to find out what attributes are available, copy the whole query string into your browser.

## Example for heaters

http://192.168.1.10:7125/printer/objects/query?**heaters**

This query returns
```{"result": {"status": {"heaters": {"available_sensors": ["temperature_sensor raspberry_pi", "heater_bed", "extruder"], "available_heaters": ["heater_bed", "extruder"]}}, "eventtime": 4866.209659858}}```

We can therefore use the following attributes from **heaters** in another sensor
* temperature_sensor raspberry_pi
* heater_bed
* extruder
