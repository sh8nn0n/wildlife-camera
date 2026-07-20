# Journal
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
