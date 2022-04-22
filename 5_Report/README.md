# REPORT
-> This circuit is developed by interfacing ultrasonic sensor“HC-SR04” with AVR microcontroller. This sensor uses a technique called “ECHO” which is something you get when sound reflects back after striking with a surface.

-> We know that sound vibrations can not penetrate through solids. So what happens is, when a source of sound generates vibrations they travel through air at a speed of 220 meters per second. These vibrations when they meet our ear we describe them as sound. As said earlier these vibrations can not go through solid, so when they strike with a surface like wall, they are reflected back at the same speed to the source, which is called echo.

-> Ultrasonic sensor “HC-SR04” provides an output signal proportional to distance based on the echo. The sensor here generates a sound vibration in ultrasonic range upon giving a trigger, after that it waits for the sound vibration to return. Now based on the parameters, sound speed (220m/s) and time taken for the echo to reach the source, it provides output pulse proportional to distance.

![timing diagram](https://user-images.githubusercontent.com/80596756/164614761-b8127788-cdb2-459a-b95e-d5336896ba63.PNG)

As shown in figure, at first we need to initiate the sensor for measuring distance, that is a HIGH logic signal at trigger pin of sensor for more than 10uS, after that a sound vibration is sent by sensor, after a echo, the sensor provides a signal at the output pin whose width is proportional to distance between source and obstacle.

-> This distance is calculate as, distance (in cm) = width of pulse output (in uS) / 58.

-> Here the width of the signal must be taken in multiple of uS(micro second or 10^-6).

# CONNECTIONS

-> Here we are using PORTB to connect to LCD data port (D0-D7). Anyone who does not want to work with FUSE BITS of ATMEGA328 can not use PORTC, as PORTC contains a special type of communication which can only disabled by changing FUSEBITS.

-> In the circuit, you observe I have only took two control pins, this give the flexibility of better understanding. The contrast bit and READ/WRITE are not often used so they can be shorted to ground. This puts LCD in highest contrast and read mode. We just need to control ENABLE and RS pins to send characters and data accordingly.

The connections which are done for LCD are given below:

PIN1 or VSS to ground

PIN2 or VDD or VCC to +5v power

PIN3 or VEE to ground (gives maximum contrast best for a beginner)

PIN4 or RS (Register Selection) to PD6 of uC

PIN5 or RW (Read/Write) to ground (puts LCD in read mode eases the communication for user)

PIN6 or E (Enable) to PD5 of uC

PIN7 or D0 to PB0 of uC

PIN8 or D1 to PB1 of uC

PIN9 or D2 to PB2 of uC

PIN10 or D3 to PB3 of uC

PIN11 or D4 to PB4 of uC

PIN12 or D5 to PB5 of uC

PIN13 or D6 to PB6 of uC

PIN14 or D7 to PB7 of uC

In the circuit you can see we have used 8bit communication (D0-D7) however this is not a compulsory and we can use 4bit communication (D4-D7) but with 4 bit communication program becomes a bit complex. So as shown in the above table we are connecting 10 pins of LCD to controller in which 8 pins are data pins and 2 pins for control.

The ultrasonic sensor is a four pin device, PIN1- VCC or +5V; PIN2-TRIGGER; PIN3- ECHO; PIN4- GROUND. Trigger pin is where we give trigger to tell the sensor to measure the distance. Echo is output pin where we get the distance in the form of width of pulse. The echo pin here is connected to controller as an external interrupt source. So to get the width of the signal output, the echo pin of sensor is connected to INT0 (interrupt 0) or PD2.

### Now for getting the distance we have to program the controller for the following:

1. Triggering the sensor by pulling up the trigger pin for atleast 12uS.

2. Once echo goes high we get an external interrupt and we are going to start a counter (enabling a counter) in the ISR (Interrupt Service Routine) which is executed right after an interrupt triggered.

3. Once echo goes low again an interrupt is generated, this time we are going to stop the counter (disabling the counter).

4. So for a pulse high to low at echo pin, we have started a counter and stopped it. This count is updated to memory for getting the distance, as we have the width of echo in count now.

5. We are going to do further calculations in the memory to get the distance in cm

6. The distance is displayed on 16x2 LCD display.

For setting up the above features we are going to set the following registers:

![register](https://user-images.githubusercontent.com/80596756/164646867-1f9d2343-3c1f-4e78-a22a-72902c584014.PNG)

The above three registers are to be set accordingly for the setup to work and we are going to discuss them briefly,

BLUE (INT0): this bit must be set high to enable the external interrupt0, once this pin is set we get to sense the logic changes at the PIND2 pin.

BROWN (ISC00, ISC01): these two bits are adjusted for the appropriate logic change at PD2, which to be considered as interrupt.

![interrupt](https://user-images.githubusercontent.com/80596756/164647174-78ba9039-7e75-4569-911c-9e1e69f7a444.PNG)

So as said earlier we need an interrupt to start a count and to stop it. So we set ISC00 as one and we get an interrupt when there is a logic LOW to HIGH at INT0 ; another interrupt when there is a logic HIGH to LOW.

RED(CS10): This bit is simply to enable and to disable counter. Although it works along with other bits CS10, CS12. We are not doing any prescaling here, so we need not worry about them.

# RESULT
      The output will be appeared on lcd display.
      
      



# Source
  INTERNET
