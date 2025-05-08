# Stopwatch

For a quick recap on how to display digits, see my overview on the <a href="https://github.com/kiarazhu/Seven-Segment-Display"> Seven Segment Display</a>.

The stopwatch will count up and have two digits illuminated to represent the time elapsed in seconds:
- When counting from 0.0-9.9, the ones and tenths place, along with the decimal point, will be illuminated. The display will increment every 0.1s.
- Once the stopwatch surpasses 9.9ms and starts counting from 10-99s, the tens and ones place will be illuminated instead. The display will update every 1s.

The stopwatch will also have two buttons:
- Start/stop button: Starts the stopwatch if it is paused or in the reset state, otherwise pauses the stopwatch.
- Reset button: Sends the stopwatch to a reset state, turning off the display and clearing the time elapsed.

## State Diagram
(Insert diagram here)

To implement the stopwatch, I will be using the ATmega324A microcontroller. The setup relies on the megaAVR data sheet, which can be found <a href="https://ww1.microchip.com/downloads/en/DeviceDoc/ATmega164A_PA-324A_PA-644A_PA-1284_P_Data-Sheet-40002070B.pdf"> here</a>.

## Setting up Timer/Counter 1

### Output Compare Register 1 A

First, we should understand how frequently we wish for an output compare. The shortest amount of time we are required to count is every 0.1s (for incrementing from 0.0-9.9s). Thus, we will setup timer/counter 1 to reach a compare match (A) every 10ms. 

To obtain the OCR value, we can use the following formula:

$OCR = CLK / (PRE * FREQ) - 1$

CLK: Number of clock cycles per second. For the ATmega324A, we have 8MHz = 8 000 000 Hz.

PRE: Clock prescaler; we will use PRE = 8.

FREQ: Number of compare matches per second (1000 / 10 = 100). 

We can now evaluate OCR1A as follows:

$OCR1A = 8 000 000 / (8 * 100) - 1 = 1000 - 1 = 9999$

$OCR1A = 9999$

### Timer/Counter 1 Control Register A

The bits of TCCR1A are shown below:

<img width="650" alt="TCCR1A" src="https://github.com/user-attachments/assets/749fbcc0-3127-45d2-afd8-a18c5d0b0eee" />

<img width="650" alt="COM" src="https://github.com/user-attachments/assets/65b7e185-bdd6-4ee6-865b-c8f01ca92fc4" />

Our desired Compare Output Mode (non-PWM) is Clear on Compare Match on OC1A, so we will be setting COM1A1 to 1.

### Timer/Counter 1 Control Register B
<img width="650" alt="TCCR1B" src="https://github.com/user-attachments/assets/1e6a0605-6695-4424-86b0-5183d6fa3f30" />

<img width="650" alt="CS" src="https://github.com/user-attachments/assets/94de73ba-d9c9-4b96-b31a-9c7607d95dea" />

Since we have $CLK_{IO}/8$ (from prescaler), CS11 will be set to 1. 

<img width="650" alt="WGM" src="https://github.com/user-attachments/assets/50089e22-ce27-4bdb-831e-4a11dda13dc3" />

To set the Timer/Counter mode of operation to CTC with TOP set to OCR1A, we need to set WGM12 to 1.




## Setting up Interrupts
Implementation Notes:

If a var can be changed outside the normal flow of a program, it should be declared as `volatile`.

## I/O Board Connections

<img width="650" alt="Pinout" src="https://github.com/user-attachments/assets/4bd974fc-c7cb-4137-bdad-6b76e1fec801" />

INT1: PD3 on pinout
INT0: PD2 on pinout
- Seven segment display A to DP connected to AVR port A 
- Seven segment display CC connected to AVR pin C0
- Button B0 connected to AVR pin D2 - this is the start/stop button
- Button B1 connected to AVR pin D3 - this is the reset button
