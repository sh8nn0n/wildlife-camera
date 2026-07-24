# Journal
## 7/23/26 & 7/24/26
### What I did
I spent an hour or two debugging why the serial monitor kept saying "camera capture failed," only to realize that I forgot to select the right esp 32 board in the other tab...
AT LAST! the camera started taking photos, but instead of taking them only when the infrared sensor detected a person, it was taking them every second: <img width="677" height="222" alt="Screenshot 2026-07-24 at 2 36 04 PM" src="https://github.com/user-attachments/assets/8308e5c1-552b-439c-97b1-d845ff455b55" />

So I went back into the code to define a sleep and wake up sequence, where once the PIR sensor detected a living being, it would tell the camera, "WAKE UP!," and the camera would come alive and snap a photo. Otherwise, it would just be asleep. This helped save battery when I want to take this project outside to take photos.
When the PIR sensor activates, the camera knows to take a photo now because I've passed this command at the beginning of the setup <img width="328" height="32" alt="Screenshot 2026-07-24 at 3 04 37 PM" src="https://github.com/user-attachments/assets/c1791f07-c8c1-4253-9db2-315de475bd4d" />, which allows this snippet of code to run in the loop:
<img width="775" height="85" alt="Screenshot 2026-07-24 at 2 39 09 PM" src="https://github.com/user-attachments/assets/4e5fea43-3ba9-4afe-ba8e-297e2b5fbfbf" />. 

Previously, I had thought that simply detecting when the PIR is on HIGH mode is enough to trigger the wakeup mode AND snap the photo, but because everytime the PIR goes to HIGH, it only pulses there for a couple of seconds, and by the time that the system checked everything in the setup, the PIR had already gone to LOW. However, I was able to take photos some of the time because the sensor I'm using is pretty cheap so it wasn't that reliable.

Now, using the correct logic for the wakeup and sleep sequence and hooking the whole thing to a USB powerbank, I can successfully take photos when the PIR sensor detects a living being in front of it!


## 7/20/26
### What I did
I modified the ESP32 camera server example sketch so that instead of running a web server on WiFi, I'm capturing single photos and saving them onto the microSD card. 
### What I learned
Because I didn't need the server, I could delete all the code relating to those. I also had to make sure to include: if (!SD_MMC.begin("/sdcard", true)) {
  Serial.println("SD Card Mount Failed");
  return;
}
in order to free up pin 12 for the pir sensor.
Another hard part was finding the syntax for writing the image data onto the sd card (highlighted lines here): <img width="837" height="197" alt="Screenshot 2026-07-20 at 11 43 50 AM" src="https://github.com/user-attachments/assets/173a945d-a0fe-4b5b-a110-25a9b35c37d0" />

Right now, I have the camera set to wait one second before capturing another photo, but maybe I'll figure out a way so it can capture it quicker but also not take up so much data.

## 7/19/26
### What I did
I did the wiring for the three components of the project: the ESP32-CAM, the PIR sensor, and the ESP32-CAM motherboard (MB) (and also the power supply module connected on the breadboard).

<img width="460" height="340" alt="Screenshot 2026-07-20 at 9 53 11 PM" src="https://github.com/user-attachments/assets/a9a49c79-bca0-4918-adbd-fe57959bb6b4" />


### What I learned
Because I was using the motherboard and not the FTDI adapter on the ESP32-CAM and I want to connect a sensor to the camera module, I had to figure out which GPIOs corresponded to what function on the ESP32-CAM. Here is a diagram of what all of the GPIOs do: <img width="1000" height="445" alt="image" src="https://github.com/user-attachments/assets/9abd23bd-37af-4d07-9102-aa172263af48" /> (from https://randomnerdtutorials.com/esp32-cam-pir-motion-detector-photo-capture/) For this project, all I have to focus on are five of the pins:

1. UOR (GPIO3): Serial input
2. UOT (GPIO1): Serial output
UOR & UOT should be connected to the motherboard from the cam module. They are what sends and receives data and communication between the two modules.
3. 5V: To deliver power to components
4. GND: To ground all components
5. GPIO12: This one connects the GPIO12 pin on the MB to the OUT pin of the PIR sensor. The ESP32-CAM uses a standard 4 bit mode, which uses up six pins (GPIOs 2, 4, 12, 13, 14, & 15), but if I switch to using just 1 bit, then it will only take up three pins (GPIOs 2, 14, 15). I need to use 1 bit in order to use the micro SD card to store my data from the camera. Also have to make sure to configure the SD card to 1 bit mode.
6. You have to connect the camera module's GPIO0 pin to GND but disconnected for now until you upload the code to put the module in flashing mode, and then remove it afterwards.

I think understanding what the pins do on the camera module was the hardest part of today.
