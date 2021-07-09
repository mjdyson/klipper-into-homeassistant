# klipper-into-homeassistant

This is a simple guide to getting data from your 3d printer, which uses Klipper and moonraker, into Home Assistant.

I'd advise against running HA and klipper on the same computer - I wouldnt want HA to interfere with the real-time-like processing of Klipper.

Links:
* Klipper: https://www.klipper3d.org/
* Moonraker: https://github.com/Arksine/moonraker
* Home Assistant: https://www.home-assistant.io/

## Prerequisistes

1) All three components listed above are installed and operational
2) KNow your printer's IP address
3) Be able to edit yml files on your HA server (you may need sudo, depending on your setup)
4) Understand a sensors in HA (https://www.home-assistant.io/integrations/sensor/)
5) You're ready to go!

## Instructions

1) [Create klipper.yaml](../main/Create%20klipper%20file.md) - this will be used to store the sensor
2) Edit configuration.yaml to reference your new klipper.yaml file
3) Edit klipper.yaml
4) Restart HA and start adding sensors to the UI

## Todo

* To resolve: Currently HA has a few too many errors when the klipper server is not running.
