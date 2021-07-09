# Edit klipper.yaml

Start of the file needs to contain
`#klipper.yaml`

Indentations are really important. The first section here has a single tab indent.

``` - platform: rest
    name: voron_extruder
    resource: "http://192.168.1.10:7125/printer/objects/query?extruder"
    value_template: "OK"
    json_attributes_path: "$.result.status.extruder"
    json_attributes:
      - pressure_advance
      - power
      - target
      - temperature```
      
```  - platform: template
        sensors:
          voron_hotend_actual:
            friendly_name: 'Hot End Actual'
            value_template: "{{ state_attr('sensor.voron_extruder', 'temperature') | float | round(1) }}"
            device_class: temperature
            unit_of_measurement: '°C'```
