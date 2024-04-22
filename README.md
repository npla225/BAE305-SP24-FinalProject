# BAE305 SP24 Final Project: The Palmer-nator

Noah Lane, Lily Schlaug, and Isaac Stevens

April 20, 2024

## Summary of Project ##

For the BAE 305 final project, our group created a drink mixer, specifically an Arnold Palmer mixer. An Arnold Palmer is a mixed drink consisting of iced tea and lemonade, with our machine dispensing 25% lemonade and 75% iced tea. Pressing a certain color button (green, yellow, red) on the Arduino, will start the code and open the solenoids in the mixer causing tea and lemonade to be dispensed into a cup. The amount of Arnold Palmer depends on the button pressed with the green dispensing 4 ounces, the yellow dispensing 8 ounces, and the red dispensing 12 ounces. Once the amount of Arnold Palmer is properly dispensed, a stirrer attached to a motor will rotate within the cup causing the drink to be mixed. After a small delay once the stirrer stops, another button can be pressed again to start the code with a different proportion size. The iced tea and the lemonade were stored in a 2-liter plastic bottle held a few feet above the cup to allow natural flow caused by gravity. Once a button is pressed, a double relay switch is turned on and a solenoid on each side of the machine will activate and allow the liquid to flow through causing the cup to be filled. Lastly, an LCD was used to instruct the user on how to use the mixer and as well as display congratulations once the process is over.

## Design Description ##

