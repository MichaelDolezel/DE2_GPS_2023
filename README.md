# C project

### Topic

**GPS-based environmental sensor data logger**
Create a comprehensive data logging system using an AVR microcontroller. The system integrates GPS functionality for location tracking and an I2C environment sensor to capture data related to environmental conditions. The project aims to log and display sensor data and provide the capability to export the collected information for analysis.

# Recommended README.md file structure

### Team members

* Michael Doležel (responsible for ...)
* Samuel Vandovič (responsible for ...)
* Petr Vaněk (responsible for ...)
* Dominik (responsible for ...)

## Theoretical description and explanation

In our project use GPS module [**GPS NEO-6M GYNEO6MV2**](https://dratek.cz/arduino/1510-gps-neo-6m-gyneo6mv2-modul-s-antenou.html?ehub=e6081933ad2044229d14dafac815f8e3) and save data "[*GPRMC*](https://docs.novatel.com/OEM7/Content/Logs/GPRMC.htm)" than we merge them with data from Humidity/temperature **DHT12 digital sensor** and display them on **SH1106 I2C OLED display 128x64**. Data are also saved to .txt file for future use and analysis.

GPS sensor communicates with uart protocol and sends info in various formats from a few GPS satellites messages usually look like this: 

$GPRMC 093533.00 A 4913.58989 N 01634.42942 E 0.322 21.61 161123   A53\r\n
$GPVTG 21.61 T A 0.322 0.597 K A01\r\n
$GPGGA 093533.00 4913.58989 N 01634.42942 E 1 03 4.64 261.4 M 42.4 M   56\r\n
$GPGSA A 2 04 09 16           4.74 4.64 1.0009\r\n
$GPGSV 2 1 06 04 21 082 36 07       35 09 55 078 40 13 00 263 42\r\n
$GPGSV 2 2 06 16 16 041 36 20 47 2987D\r\n
$GPGLL 4913.58989 N 01634.42942 E 093533.00 A A65\r\n
$GPRMC 093533.00 A 4913.58989 N 01634.42942 E 0.322 21.61 161123   A53\r\n

![GPS data](images/gps_1.png)
![GPS data](images/gps_2.png)
![GPS data](images/gps_3.png)

GPS sensor sends them at a rate of one message per second on a baud rate of 9600
Our problem was that working with this sensor turned out to be a bit complicated due to the low power of antenna causing us to receive GPS signal outside of the window or outside 

![GPS measuring](images/gps_measuring.png)

So we created a simple solution to transmit our measured signal on one Arduino and process the data in the second Arduino 

![Ardzuino as source of signal](images/gps_arduino.png)

The next task was to receive the data coming to uart and then select GPRMC part and store it as data.
the GPRMC looks like this: **$GPRMC 093533.00 A 4913.58989 N 01634.42942 E 0.322 21.61 161123   A53\r\n**
First field is UTC of position in hhmmss.ss format so in this case we measured at 09:35:33 UTC. Next is position status with only two values (A = data valid, V = data invalid). after that is lattitude in format 
(DDmm.mm) followed by latitude direction (N = North, S = South) as can be expected next two fields are longitude (DDDmm.mm) and longitude direction (E = East, W = West). The following fields are Speed over ground in knots, than Track made good - degrees True (which means traveling direction that is independent on direction which device is pointing), than date (dd/mm/yy), last is 	
Check sum and sentence terminator.

From all this data we decided that only Date, time and position is usefull.
These coordinates that we measured from window is roughly 223m from our correct location which can be explained by measurement short time after sensor found GPS signal. 

Schematic of our project:



## Software description

Put flowchats of your algorithm(s) and direct links to source files in `src` or `lib` folders.

## Instructions

Write an instruction manual for your application, including photos and a link to a short app video.

## References

1. Write your text here.
2. ...
