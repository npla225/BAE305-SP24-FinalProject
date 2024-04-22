# BAE305 SP24 Final Project: The Palmernator

Noah Lane, Lily Schlaug, and Isaac Stevens

April 20, 2024

## Summary of Project ##

For the BAE 305 final project, our group created a drink mixer, specifically an Arnold Palmer mixer. An Arnold Palmer is a mixed drink consisting of iced tea and lemonade, with our machine dispensing 25% lemonade and 75% iced tea. Pressing a certain color button (green, yellow, red) on the Arduino, will start the code and open the solenoids in the mixer causing tea and lemonade to be dispensed into a cup. The amount of Arnold Palmer depends on the button pressed with the green dispensing 4 ounces, the yellow dispensing 8 ounces, and the red dispensing 12 ounces. Once the amount of Arnold Palmer is properly dispensed, a stirrer attached to a motor will rotate within the cup causing the drink to be mixed. After a small delay once the stirrer stops, another button can be pressed again to start the code with a different proportion size. The iced tea and the lemonade were stored in a 2-liter plastic bottle held a few feet above the cup to allow natural flow caused by gravity. Once a button is pressed, a double relay switch is turned on and a solenoid on each side of the machine will activate and allow the liquid to flow through causing the cup to be filled. Lastly, an LCD was used to instruct the user on how to use the mixer and as well as display congratulations once the process is over.

## Design Description ##

Materials:

Drawings:

Code:

```c++
// include the library code:
#include <Wire.h>               //For I2C
#include <LiquidCrystal_I2C.h>  //For I2C LCD
// used example code at this link  http://www.arduino.cc/en/Tutorial/LiquidCrystalHelloWorld

// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

//pin that the buttons are connected to
int button4 = 2;                    
int button8 = 3; 
int button12 = 4; 

LiquidCrystal_I2C lcd(0x3F, 16, 2); // set the LCD address to 0x3F for a 16 chars and 2 line display


void setup() {
  //set the button pin as an input
  pinMode(button4, INPUT_PULLUP);     
  pinMode(button8, INPUT_PULLUP);  
  pinMode(button12, INPUT_PULLUP);    
  
  // set up the LCD's number of columns and rows:
  lcd.init(); //initialize the lcd
  lcd.backlight(); //open the backlight
}

void loop() {
  /*This block tests the LCD It worked with my setup SCL and SDA are
    supposed to be connected to A5 and A4 respectively.*/
  lcd.setCursor(3, 0); // set the cursor to column 3, line 0
  lcd.print("SunFounder"); // Print a message to the LCD
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("Hello, World!"); // Print a message to the LCD.

  /*Your code starts here*/

  //display info to user, max 32 characters, 16x2 display
  //clear display
  lcd.clear();
  // Print a message to the LCD.
  lcd.print("Arnold Palmer");
  // set the cursor to column 0, line 1 (note: line 1 is the second row, since counting begins with 0):
  lcd.setCursor(0, 1);
  // print the oz options 
  lcd.print("4oz, 8oz, 12oz");

  //user input 4, 8, or 12 oz via buttons
  //solenoids open for time based on oz. requested
  if (digitalRead(button4) == LOW){             //if 4oz button is pressed
    //open solenoid 1, lemonade
    //open solenoid 2, tea
    delay(2000);  
    //close 1
    delay(2000)
    //close 2
  }

  else if (digitalRead(button8) == LOW){             //if 8oz button is pressed
    //open solenoid 1 
    //open solenoid 2  
    delay(4000);
    //close 1
    delay(4000); 
    //close 2
  }

  else if (digitalRead(button12) == LOW){            //if 8oz button is pressed
    //open solenoid 1 
    //open solenoid 2  
    delay


## Testing Description ##

## Discussion ##

## Testing Results ##