### Materials: ###      
![image](https://github.com/npla225/BAE305-SP24-FinalProject/assets/156371043/868765b4-e32b-44b6-82e7-a3a1956556bd)


### Drawings: ###      
![image](https://github.com/npla225/BAE305-SP24-FinalProject/assets/156371043/519abd4b-f517-4149-b785-d5f05e6a83a2)

### Code: ###   

```c++
// include the library code:
#include <Wire.h>               //For I2C
#include <LiquidCrystal_I2C.h>  //For I2C LCD
// used example code at this link  http://www.arduino.cc/en/Tutorial/LiquidCrystalHelloWorld

// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
//LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

//pin that the buttons are connected to
int button4 = 2;                    
int button8 = 3; 
int button12 = 4; 
int tea = 7; //tea solenoid relay pin
int lem = 8; //lem solenoid relay pin

LiquidCrystal_I2C lcd(0x3F, 16, 2); // set the LCD address to 0x3F for a 16 chars and 2 line display

//motor control pins
int enablePin = 11; // Enable pin of the motor driver (PWM pin)
int in2Pin = 12;    // Input 1 pin of the motor driver
int in1Pin = 13; 

void setup() {
  //set the button pin as an input
  pinMode(button4, INPUT_PULLUP);     
  pinMode(button8, INPUT_PULLUP);  
  pinMode(button12, INPUT_PULLUP);    
  pinMode(tea,OUTPUT); //set up relays
  pinMode(lem,OUTPUT);

  // set up the LCD's number of columns and rows:
  lcd.init(); //initialize the lcd
  lcd.backlight(); //open the backlight

  /*Your code starts here*/

  //display info to user, max 32 characters, 16x2 display
  //clear display
  lcd.clear();
  // Print a message to the LCD.
  lcd.print("The Palmernator");
  // set the cursor to column 0, line 1 (note: line 1 is the second row, since counting begins with 0):
  lcd.setCursor(0, 1);
  // print the oz options 
  lcd.print("4oz, 8oz, 12oz");

  // Set motor control pins as outputs
  pinMode(enablePin, OUTPUT);
  pinMode(in1Pin, OUTPUT);
}

void loop() {
 
  //user input 4, 8, or 12 oz via buttons
  //solenoids open for time based on oz. requested
  if (digitalRead(button4) == LOW){             //if 4oz button is pressed
    lcd.clear();
    lcd.print("FORE...");
    digitalWrite(tea, HIGH); //open solenoid 1, tea
    digitalWrite(lem, HIGH); //open solenoid 2, lemonaid
    delay(6000);  
    digitalWrite(lem,LOW); //close 1
    delay(6000);
    digitalWrite(tea,LOW); //close 2
    end(); //go to end function
  }

  else if (digitalRead(button8) == LOW){             //if 8oz button is pressed
    lcd.clear();
    lcd.print("FORE...");
    digitalWrite(tea, HIGH); //open solenoid 1, tea
    digitalWrite(lem, HIGH); //open solenoid 2, lemonaid 
    delay(8000);
    digitalWrite(lem,LOW);//close 1
    delay(8000); 
    digitalWrite(tea,LOW);//close 2
    end(); //go to end function
  }

  else if (digitalRead(button12) == LOW){            //if 12oz button is pressed
    lcd.clear();
    lcd.print("FORE...");
    digitalWrite(tea, HIGH); //open solenoid 1, tea
    digitalWrite(lem, HIGH); //open solenoid 2, lemonaid  
    delay(10000); 
    digitalWrite(lem,LOW);//close 1
    delay(10000); 
    digitalWrite(tea,LOW);//close 2
    delay(10000);
    end(); //go to end function
  }
}         
  void end() { 
   // Turn the motor on
   digitalWrite(in1Pin, HIGH);
   analogWrite(enablePin, 125); // Set speed (255 is maximum)
   delay(4000); // Wait for 4 seconds
   // Turn the motor off
   digitalWrite(in1Pin, LOW);
   analogWrite(enablePin, 0); // Set speed to 0 (turn off motor)
   lcd.clear();    //display done
   lcd.setCursor(0,0); 
   lcd.print("Hole In One!");
   delay(10000);//wait 10s

   lcd.clear();
   // Print a message to the LCD.
   lcd.print("The Palmer-nator");
   // set the cursor to column 0, line 1 (note: line 1 is the second row, since counting begins with 0):
   lcd.setCursor(0, 1);
   // print the oz options 
   lcd.print("4oz, 8oz, 12oz");
  }
```

### Assembly Instructions: ###   
1. Build a simple frame that can fit a 12 oz cup underneath it while also accommodating for the stirring rod.
2. Mount the Arduino to the back of the frame leaving enough room for the display and breadboard to fit on the front of the machine.
3. Attach hose barbs to both ends of each solenoid with plumbers' tape to avoid leaks. 
4. Mount the solenoids on opposite sides of the frame above where the cup will be with the flow direction leading to the front of the frame.
5. Size tubing appropriately so that ample tubing leads from both solenoids to the front of the frame to pour into the drink.
6. Build a separate frame that will supply a sufficient elevation head to dispense at the desired flow rate.
7. Drill holes in the caps of the desired lemonade and tea containers so that the tubing can be inserted and sealed to prevent leaks. Make sure that the containers share the same dimensions or modify the frame accordingly.
8. Size tubing appropriately so that ample tubing leads from both containers to the inlet of the solenoids. 
9. Mount the motor to the front of the machine so that it will be able to reach the desired cup and not interfere with the outlet tubing.
10. Attach a breadboard and LCD to the front of the machine so that the display is on top of the buttons.
11. Mount the relay module to the top of the machine in proximity to both solenoids and the Arduino. 
12. Wire the buttons on the foremost breadboard to digital pins on the Arduino.
13. Wire the motor to the motor control unit on the breadboard connected to the Arduino.
14. Wire the display using the I2C ports on the Arduino.
15. Wire the two positive terminals of the solenoids to the relay module in the communication port.
16. Attach two wires in the always closed ports of the relays and wire them in parallel.
17. Wire both ground cables of the solenoids in parallel.
18. Connect the two signal ports of the relay module to digital pins on the Arduino as well as power and ground. 
19. Connect the DC power supply to the corresponding cables from the solenoids.
20. Connect the Arduino to a computer or sufficient DC power supply.
## Testing Description ##
### Test Equipment: ###   
Global Specialties 1403 DC Power Supply
Arduino IDE  
Graduated Cylinder 
### Testing Procedures ###
1. Connect the Arduino to a computer running Arduino IDE and the power and ground leads of the solenoids to the DC power supply.
2. Begin with the 4oz loop. 
3. Using a 2:1 volumetric ratio of tea to lemonade change the first delay on the relay open times to dispense ~1.33 oz of tea and lemonade for a total of 2.66 oz.
4. Close the lemonade relay and adjust the second delay to dispense an additional ~1.33 oz of tea to achieve the 2:1 ratio of tea to lemonade. 
5. Repeat steps 3 and 4 until the delay times result in the desired volumes of tea and lemonade.
6. Repeat steps 3 through 5 for the 8 oz and 12 oz loops with their respective volumes of tea and lemonade based on the 2:1 volumetric ratio.      
## Design Decision Discussion ##
After completing the testing phase of the project there were several aspects of the current design that could be modified to improve the efficiency of the system. To begin, it would be reccomended that the solenoids be replaced with pumps in order to ensure accurate dispensation. The calculated elevation head did not supply enough flow to dispense in a timely manner. This required the tanks to be placed on a shelf ~2ft above the machine, reducing practaclity. By using pumps, there wouldnt be a reliance on gravity in oder to dispense the drink and the rate of dispensation would not change based on the volume of fluid in the system. The pumps would also reduce the need to seal the tubes to the caps and not require the tanks to be flipped upside down, reducing the risk of leaks while pouring and spillage from refilling the containers.  The next possible modification to the machine would be to make the stirring rod height adjustable. In the current design, the stirring rod has to be detachable and placed into the cup prior to filling in order to be able to fit underneath the canopy of the machine. to require less user input in the mixing process, it would be reccomended that the stirring motor be put on a vertical track system attached to another motor so that the rod could be raised and lowered after the cup is placed under the nozzle. This would require another motor, (two are included in the sparkfun kit), a rack and pinion system, and a new function to be created in the code that would lower the motor after the cup is placed and modifying the "end()" fucntion to raise the rod after the mixing is complete.  
## Testing Results Discussion ##
elevation head to inconsisitent, noise from wiring solenoids messing up arduino
## Testing Results ##
