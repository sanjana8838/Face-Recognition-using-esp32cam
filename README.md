# Face-Recognition-using-esp32cam

## Tools used
1. Python Libraries - numpy, glob, os, PIL, sklearn
2. micromlgen package - https://github.com/agrimagsrl/micromlgen.git
3. Arduino IDE
4. Hardware - ESP32 CAM

## Procedure
- A supervised model is created using SVM in python
- The trained model is convereted to a C header file using micromlgen and used in the code to be written in the esp32cam module
- camerapins.h holds the pin definitions of the esp32 cam module
- The esp32 cam is made to click images in the loop() and predict the image's class
- If a classification is made, a 0/1 is printed in the serial monitor

## Resources Referred 
- https://eloquentarduino.github.io/2019/11/how-to-train-a-classifier-in-scikit-learn/
- https://eloquentarduino.github.io/2019/11/you-can-run-machine-learning-on-arduino/
- https://techtutorialsx.com/2020/06/13/esp32-camera-face-detection/
- https://www.survivingwithandroid.com/esp32-cam-image-classification-machine-learning/
