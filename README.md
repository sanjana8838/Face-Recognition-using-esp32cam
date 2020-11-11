# Face-Recognition-using-esp32cam

## Tools used
1. Libraries - numpy, glob, os, PIL
2. micromlgen package - https://github.com/agrimagsrl/micromlgen.git
3. Arduino IDE

## Procedure
- A supervised model is created using SVM in python
- The trained model is convereted to a C header file using micromlgen and used in the code to be written in the esp32cam module
- camerapins.h holds the pin definitions of the esp32 cam module
- The esp32 cam is made to click images in the loop() and predict the image's class
- If a classification is made, a 0/1 is printed in the serial monitor
