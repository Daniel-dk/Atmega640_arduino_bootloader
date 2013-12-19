Atmega640_arduino_bootloader
============================

a bootloader for the Atmega640 so it can be used in place of a Atmega2560 

Upload it using a ISP flashing tool such as the AVRISP MK II , or AVR JTAGICE or bit blaster or an arduino configured as an ICP 

the fuse settings are the same as the Atmega 2560 ( Arduino Mega 2560 )
Low fuses       = 0xFF
high fuses      = 0xD8
extended fuses  = 0xFD

lock bits       = 0x0F

add the following snipped to your boards.txt file, it usually lives at :
Arduino_IDE\hardware\arduino\avr\boards.txt

##############################################################
mega640.name=Arduino Mega 640
mega640.cpu=640

mega640.upload.tool=avrdude
mega640.upload.protocol=wiring
mega640.upload.maximum_size=61440
mega640.upload.speed=115200

mega640.bootloader.tool=avrdude
mega640.bootloader.low_fuses=0xFF
mega640.bootloader.high_fuses=0xD8
mega640.bootloader.extended_fuses=0xFD
mega640.bootloader.file=stk500v2/stk500boot_v2_mega640.hex
mega640.bootloader.unlock_bits=0x3F
mega640.bootloader.lock_bits=0x0F

mega640.build.mcu=atmega640
mega640.build.f_cpu=16000000L
mega640.build.core=arduino
mega640.build.variant=mega
##############################################################


i got the source from http://www.avr-developers.com/bootloaderdocs/index.html and changed the "robotx" to be for  UART0
in Windows with the following commands in cmd ( note that my arduino installation is in "C:\Arduino_boot\" )

set BASEDIR=C:\Arduino_boot\hardware
set DIRAVRUTIL=%BASEDIR%\tools\avr\utils\bin
set DIRAVRBIN=%BASEDIR%\tools\avr\bin
set DIRAVRAVR=%BASEDIR%\tools\avr\avr\bin
set DIRLIBEXEC=%BASEDIR%\tools\avr\libexec\gcc\avr\4.3.2
set OLDPATH=%PATH%
@path %DIRAVRUTIL%;%DIRAVRBIN%;%DIRAVRAVR%;%DIRLIBEXEC%;%PATH%
cd %BASEDIR%\arduino\bootloaders\stk500v2
%DIRAVRUTIL%\make.exe clean
%DIRAVRUTIL%\make.exe robotx
copy stk500boot.hex stk500boot_v2_mega640.hex
%DIRAVRUTIL%\make.exe clean
@path %OLDPATH%

