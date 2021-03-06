 DIY ultra cheap GPS bike/car tracker based on ATMEL ATTINY 2313/4313 or ATMEGA 328P (arduino uno chip) and SIM800L module from China vendor SIMCOM. The description of SIM800L module is here : https://lastminuteengineers.com/sim800l-gsm-module-arduino-tutorial/

The device when called by mobile phone polls cell-id info from nearest 2G cell, uses GPRS to query Google servers for GPS location of that cell and sends back text message to your phone with current location link to Google map with timestamp. 

BILL OF MATERIALS (with the cost as in 2018): 

- SIM800L (3.5 USD on Aliexpress, but notice this module DOES NOT have GPS so we are polling info from base stations - if you want better accuracy from real GPS you need to use SIM808 board - all details described in this project - https://github.com/mcore1976/sim808gpstracker ) 
- ATMEL ATTINY2313 (2USD)  or ATMEGA 328P (arduino uno)  or ARDUINO MINI PRO 3.3V board instead
- LM7805 (0.5USD) - optional if you intend to connect to car/bike battery directly
- 6x 1N4007 (0.3 USD) - optional - for CAR/BIKE battery or USB Powerbank
- 2x 1000uF / 16V capacitor ( 0.5 USD) - when powered from 3xAA battery pack only 1 capacitor is needed 
- 100nF / 12V (or higher)  capacitor (0.2 USD) 
- universal PCB, pins & connector (2 USD) 


SOURCE FILE OPTIONS  : 

The code is written in avr-gcc and was uploaded via USBASP. Both binary output versions are provided : for ATTINY 2313(2313A/2313V) and ATMEGA 328P.

main.c  ( compilation script : compileatmega / compileatmega.bat ) - file for ATMEGA328P when MCU goes into POWERDOWN mode - the lowest power consumption (<3mA)

mainb.c ( compilation script : compileatmegab / compileatmegab.bat ) - file for ATMEGA328P when MCU periodically checks SIM800L 2G network status (once per 30 min and does radio switchoff for 30 min if necessary) - this version is most stable now but power consumption is slightly higher (~6mA)

main3.c ( compilation script : compileattiny / compileattiny.bat )  - file for ATTINY2313 when MCU goes int o POWERDOWN mode - the lowest power consumption (<2mA)

main3b.c ( compilation script : compileattinyb / compileattinyb.bat )  - file for ATTINY2313 when MCU periodically checks SIM800L 2G network status (once per 30 min and does radio switchoff for 30 min if necessary)- this version is most stable now but power consumption is slightly higher (5mA)

In the code you have to put correct APN, USERNAME and PASSWORD of GPRS access from your Mobile Network Operator before compiling - replace word "internet" below wit correct words for your MNO (check with your network provider hoe to configure GPRS access):

constchar SAPBR2[] PROGMEM = {"AT+SAPBR=3,1,"APN","internet"\r\n"}; // Put your mobile operator APN name here

constchar SAPBR3[] PROGMEM = {"AT+SAPBR=3,1,"USER","internet"\r\n"}; // Put your mobile operator APN username here

constchar SAPBR4[] PROGMEM = {"AT+SAPBR=3,1,"PWD","internet"\r\n"}; // Put your mobile operator APN password here

COMPILATION ON LINUX PC :

The script attached in repository  ( "compileatmega" or "compileattiny" ) can be used to upload data to the chip if you have Linux machine with following packages : "gcc-avr", "binutils-avr" (or sometimes just "binutils"), "avr-libc", "avrdude" and optionally "gdb-avr"(debugger only if you really need it) . For example in Ubuntu download these packages using command : "sudo apt-get install gcc-avr binutils-avr avr-libc gdb-avr avrdude". 
After this is done you can run from directory you downloaded the github files appropriate compilation script by commands 
- "sudo chmod +rx compileatmega*" and "sudo ./compileatmega" ( "sudo ./compileatmegab" )
- "sudo chmod +rx compileattiny*" and "sudo ./compileattiny" (  "sudo ./compileattinyb" )

COMPILATION ON WINDOWS PC : 

If you have Windows 10 machine please follow this tutorial to download and install full AVR-GCC environment for Windows : http://fab.cba.mit.edu/classes/863.16/doc/projects/ftsmin/windows_avr.html  with latest compiler from Microchip/Atmel.

After it is done please use "compileattinyX.bat" or "compileatmegaX.bat" for compilation inside directory where you have downloaded "mainXX.c" files. You have to be logged as Windows Administrator to use avrdude software.

PROGRAMMING THE ATTINY / ATMEGA / ARDUINO - connecting cables to the chip :

Link to video how to program the chip : https://www.youtube.com/watch?v=7klgyNzZ2TI

To upload program code to the chip using cheapest USBASP programmer (less than 2 USD on eBay/Aliexpress) look at this page :
http://www.learningaboutelectronics.com/Articles/Program-AVR-chip-using-a-USBASP-with-10-pin-cable.php , because we are not using ARDUINO bootloader here (especially for chips like ATTINY)

If you are having problems with C code compilation and USBASR programmer you may also look at these tutorials  :  

http://www.linuxandubuntu.com/home/setting-up-avr-gcc-toolchain-and-avrdude-to-program-an-avr-development-board-in-ubuntu 

https://blog.podkalicki.com/how-to-compile-and-burn-the-code-to-avr-chip-on-linuxmacosxwindows/  

Some people do not like to use universal PCB and are having problems with soldering. You may use "Arduino Pro Mini 3.3V" board (or clone)  instead with some pins and cables connected to SIM800L module.
There are two options for this board - 5V voltage and 3.3V voltage. Pay attention to it when selecting the board so it could match SIM800L board 3.3V TTL logic. 
To use "Arduino Pro Mini" you will have to connect USBASP programmer with KANDA socket (look here : https://www.atnel.pl/download/blog/ISP_KANDA.jpg )  to appropriate pins of this board  : SCK (pin 13), MISO (pin 12), MOSI (pin 11), RESET (pin RST), pin VCC, pin GND - like shown here when changing/uploading bootloader https://www.arduino.cc/en/Hacking/MiniBootloader
Description of this board "Arduino Pro Mini" is here : https://www.theengineeringprojects.com/2018/06/introduction-to-arduino-pro-mini.html 

This GPS tracker solution is not based on ARDUINO FRAMEWORK (it does not use ARDUINO bootloader and we are getting rid of it here), it uses pure C code instead so USBASP programmer is still needed. 


OTHER INFORMATION : 

For smallest chip ATTINY2313 the code takes about 2KB of Flash memory so the chip memory gets completely full. However old ATTINY2313 chips takes less space on PCB and are a bit cheaper than ATMEGA328P.
Considering the SIM800L capability if more Flash memory is available (like in ATMEGA328P), the chip could even upload the GPS data to some EMAIL/FTP/HTTP server to get car tracking history. 
In source files above same functions are available for ATTINY2313 and ATMEGA328P

The tracker has ultra low power consumption because it is utilizing SLEEP MODE on SIM8XX/9XX module and POWER DOWN feature on ATTINY/ATMEGA MCU (current in standby is below 2mA, but only when signal RI/RING from SIM800L is connected to MCU) and connects to GPRS/polls GPS only upon request. Also the LED on the SIM800L is switched off to further reduce current consumption.
This will give you something like at least 1 month of work time on smallest USB powerbanks like 2000mAh or 3xAA battery ( I personally do recommend to use  3xAA because powerbanks have LED and converters that drain extra current). 
The ATTINY/ATMEGA wakes up (or activates the code to check GPS location) when SIM8xx PIN RING goes to GND (in case of incoming call or SMS or MODULE RESTART - "Call Ready" message from URC). 

When SIM8xx module sends SMS/GPRS data it it may draw a lot of current ( up to 2A ) in short peaks so it is crucial to use good cables and thick copper lines for GND and VCC on PCB. This is the main issue people face when dealing with SIM8xx/9xx modules. The voltage may additionaly drop during this situation so that is why such big capacitor is in use. 
In the design there are 1N4007 diodes connected in serial+parallel to ensure that voltage is dropped to correct levels (when powered from car/bike battery or USB 5V powerbank) which is around 4,4V for SIM8XX module (5V would damage SIM800L) and 2A current can be handled. Also we have to secure that during current peaks the voltage will not drop below 3V for SIM800L ( otherwise SIM800L would restart itself during SMS/GPRS sending). 

The tracker can be powered 3 ways  ( and depending on powering method you can get rid of LM7805 or some 1N4007 diodes to simplify the design): 

a) powering directly from car/bike battery - ensure that proper cables are used (must sustain 2Amps) and attach small heatsink to LM7805 TO220 case. It will not get hot all the time but ensure that current/heat protection within LM7805 would not activate. In such case the tracker will consume in standby something like 12mA due to LM7805 drop (conversion from 12V to 5V). You may try to put some replacement of LM7805 like switching power supply step-down converter -  DC-DC buck converter based on LM2596 - it generates less heat.

b) powering from USB 5V Powerbanks - it is good to use the cheapest USB powerbanks that do not have current sensor. Remember that GPS tracker will draw VERY LOW current ( lower than 2mA). Some advanced powerbanks tend to switch off USB 5V when they find out that there is very little current consumed. If you have Powerbank with signalling LED then I suggest to get rid of this LED to reduce power drain on Powerbank. 

c) Powering from 3 x AA battery pack (LR6). Estimated working time on set of batteries is more than 1 month due to low current ( less than 2mA). Batteries must be connected directly to VCC on ATTINY-ATMEGA/SIM8XX without any diodes - this is the simplest design. Batteries will give something like 4.5V of powering voltage which is perfect for SIM800L. Avoid Ni-Mh Accumulators. They give only 1.2V each. 

The solution has been field tested and works without a glitch. 

You can see how it works :
here :      https://www.youtube.com/watch?v=t7mvomytDq4
and here :  https://www.youtube.com/watch?v=546j1f_qA50
