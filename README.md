# Stopwatch

For a quick intro, see my brief overview on the <a href="https://github.com/kiarazhu/Seven-Segment-Display"> Seven Segment Display</a>.

## The Goal

The stopwatch will count up and have two digits illuminated to represent the time elapsed in seconds:
- When counting from 0.0-9.9, the ones and tenths place, along with the decimal point, will be illuminated. The display will increment every 0.1s.
- Once the stopwatch surpasses 9.9ms and starts counting from 10-99s, the tens and ones place will be illuminated instead. The display will update every 1s.

The stopwatch will also have two buttons:
- Start/stop button: Starts the stopwatch if it is paused or in the reset state, otherwise pauses the stopwatch.
- Reset button: Sends the stopwatch to a reset state, turning off the display and clearing the time elapsed.

## State Diagram


To implement the stopwatch, I will be using the ATmega324A microcontroller. The setup will reference the megaAVR data sheet, which can be found <a href="https://ww1.microchip.com/downloads/en/DeviceDoc/ATmega164A_PA-324A_PA-644A_PA-1284_P_Data-Sheet-40002070B.pdf"> here</a>.

## Setting up Timer/Counter 1

 We are working with an 8MHz AVR clock.
- PRE: 8


### Output Compare Register 1 A
CLK / PRE = 8 000 000 / 8 = 1 000 000 ticks / second

1 000 000 / 100 = 1 000 clock cycles every 10ms

1000 - 1 = 9999

OCR1A = 9999

### Timer/Counter 1 Control Register A

<img width="650" alt="TCCR1A" src="https://github.com/user-attachments/assets/749fbcc0-3127-45d2-afd8-a18c5d0b0eee" />

<img width="650" alt="COM" src="https://github.com/user-attachments/assets/65b7e185-bdd6-4ee6-865b-c8f01ca92fc4" />

Our desired Compare Output Mode (non-PWM) is Clear on Compare Match, so we will be setting COM1A1 to 1.

### Timer/Counter 1 Control Register B
<img width="650" alt="TCCR1B" src="https://github.com/user-attachments/assets/1e6a0605-6695-4424-86b0-5183d6fa3f30" />

<img width="650" alt="CS" src="https://github.com/user-attachments/assets/94de73ba-d9c9-4b96-b31a-9c7607d95dea" />

Since we have $CLK_{IO}/8$ (from prescaler), CS11 will be set to 1. 

<img width="650" alt="WGM" src="https://github.com/user-attachments/assets/50089e22-ce27-4bdb-831e-4a11dda13dc3" />

To set the Timer/Counter mode of operation to CTC with TOP set to OCR1A, we need to set WGM12 to 1.




## Setting up Interrupts
