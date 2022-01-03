# PART AutoCalibration

## One-time SLM Setup

1. For PART to communicate with the SLM, the Webserver on the SLM must be enabled.  Go to `Preferences` -> `Remote Access` and make sure the `Web Server` setting is set to `Enabled`.
2. In `Preferences` -> `Remote Access`, set a `User Name` and `Password` and note them for later.

## One-time Tablet Setup

1. Download [the latest autocalibration build](https://bgcgamefiles.s3.us-east-2.amazonaws.com/PART/Builds/Test/PART_AutoCalibration28.zip) and extract it on the PC you wish to use. _(Updated Jan 3rd, 2021)_
2. Press the small buttopn on the back of the pod that enters it in Bluetooth pairing mode.
3. Add the Pod as a Bluetooth Device for the PC.
    1. Right click the Bluetooth Icon in the task bar and choose `Add a Bluetooth Device` to open the settings window.
    2. Click the large `+ Add Bluetooth or other devies` button at the top of the window to open the "Add a device" dialogue.
    3. Choose `Bluetooth` on the dialogue.
    4. Find and select the Pod, likely labeled `NCRAR-WPOD-#####`.

## Calibration Setup

1. Connect the SLM to a local network, for example by plugging it into a router or switch using an ethernet cable.
2. On the SLM, go to `Preferences` -> `Network` and record the `IP Address`.
3. Make sure the Pod is on. (If all of the LEDs on the front of the Pod are off, then press the small bluetooth button in the back to wake it up.)
4. Make sure the Tablet is currently connected to the Pod with bluetooth.
    1. Right click the Bluetooth Icon in the task bar and choose `Show Bluetooth Devices`
    2. Make sure you see the Pod in the connected devices, under `Audio`.
    3. Click on the name of the Pod and some buttons should appear.
    4. If there is a `Connect` button, press it and make sure the device properly connects. If it doesn't, try removing the device and repairing it.
5. Make sure the Audio Output Device is set properly.
    1. Click the Speaker in the taskbar to expand the audio settings.
    2. Above the volume slider is a label that should say `Headphones (NCRAR-WPOD-##### Stereo)`.  If it doesn't click the label and find this option in the list.
    3. Make sure the volume slider is set all the way up.
6. Open PART and go to the Pod Calibration tab.
7. Press `Begin`.
8. Find and select the COM port associated with the Pod.
    * Typically, the two options are `COM5` and `COM6`. The Pod mounts two, and the one you want is the lower-valued port, `COM5`.
    * Press `Continue` and it will try to connect. The connection doesn't always succeed on the first try, but if you see any lights on the front of the Pod change, you'll know you're on the correct COM port.
9. Enter the connection information for the SLM.
    * If you're using the front port of a type 2270 B&K SLM, then you should select SLM `Channel2`.
    * Enter the SLM `IP Address` noted above.
    * Enter the `UserName` and Password you created for the SLM above.
10. You'll be instructed to prepare the Left headphone. Do so and press `Continue`.
11. The App will now automatically walk through the calibration process.
12. After calibration of the Left channel finishes, you'll be asked to set up the Right headphone.
13. Calibration should run through to the end.
