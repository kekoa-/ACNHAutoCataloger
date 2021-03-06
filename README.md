# ACNHAutoCataloger
Automatically records what's in your Animal Crossing: New Horizons catalog

## Requirements
* Elgato Cam Link / Cam Link 4k / something else that exposes an HDMI input as a webcam
  * Regular capture cards meant for streaming will probably not work out of the box, you may have to use screen capture
  * I also highly recommend having an HDMI splitter so you can see what is happening on your Switch via a different
    screen, since the catalog script will lock your Cam Link and OBS/other software will not be able to use it
* A dev board with an ATMega16U2 (Arduino Uno R3 has one conveniently onboard for USB communication)
* USB to UART adapter (another Arduino can be used by connecting RX -> TX and vice versa)

## Usage
1. Flash the provided Joystick.hex to your ATMega16U2 using DFU mode. 
    * [Instructions for doing this for an Arduino Uno R3](https://www.arduino.cc/en/Hacking/DFUProgramming8U2)
2. Set up a virtualenv and install dependencies
    * `python -m virtualenv venv`
    * `pip install -r requirements.txt`
3. Connect your Switch to your Cam Link 
4. Open up Nook Shopping (stay on the screen that says `Welcome to Nook Shopping!`)
5. Press Home, then navigate to Controllers > Change Grip/Order
    * It is very important that you do this, otherwise Animal Crossing will not recognize accept input from the fake
    controller and the script will not work. In addition, the script assumes you are on this screen.
6. Run the script: `python main.py <serial port>`

## How does it work?
The ATMega16U2 emulates a USB controller that the auto cataloger can control via serial port. Using this, it's trivial 
to manipulate the game interface to get information on each item in your catalog.

The Cam Link lets us use OpenCV in order to capture image data from the Switch. We then use EAST to perform text 
detection to see if an item has any variants, and Tesseract OCR to turn everything into text. Finally, we use datamined
information from the game in order to generate output for what items you have cataloged.