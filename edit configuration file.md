# Edit configuration.yaml

Now that you have your klipper.yaml file ready to be read, you need to make the link to it from within the configuration file

1) Open the configuration file for editing 

`nano configuration.yaml`

2) Scroll to the bottom of this file and add the lines

`sensor: !include klipper.yaml`

3) Save the file by using key-combo `ctrl + x`, type `y` (to write to file) and press enter to save
4) If you have permission problems here, restart this page from step 1 but open the file using sudo

`sudo nano configuration.yaml`
