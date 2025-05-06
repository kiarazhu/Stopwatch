# Stopwatch

To make the stopwatch, I will be using the ATmega324A microcontroller. For a quick intro, see my brief overview on the <a href="https://github.com/kiarazhu/Seven-Segment-Display"> Seven Segment Display </a>.

## The Goal

The stopwatch will count up and have two digits illuminated to represent the time elapsed in seconds:
- The ones and tenths place, along with the decimal point, will be illuminated when counting from 0.0-9.9s. The display will increment every 0.1s.
- The tens and ones place will be illuminated when counting from 10-99s. The display will update every 1s.

The stopwatch will also have two buttons:
- Start/stop button: 
- Reset button: Sends the stopwatch to a reset state, turning off the display and setting the time elapsed to 0.

## State Diagram

## Setting up Timer/Counter 1

## Setting up Interrupts
