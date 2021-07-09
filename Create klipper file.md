# Create Klipper.yaml

First it is worth explaining (and understanding) that HA uses a file called configuration.yaml to configure user directed changes. Whilst you are able to keep adding to this file, you may wish to split out some aspects of the configuration into separate files - for ease of review down the line.
This guide assumes that you would like to do this. Take note of what is going on in the image below. 

The new klipper.yaml file is acting as an indented part of the main configuration file - you need to make sure that configuration.yaml has a section called `sensor:` which can then be pointed at your new file. The new file (in our case is klipper.yaml) then assumes it is embedded in the main configuration.yaml file, so you shoud not add `sensor:` in this file too.

![alt text](https://github.com/mjdyson/klipper-into-homeassistant/blob/main/images/Extra%20Config%20Files.png?raw=true)

My preferred approach to creating and editing files is via an SSH terminal. You may have other preferred methods - these should be ok too.
In my examples, HA is running on a Raspberry Pi 3+, so the command line is bash.

1) SSH into your HA server and navigate to the config folder of your HA setup
2) Create a file called klipper.yaml
  
`mkdir klipper.yaml`

If you see access permissions, you may need to use sudo

`sudo mkdir klipper.yaml`
