## 4x4 LED MATRIX

### Description
#### In this blog we will focus on how to make and code a 4x4 LED matrix using a shift register(SN7HC595N).

### Materials Required
- #### Shift register(SN7HC595N)
- #### Jumper cables
- #### Arduino board(I will be using Arduino UNO)
- #### 16 LED's
- #### 330 ohm's resistors x4
- #### Soldering kit
- #### Pcb plate
- #### Solid wires

### Circuit-
- #### Place 16 LED'S in square such that anode of each LED are facing downwards and cathodes facing rightwards. 
- #### Connect all the cathodes of the LED in columns
- #### Connect all the anodes of the LED'S in rows
- #### Take output from each rows and columns,so at the end you will have 8 outputs from the4x4 matrix.

### Precautions
- #### Correct value of resistor is very important as the circuit won't work properly without it.
- #### While soldering be very careful and make sure no row and column wires are touching each other.
- #### Do not connect the circuit while arduino is on i.e-when arduino board is powered.
- #### Individually check all the LED'S before connecting.
### Circuit Diagram
![circuit_dia1](https://github.com/ONEV2/4x4-LED-matrix/blob/master/circuit/circuit%20(1).png)
### Explanation

| Arduino Uno  |   | Shift Register     |
| ------------- |---| ------------- |
| 5v            | - |   SCLR,VCC    |
| D11           | - |   SCLK        |
| D12           | - |   RCLK        |
| D10           | - |  Pin 14 (SER) |
#### Important pins on the IC-
- ##### SER (Serial) where the data gets in;
- ##### SRCLK (Serial Clock) the pin you set to high to store what’s in SER;
- ##### RCLK (Register Clock) the pin you set to high once you’re done setting all the pins.
##### Shift register chip transforms bits that are inserted in series trough the data pin into 8 parallel bits,So if you want to send lets say 10010000 you start with the least significant bit (0) so you set SER to LOW (D10 on the Arduino). Next, you set SCK (D11 on the Arduino) to HIGH and then to LOW, to “save” the value.

### Code
```c
byte character[8]={0x00, 0x00, 0x00, 0x00, 0xFf, 0xfF, 0xFf, 0xFf};//for row
uint8_t colPins[4]={2, 3, 4, 5};//specify's column pin,directly connected to arduino

#define SER_PIN 10
#define SCK_PIN 11
#define RCK_PIN 12

void setup() {
  // Turn everything to low
  for(int i=0; i<8; i++) {
    pinMode(colPins[i],OUTPUT);
  }
  pinMode(SER_PIN, OUTPUT);
  pinMode(SCK_PIN, OUTPUT);
  pinMode(RCK_PIN, OUTPUT);
  Serial.begin(9600);
}

void write595(byte data) {
  digitalWrite(RCK_PIN, LOW);
  shiftOut(SER_PIN, SCK_PIN, LSBFIRST, data);
  digitalWrite(RCK_PIN, HIGH);
}

void loop() {
 // iterate each row
  int rowbits = 0x08;//for registering the value
   for(int row=0; row<4; row++) {
      for(int k=0; k<4; k++) 
      digitalWrite(colPins[k],HIGH); // Cleanup cols
    write595(rowbits); // prepare to write the row
    for(int col=0; col<4; col++)
      digitalWrite(colPins[col]/*operating column pins*/, character[row+4] & 1 << col ? LOW : HIGH);
      delay(1000);
    write595(0);
    rowbits >>= 1; 
   }
}
```
### Things to take away 
- #### By changing the code you can actually print different patterns and figures on a 4x4 matrix.
- #### It can act as a low cost display for small projects.

### References
- ### [SN74HC595N datasheet](http://www.ti.com/lit/ds/symlink/sn74hc595.pdf)
- ### [online circuit diagram building software](https://www.circuit-diagram.org/editor/)


