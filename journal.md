# Journal
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

I think understanding what the pins do on the camera module was the hardest part of today.
