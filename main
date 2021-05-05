/*

*/
#include <Wire.h>
#include <SerLCD.h> //Click here to get the library: http://librarymanager/All#SparkFun_SerLCD
#include "SparkFun_Qwiic_Keypad_Arduino_Library.h"
#include "SparkFun_MMA8452Q.h"

/** START of I2C OBJECTS **/

// Creates LCD object// LCD object for interacting with the lcd
SerLCD lcd;
// Creates KEYPAD object
KEYPAD keypad;
// Creates accelerometer object
MMA8452Q acc;

/** END of I2C OBJECTS **/


// COMPONENT PINS
const unsigned short BUZZER = A0;
const unsigned short LEFT_PUSHBUTTON = 11;
const unsigned short RIGHT_PUSHBUTTON = 10;
const unsigned short LATCH_PUSHBUTTON = 12;
const unsigned short FLIP_SWITCH = A1;
const unsigned short KEY_SWITCH = A2;
const unsigned short GREEN_1 = 5;
const unsigned short GREEN_2 = 4;
const unsigned short RED_1 = 2;
const unsigned short RED_2 = 3;
const unsigned short YELLOW_1 = 6;
const unsigned short YELLOW_2 = 7;
const unsigned short YELLOW_3 = 8;
const unsigned short YELLOW_4 = 9;


/** START of COMPONENT variables and constants **/

// SENSITIVITY & DELAY CONSTS
const unsigned int BEEP_WIDTH = 10;
const unsigned int QUICK_BEEP_DELAY = 25;
const float ACC_X_THRESH = 0.6;
const float ACC_Y_THRESH = 0.6;
const float ACC_Z_THRESH = 1.3;

// BUZZER variables and constants
unsigned long buzzerStart;    // Stores time when buzzer was started
unsigned int totalD;        // Stores the total duration in milliseconds
unsigned int onD;          // Stores how long the buzzer should be on at any given time
unsigned int offD;          // Stores how long the buzzer should be off at any given time
bool buzzerOn;          // Bool to check whether the buzzer is currently on or off
bool buzzerOnTimerSet;      // Checks if the on timer has been set
bool buzzerOffTimerSet;     // Chacks if the off timer has been set

// GENERAL const
const bool OFF = false;     // Constant that maps OFF to false
const bool ON = true;       // Constant that maps ON to true

// LCD Line Buffers
char line1[17];             // Character array for the first line of LCD
char line2[17];             // Character array for the second line of LCD

/** END of COMPONENT variables and constants **/



/** START of MENU & GAME functions **/

// Main menu function
void menu();

// Game mode functions
void CSGO();
void KOTH();
void R6S();
void RECO();

/** END of MENU & GAME functions **/



/** START of COMPONENT functions **/

// LCD functions
void wl1(String);
void wl2(String);
void wls(String, String);
void updateLcdTimer(unsigned long startTime, unsigned int totalTime);
void progressBar(unsigned short);
void deprogressBar(unsigned short);
void updateDisplay();

// KEYPAD functions
char getKeypad();
char waitForKeypad();
void waitForKeypadVoid();

// BUZZER functions
void buzzerSet(unsigned int, unsigned int);
void buzzer();

// Miscellaneous COMPONENT functions
bool hasMoved();
bool buttonPressed(unsigned short);
bool switchOn(unsigned short);
bool bothButtonsPressed();
void RED(bool);
void GREEN(bool);
void YELLOW(bool);
void allOff();
void keypadLights();

/** END of COMPONENT functions **/



/** START of miscellaneous GAME functions **/

// In Game Functions
bool quit(int&);
bool quit3(int&, unsigned long&, unsigned long&);
bool OOT(unsigned long, unsigned int);

/** END of MENU & GAME functions **/


// Called once when arduino is powerd up
void setup() {
  // Initializes wire object
  Wire.begin();
  lcd.begin(Wire); //By default .begin() will set I2C SCL to Standard Speed mode of 100kHz
  Wire.setClock(400000); //Optional - set I2C SCL to High Speed Mode of 400kHz
  lcd.setBacklight(255, 255, 255); //bright white
  lcd.clear();

  // Initializes KEYPAD
  if (keypad.begin() == false) {
    wl1("ERROR: keypad");
    wl2("not detected");
    updateDisplay();
    while (!keypad.isConnected());
  }

  // Initializes ACCELEROMETER
  if (acc.begin() == false) {
    wl1("ERROR: acc not");
    wl2("detected");
    updateDisplay();
    while (!acc.begin());
  }

  // PIN SETUP
  pinMode(BUZZER, OUTPUT);
  pinMode(LEFT_PUSHBUTTON, INPUT);
  pinMode(RIGHT_PUSHBUTTON, INPUT);
  pinMode(LATCH_PUSHBUTTON, INPUT);
  pinMode(FLIP_SWITCH, INPUT);
  pinMode(KEY_SWITCH, INPUT);
  pinMode(GREEN_1, OUTPUT);
  pinMode(GREEN_2, OUTPUT);
  pinMode(RED_1, OUTPUT);
  pinMode(RED_2, OUTPUT);
  pinMode(YELLOW_1, OUTPUT);
  pinMode(YELLOW_2, OUTPUT);
  pinMode(YELLOW_3, OUTPUT);
  pinMode(YELLOW_4, OUTPUT);
  
}

// Loops after setup is finished
void loop() {
  // Loops the main menu
  menu();
}


/** START of MENU & GAME functions **/

// Main menu function to call gamemode functions
void menu() {
  digitalWrite(BUZZER, LOW);
  allOff();
  keypadLights();
  // Displays the 4 different game types
  wl1("1:CSGO   2:KOTH");
  wl2("3:R6S    4:RECO");
  updateDisplay();
  // Gets button for menu selection
  char button = waitForKeypad();
  switch (button) {
    case '1':
      CSGO();
      break;
    case '2':
      KOTH();
      break;
    case '3':
      R6S();
      break;
    case '4':
      RECO();
      break;
  }
}

// Search and Destroy gamemode function from Counter Strike: Global Offensive
void CSGO() {
  // Game Time Values
  int plantT, diffuseT, armT;

  // Bool to check if game has started
  bool start = false;
  if (!start) {
    // uint for settings menus
    unsigned short section = 0;

    // Cursor selectors
    int pc = 0;
    int dc = 0;
    int ac = 0;
    
    String pmh = "0";
    String pmt = "5";
    String psh = "0";
    String pst = "0";
    String dmh = "0";
    String dmt = "1";
    String dsh = "0";
    String dst = "0";
    String ash = "1";
    String ast = "0";
    
    // Digit chars

    
    
    while (!start) {
      // Buffers Section Controls to Line 2
      wl2("BACK:* NEXT:#");

      // Resets cursor locations
      pc = 0;
      dc = 0;
      ac = 0;

      // Loops based on current section of the menu
      while (section == 0) {
        // Buffers plant time to line 1 and updates screen
        wl1("Plant T: " + pmh + pmt + ":" + psh + pst);
        updateDisplay();
        
        // Blinks cursor based on which digit is being updated
        if (pc == 0) {
            lcd.setCursor(9, 0);
            lcd.blink();
        } else if (pc == 1) {
            lcd.setCursor(10, 0);
            lcd.blink();
        } else if (pc == 2) {
            lcd.setCursor(12, 0);
            lcd.blink();
        } else if (pc == 3) {
            lcd.setCursor(13, 0);
            lcd.blink();
        }
        
        // Waits for valid button press to continue
        char button = waitForKeypad();

        // If on section 0 and * is pressed, returns to main menu
        // else decrements section number
        if (button == '*') {
          if (section == 0) return;
          else section--;
        }
        // If button is #, move to next section
        else if (button == '#') section++;

        // If button is a digit, sets digit based on where the cursor is
        else {
          if (pc == 0) pmh = String(button);
          else if (pc == 1) pmt = String(button);
          else if (pc == 2) psh = String(button);
          else if (pc == 3) pst = String(button);
          if (pc == 3) pc = 0;
          else pc++;
        }
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 1) {
        // Buffers plant time to line 1 and updates screen
        wl1("Diffuse T: " + dmh + dmt + ":" + dsh + dst);
        updateDisplay();

        // Blinks cursor based on which digit is being updated
        if (dc == 0) {
          lcd.setCursor(11, 0);
          lcd.blink();
        } else if (dc == 1) {
          lcd.setCursor(12, 0);
          lcd.blink();
        } else if (dc == 2) {
          lcd.setCursor(14, 0);
          lcd.blink();
        } else if (dc == 3) {
          lcd.setCursor(15, 0);
          lcd.blink();
        }

        // Waits for valid button press to continue
        char button = waitForKeypad();

        // If on section 0 and * is pressed, returns to main menu
        // else decrements section number
        if (button == '*') {
          if (section == 0) return;
          else section--;
        }
        // If button is #, move to next section
        else if (button == '#') section++;
        // If button is a digit, sets digit based on where the cursor is
        else {
          if (dc == 0) dmh = String(button);
          else if (dc == 1) dmt = String(button);
          else if (dc == 2) dsh = String(button);
          else if (dc == 3) dst = String(button);
          if (dc == 3) dc = 0;
          else dc++;
        }
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 2) {
        // Buffers plant time to line 1 and updates screen
        wl1("Arm T: " + ash + ast + " sec");
        updateDisplay();

        // Blinks cursor based on which digit is being updated
        if (ac == 0) {
          lcd.setCursor(7, 0);
          lcd.blink();
        } else if (ac == 1) {
          lcd.setCursor(8, 0);
          lcd.blink();
        }

        // Waits for valid button press to continue
        char button = waitForKeypad();

        // If on section 0 and * is pressed, returns to main menu
        // else decrements section number
        if (button == '*') {
          if (section == 0) return;
          else section--;
        }
        // If button is #, move to next section
        else if (button == '#') section++;
        // If button is a digit, sets digit based on where the cursor is
        else {
          if (ac == 0) ash = String(button);
          else if (ac == 1) ast = String(button);
          if (ac == 1) ac = 0;
          else ac++;
        }
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 3) {
        wl1("P-" + String(pmh[0]) + String(pmt[0]) + ":" + String(psh[0]) + String(pst[0]) + ",D-" + String(dmh[0]) + String(dmt[0]) + ":" + String(dsh[0]) + String(dst[0]));
        wl2("A-" + String(ash[0]) + String(ast[0]) + "    START:#");
        updateDisplay();
        char button = waitForKeypad();
        if (button == '#') section++;
        else if (button == '*') section--;
      }
      if (section == 4) {
        // Makes sure that the flip switch is in the off position
        if (switchOn(FLIP_SWITCH)) {
          wls("Please turn off", "flip switch");
          updateDisplay();
          while (switchOn(FLIP_SWITCH) && section == 4) {
            char button = getKeypad();
            if (button == '*') section--;
            keypadLights();
          }
        }
        if (section == 4) {
          // Updates plant time based on digits
          plantT = (pmh.toInt() * 600) + (pmt.toInt() * 60) + (psh.toInt() * 10) + (pst.toInt());
          diffuseT = (dmh.toInt() * 600) + (dmt.toInt() * 60) + (dsh.toInt() * 10) + (dst.toInt());
          armT = (ash.toInt() * 10) + (ast.toInt());
          
          // Starts the game
          start = true;
        }
      }
    }
  }
  
  const short PLANT = 1;
  const short DIFFUSE = 2;
  const short POOT = 3;
  const short DOOT = 4;
  const short DIFFUSED = 5;
  const short MOVED = 6;
  const short SWITCHED = 7;
  
  // Sets the game phase to 1
  unsigned short phase = PLANT;
  
  if (phase == PLANT) {
    // Start time for phase 1
    unsigned long plantST = millis();
    
    GREEN(ON);
    
    bool doubleBeep = true;
    bool tripleBeep = true;
    
    while (phase == PLANT) {
      
      keypadLights();
      
      // Plays a beep twice a second if there is 1m left
      if (((plantT <= 60) || ((floor((millis()-plantST)/1000)) > (plantT-60))) && doubleBeep) {
        quickBeep();
        delay(200);
        quickBeep();
        doubleBeep = false;
      }
      
      // Plays a beep 5 times a second if there is 30s left
      if (((plantT <= 30) || ((floor((millis()-plantST)/1000)) > (plantT-30))) && tripleBeep) {
        quickBeep();
        delay(200);
        quickBeep();
        delay(200);
        quickBeep();
        tripleBeep = false;
      }
      
      // If quit then return to main menu
      char b = getKeypad();
      if (b == '*' || b == '#') {
        if (quit(plantT)) return;
      };
      
      // Updates bomb timer
      updateLcdTimer(plantST, plantT);
      wl2("");
      updateDisplay();

      // If time runs out GOTO phase 3
      if (OOT(plantST, plantT)) phase = POOT;
      
      if (switchOn(FLIP_SWITCH)) {
        
        // If the bomb has moved GOTO phase 0 (BOOM)
        if (hasMoved()) phase = MOVED;
        
        if (bothButtonsPressed()) {
          unsigned long buttonST = millis();
          unsigned int buttonTT = armT*1000;
          while (bothButtonsPressed() && phase == PLANT) {
            
            keypadLights();
            
            // Updates countdown timer buffer
            updateLcdTimer(plantST, plantT);
            
            // Calculated percentage (1-10) progress of arming
            unsigned short percent = map(((millis()-buttonST)%buttonTT)/1000, 0, armT, 0, 10);
            
            // Buffers progress bar based on percentage 
            progressBar(percent);
            
            // Updates the display to reflect timer and progress bar
            updateDisplay();
            
            // If the bomb has moved GOTO phase 0 (BOOM)
            if (hasMoved()) phase = MOVED;
            
            // If the plant time has run out GOTO phase 3 (OOT)
            if (OOT(plantST, plantT)) phase = POOT;
            
            // If the percentage is completed 
            if ((millis()-buttonST)>buttonTT) phase = DIFFUSE;
          }
        }
      }
      
      
    } 
    
  }
  if (phase == DIFFUSE) {
    
    RED(ON);
    GREEN(OFF);
    
    unsigned long diffST = millis();
    
    buzzerSet(1950, 50);
    
    bool speed1 = true;
    bool speed2 = true;
    
    while (phase == DIFFUSE) {
      
      keypadLights();
      
      buzzer();
      
      // Plays a beep twice a second if there is 1m left
      
      if (speed1) {
        if ((diffuseT-((millis()-diffST)/1000))<60) {
          buzzerSet(450, 50);
          speed1 = false;
        }
      }
      
      // Plays a beep 5 times a second if there is 30s left
      if (speed2) {
        if ((diffuseT-((millis()-diffST)/1000))<30) {
          buzzerSet(50, 50);
          speed2 = false;
        }
      }
      
      // If quit then return to main menu
      char b = getKeypad();
      if (b == '*' || b == '#') {
        if (quit(diffuseT)) return;
      };
      
      if (!switchOn(FLIP_SWITCH)) phase = SWITCHED;
      
      // Updates bomb timer
      updateLcdTimer(diffST, diffuseT);
      wl2("");
      updateDisplay();
      
      // If time runs out GOTO phase 3
      if (OOT(diffST, diffuseT)) phase = DOOT;
      
      if (bothButtonsPressed() && (millis()-diffST > 1000)) {
        unsigned long buttonST = millis();
        unsigned int buttonTT = armT*1000;
        while (bothButtonsPressed() && phase == DIFFUSE) {
          
          if (speed1) {
            if ((diffuseT-((millis()-diffST)/1000))<=60) {
              buzzerSet(450, 50);
              speed1 = false;
            }
          }
          
          // Plays a beep 5 times a second if there is 30s left
          if (speed2) {
            if ((diffuseT-((millis()-diffST)/1000))<=30) {
              buzzerSet(150, 50);
              speed2 = false;
            }
          }
          
          buzzer();
          
          keypadLights();
          
          // Updates countdown timer buffer
          updateLcdTimer(diffST, diffuseT);
            
          // Calculated percentage (1-10) progress of arming
          unsigned short percent = map(((millis()-buttonST)%buttonTT)/1000, 0, armT, 0, 10);
            
          // Buffers progress bar based on percentage 
          progressBar(percent);
            
          // Updates the display to reflect timer and progress bar
          updateDisplay();
            
          // If the bomb has moved GOTO phase 0 (BOOM)
          if (hasMoved()) phase = MOVED;
            
          // If the plant time has run out GOTO phase 3 (OOT)
          if (OOT(diffST, diffuseT)) phase = DOOT;
            
          // If the percentage is completed 
          if ((millis()-buttonST)>buttonTT) phase = DIFFUSED;
        }
      }
      
    }
    
  }
  if (phase == POOT) {
    digitalWrite(BUZZER, LOW);
    wl1("OUT OF TIME*CT");
    wl2("WIN---PUSH KEY");
    updateDisplay();
    waitForKeypadVoid();
    return;
  }
  if (phase == DOOT) {
    digitalWrite(BUZZER, HIGH);
    wl1("!BOMB EXPLODED!!");
    wl2("!!!!!!!!!!!!!!!!");
    updateDisplay();
    delay(3000);
    digitalWrite(BUZZER, LOW);
    RED(OFF);
    GREEN(ON);
    wl1("TERRORISTS WIN--");
    wl2("--------PUSH KEY");
    updateDisplay();
    waitForKeypadVoid();
    return;
  }
  if (phase == DIFFUSED) {
    digitalWrite(BUZZER, LOW);
    RED(OFF);
    GREEN(ON);
    wl1("!BOMB DIFFUSED!!");
    wl2("!!!!!!!!!!!!!!!!");
    updateDisplay();
    for (int i=0; i<3; i++) {
      digitalWrite(BUZZER, HIGH);
      delay(50);
      digitalWrite(BUZZER, LOW);
      delay(950);
    }
    wl1("CT WIN----------");
    wl2("--------PUSH KEY");
    updateDisplay();
    waitForKeypadVoid();
  }
  if (phase == MOVED) {
    digitalWrite(BUZZER, LOW);
    RED(ON);
    GREEN(OFF);
    digitalWrite(BUZZER, HIGH);
    wl1("!BOMB EXPLODED!!");
    wl2("!!!!!!!!!!!!!!!!");
    updateDisplay();
    delay(3000);
    digitalWrite(BUZZER, LOW);
    RED(OFF);
    GREEN(ON);
    wl1("BOMB WAS MOVED--");
    wl2("--------PUSH KEY");
    updateDisplay();
    waitForKeypadVoid();
    return;
  }
  if (phase == SWITCHED) {
    digitalWrite(BUZZER, LOW);
    RED(ON);
    GREEN(OFF);
    digitalWrite(BUZZER, HIGH);
    wl1("!BOMB EXPLODED!!");
    wl2("!!!!!!!!!!!!!!!!");
    updateDisplay();
    delay(3000);
    digitalWrite(BUZZER, LOW);
    RED(OFF);
    GREEN(ON);
    wl1("FLIPSWITCH TURNE");
    wl2("D OFF---PUSH KEY");
    updateDisplay();
    waitForKeypadVoid();
    return;
  }
  
}

// King of the Hill gamemode function
void KOTH() {
  
  int gameTime, maxCaptureTime, teams;
  unsigned long winTime;
  
  bool start = false;
  
  if (!start) {
    // Short for settings menus
    unsigned short section = 0;
    
    bool firstTime = true;
    
    // Cursor selectors
    int gc = 0;
    int wc = 0;
    int cc = 0;
    
    String gmh = "1";
    String gmt = "0";
    String gsh = "0";
    String gst = "0";
    String wmh = "0";
    String wmt = "2";
    String wsh = "3";
    String wst = "0";
    String csh = "1";
    String cst = "5";
    String tms = "2";
    
    while (!start) {
      // Buffers section control to line 2
      wl2("BACK:* NEXT:#");
      
      // Reset cursor locations
      gc = 0;
      wc = 0;
      cc = 0;
      
      while (section == 0) {
        // Buffers game time to line 1 and updates screen
        wl1("Game T: " + gmh + gmt + ":" + gsh + gst);
        updateDisplay();
        
        // Blinks cursor based on which digit is being updated
        if (gc == 0) {
          lcd.setCursor(8, 0);
          lcd.blink();
        } else if (gc == 1) {
          lcd.setCursor(9, 0);
          lcd.blink();
        } else if (gc == 2) {
          lcd.setCursor(11, 0);
          lcd.blink();
        } else if (gc == 3) {
          lcd.setCursor(12, 0);
          lcd.blink();
        }
        if (firstTime) {
          delay(500);
          firstTime = false;
        }
        // Waits for valid button press to continue
        char button = waitForKeypad();
        
        // Responds to button press
        if (button == '*') return;
        else if (button == '#') section++;
        else {
          if (gc == 0) gmh = String(button);
          else if (gc == 1) gmt = String(button);
          else if (gc == 2) gsh = String(button);
          else if (gc == 3) gst = String(button);
          if (gc == 3) gc = 0;
          else gc++;
        }
        // Disables blink for next screen
        lcd.noBlink();
      }
      while (section == 1) {
        // Buffers win time to line 1 and updates screen
        wl1("Win T: " + wmh + wmt + ":" + wsh + wst);
        updateDisplay();
        
        // Blinks cursor based on which digit is being updated
        if (wc == 0) {
          lcd.setCursor(7, 0);
          lcd.blink();
        } else if (wc == 1) {
          lcd.setCursor(8, 0);
          lcd.blink();
        } else if (wc == 2) {
          lcd.setCursor(10, 0);
          lcd.blink();
        } else if (wc == 3) {
          lcd.setCursor(11, 0);
          lcd.blink();
        }
        
        // Waits for valid button press to continue
        char button = waitForKeypad();
        
        // Responds to button press
        if (button == '*') section--;
        else if (button == '#') section++;
        else {
          if (wc == 0) wmh = String(button);
          else if (wc == 1) wmt = String(button);
          else if (wc == 2) wsh = String(button);
          else if (wc == 3) wst = String(button);
          if (wc == 3) wc = 0;
          else wc++;
        }
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 2) {
        // Buffers max capture time to line 1 and updates screen
        wl1("Max Capt T: " + csh + cst);
        updateDisplay();
        
        // Blinks cursor based on which digit is being updated
        if (cc == 0) {
          lcd.setCursor(12, 0);
          lcd.blink();
        } else if (cc == 1) {
          lcd.setCursor(13, 0);
          lcd.blink();
        }
        
        // Wait for valid button press to continue
        char button = waitForKeypad();
        
        // Responds to button press
        if (button == '*') section--;
        else if (button == '#') section++;
        else {
          if (cc == 0) csh = String(button);
          else if (cc == 1) cst = String(button);
          if (cc == 1) cc = 0;
          else cc = 1;
        }
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 3) {
        // Buffers win time to line 1 and updates screen
        wl1("# of Teams: " + tms);
        updateDisplay();
        
        // Blinks curser at teams location
        lcd.setCursor(12, 0);
        lcd.blink();
        
        // Waits for valid button press to continue
        char button = waitForKeypad();
        
        // Responds to button press
        if (button == '*') section--;
        else if (button == '#') section++;
        else if (button == '0' || button == '1') {
          wl1("Min Teams must");
          wl2("be 2");
          updateDisplay();
          delay(3000);
        }
        else tms = String(button);
        // Disables blink for next screen write
        lcd.noBlink();
      }
      while (section == 4) {
        wl1("G-" + gmh + gmt + ":" + gsh + gst + ",W-" + wmh + wmt + ":" + wsh + wst);
        wl2("C-" + csh + cst + ",T-" + tms + " START:#");
        updateDisplay();
        char button = waitForKeypad();
        if (button == '#') section++;
        else if (button == '*') section--;
        
        if (section == 5) {
          gameTime = (gmh.toInt() * 600) + (gmt.toInt() * 60) + (gsh.toInt() * 10) + gst.toInt();
          winTime = (wmh.toInt() * 600) + (wmt.toInt() * 60) + (wsh.toInt() * 10) + wst.toInt();
          maxCaptureTime = (csh.toInt() * 10) + cst.toInt();
          teams = tms.toInt();
          
          // Starts the game
          start = true;
        }
      }
    }
    
    // Int array for keeping track of team scores and initializes them to 0
    unsigned int tScore[teams];
    for (int i=0; i<teams; i++) {
      tScore[i] = 0;
    }
    
    // Phase constants
    const short GAME = 1;   // Main game phase
    const short ROOT = 2;    // Total gime time ran out
    const short WBS = 3;    // A team has won by their score
    const short MOVED = 4;
    
    short currentTeam = -1;
    
    // Sets the phase to game
    unsigned short phase = GAME;
    
    unsigned int winningTeam = NULL;
    bool winningTeams[teams];
      
      
    // GAME phase
    if (phase == GAME) {
      // Start time for game phase
      unsigned long gameST = millis();
      
      bool doubleBeep = true;
      bool tripleBeep = true;
      
      buzzerSet(950, 50);
      
      winTime *= 1000;
      
      while (phase == GAME) {
        
        GREEN(ON);
        RED(OFF);
        
        digitalWrite(BUZZER, LOW);
        
        keypadLights();
        
        // Double beeps when 90 seconds are left
        if (doubleBeep) {
          if ((gameTime-((millis()-gameST)/1000))<90) {
            quickBeep();
            delay(200);
            quickBeep();
            doubleBeep = false;
          }
        }
        
        // Triple beeps when 45 seconds are left
        if (tripleBeep) {
          if ((gameTime-((millis()-gameST)/1000))<45) {
            quickBeep();
            delay(200);
            quickBeep();
            delay(200);
            quickBeep();
            tripleBeep = false;
          }
        }
        
        // If quit then return to main menu
        char b = getKeypad();
        if (b == '*' || b == '#') {
          if (quit(gameTime)) return;
        } else {
          String select = String(b);
          if (select != NULL) {
            unsigned short teamSelection = select.toInt();
            if (teamSelection < teams) currentTeam = teamSelection;
          }
        }
        
        
        // Updates game timer and shows current team
        updateLcdTimer(gameST, gameTime);
        if (currentTeam == -1) wl2("No Current Team");
        else wl2("T: " + String(currentTeam) + ", S: " + String(tScore[currentTeam]));
        updateDisplay();
        
        if (OOT(gameST, gameTime)) phase = ROOT;
        
        // If the bomb has moved GOTO phase MOVED
        if (hasMoved()) phase = MOVED;
        
        if ((buttonPressed(LEFT_PUSHBUTTON) || buttonPressed(RIGHT_PUSHBUTTON)) && currentTeam != -1) {
          unsigned long captureST = millis();
          unsigned long maxCaptureST = millis();
          unsigned short newTeam = currentTeam;
          unsigned int score = 0;
          bool outOfCaptureTime = false;
          while ((!outOfCaptureTime && (currentTeam == newTeam)) && phase == GAME) {
            RED(ON);
            GREEN(OFF);
            
            while (((buttonPressed(LEFT_PUSHBUTTON) || buttonPressed(RIGHT_PUSHBUTTON)) && (currentTeam == newTeam)) && phase == GAME) {
              // Updates game timer and shows progress bar showing remaining time
              updateLcdTimer(gameST, gameTime);
              deprogressBar(0);
              updateDisplay();
              
              buzzer();
              
              if (OOT(gameST, gameTime)) phase = ROOT;
              
              // Continues to reset the timer for the maxCapture time
              maxCaptureST = millis();
              
              keypadLights();
              
              // Double beeps when 90 seconds are left
              if (doubleBeep) {
                if ((gameTime-((millis()-gameST)/1000))<90) {
                  quickBeep();
                  delay(200);
                  quickBeep();
                  doubleBeep = false;
                }
              }
              
              // Triple beeps when 45 seconds are left
              if (tripleBeep) {
                if ((gameTime-((millis()-gameST)/1000))<45) {
                  quickBeep();
                  delay(200);
                  quickBeep();
                  delay(200);
                  quickBeep();
                  tripleBeep = false;
                }
              }
              // If the current team's score is greater than the required score, it goes to win phase
              if ((tScore[currentTeam] + (millis()-captureST)) >= winTime) {
                phase = WBS;
                winningTeam = currentTeam;
              }
              
              // If quit then return to main menu
              char b = getKeypad();
              if (b == '*' || b == '#') {
                if (quit3(gameTime, captureST, maxCaptureST)) return;
              } else {
                String selection = String(b);
                if (selection != NULL) {
                  unsigned short teamSelection = selection.toInt();
                  if (teamSelection < teams) newTeam = teamSelection;
                }
              }
              
              // If the bomb has moved GOTO phase MOVED
              if (hasMoved()) phase = MOVED;
              
            }
            
            buzzer();
            
            // If the current team's score is greater than the required score, it goes to win phase
            if ((tScore[currentTeam] + (millis()-captureST)) >= winTime) {
              phase = WBS;
              winningTeam = currentTeam;
            }
            // If the time has run out for capture time then stop loop
            if (((millis()-maxCaptureST)/1000) >= maxCaptureTime) outOfCaptureTime = true;
            
            // If quit then return to main menu
            char b = getKeypad();
            if (b == '*' || b == '#') {
              if (quit3(gameTime, captureST, maxCaptureST)) return;
            } else {
              String selection = String(b);
              if (selection != NULL) {
                unsigned short teamSelection = selection.toInt();
                if (teamSelection < teams) newTeam = teamSelection;
              }
            }
            
            // Double beeps when 90 seconds are left
            if (doubleBeep) {
              if ((gameTime-((millis()-gameST)/1000))<90) {
                quickBeep();
                delay(200);
                quickBeep();
                doubleBeep = false;
              }
            }
            
            // Triple beeps when 45 seconds are left
            if (tripleBeep) {
              if ((gameTime-((millis()-gameST)/1000))<45) {
                quickBeep();
                delay(200);
                quickBeep();
                delay(200);
                quickBeep();
                tripleBeep = false;
              }
            }
            
            if (OOT(gameST, gameTime)) phase = ROOT;
            
            unsigned short percent = map((((millis()-maxCaptureST))%(maxCaptureTime*1000))/1000, 0, maxCaptureTime, 0, 10);
            
            // Updates game timer and shows progress bar showing remaining time
            updateLcdTimer(gameST, gameTime);
            if (!outOfCaptureTime) deprogressBar(percent);
            updateDisplay();
            
            // If the bomb has moved GOTO phase MOVED
            if (hasMoved()) phase = MOVED;
            
          }
          
          // Gives the team the score for the duration of their capture
          tScore[currentTeam] += (millis()-captureST);
          // If there was a team change, updates the new team
          currentTeam = newTeam;
          
        }
        
        // Checks to make sure that no team has reached required score;
        for (int i=0; i<teams; i++) {
          if (tScore[i] >= winTime) {
            phase = WBS;
            winningTeam = i;
          }
        }
        
      }
      
    }
    if (phase == ROOT) {
      digitalWrite(BUZZER, LOW);
      GREEN(ON);
      RED(OFF);
      unsigned long highscore = 0;
      String winLn1 = "T:";
      for (int i=0; i<teams; i++) {
        if (tScore[i] > highscore) highscore = tScore[i];
      }
      for (int i=0; i<teams; i++) {
        if (tScore[i] == highscore) winningTeams[i] = true;
        else winningTeams[i] = false;
      }
      for (int i=0; i<teams; i++) {
        if (winningTeams[i]) winLn1 += (" " + String(i));
      }
      wl1(winLn1);
      wl2("WINS. Any key...");
      updateDisplay();
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(1100);
      digitalWrite(BUZZER, LOW);
      waitForKeypadVoid();
    }
    if (phase == WBS) {
      digitalWrite(BUZZER, LOW);
      GREEN(ON);
      RED(OFF);
      wl1("TEAM " + String(winningTeam) + " WINS!");
      wl2("Any key to cont.");
      updateDisplay();
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(1100);
      digitalWrite(BUZZER, LOW);
      waitForKeypadVoid();
    }
    if (phase == MOVED) {
      digitalWrite(BUZZER, LOW);
      GREEN(OFF);
      RED(ON);
      wl1("DEVICE WAS MOVED");
      if (currentTeam == -1) wl2("NO CURRENT TEAM");
      else wl2("CURRENT TEAM: " + currentTeam);
      updateDisplay();
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(100);
      digitalWrite(BUZZER, LOW);
      delay(900);
      digitalWrite(BUZZER, HIGH);
      delay(1100);
      digitalWrite(BUZZER, LOW);
      waitForKeypadVoid();
    }
    
  }
  
  
}

// Bomb gamemode function from the Rainbow 6 Siege
void R6S() {
  // Integers to keep track timer for different phases
  int bombT, diffuseT, armT;
  
  // Indicates whether the match has started
  bool start = false;
  
  // Pre-game settings
  if (!start) {
    // ushort for menu selection
    unsigned short section = 0;
    
    // Cursor selectors
    int bc, dc, ac;
    
    // Strings to display and record digits for timer
    String bmh = "0";
    String bmt = "5";
    String bsh = "0";
    String bst = "0";
    String dmh = "0";
    String dmt = "1";
    String dsh = "0";
    String dst = "0";
    String ash = "1";
    String ast = "0";
    
    // Loops until match starts
    while (!start) {
      
      // Buffers section controls to line 2
      wl2("BACK:* NEXT:#");
      
      // Sets cursor locations to 0
      bc = dc = ac = 0;
      
      // First menu which controls bomb time
      while (section == 0) {
        
        // Buffers bomb time to line 1 and updates LCD
        wl1("Bomb T: " + bmh + bmt + ":" + bsh + bst);
        updateDisplay();
        
        // Blinks cursor at current digit location
        if (bc == 0) {
          lcd.setCursor(8, 0);
          lcd.blink();
        } else if (bc == 1) {
          lcd.setCursor(9, 0);
          lcd.blink();
        } else if (bc == 2) {
          lcd.setCursor(11, 0);
          lcd.blink();
        } else if (bc == 3) {
          lcd.setCursor(12, 0);
          lcd.blink();
        }
        
        // Waits for keypad input
        char button = waitForKeypad();
        
        // Handles keypad input
        if (button == '*') return;
        else if (button == '#') section++;
        else {
          if (bc == 0) bmh = String(button);
          else if (bc == 1) bmt = String(button);
          else if (bc == 2) bsh = String(button);
          else if (bc == 3) bst = String(button);
          if (bc == 3) bc = 0;
          else bc++;
        }
        
        // Disables blink
        lcd.noBlink();
        
      }
      
      // Second menu which controls difuse time
      while (section == 1) {
        
        // Buffers diffuse time to line 1 and updates LCD
        wl1("Diffuse T: " + dmh + dmt + ":" + dsh + dst);
        updateDisplay();
        
        // Blinks cursor at current digit location
        if (dc == 0) {
          lcd.setCursor(11, 0);
          lcd.blink();
        } else if (dc == 1) {
          lcd.setCursor(12, 0);
          lcd.blink();
        } else if (dc == 2) {
          lcd.setCursor(14, 0);
          lcd.blink();
        } else if (dc == 3) {
          lcd.setCursor(14, 0);
          lcd.blink();
        }
        
        // Waits for keypad input
        char button = waitForKeypad();
        
        // Handles keypad input
        if (button == '*') section--;
        else if (button == '#') section++;
        else {
          if (dc == 0) dmh = String(button);
          else if (dc == 1) dmt = String(button);
          else if (dc == 2) dsh = String(button);
          else if (dc == 3) dst = String(button);
          if (dc == 3) dc = 0;
          else dc++;
        }
        
        // Disables blink
        lcd.noBlink();
        
      }
      
      // Third menu which controls arm time
      while (section == 2) {
        
        // Buffers arm time to line 1 and updates LCD
        wl1("Arm T: " + ash + ast);
        updateDisplay();
        
        if (ac == 0) {
          lcd.setCursor(7, 0);
          lcd.blink();
        } else if (ac == 1) {
          lcd.setCursor(8, 0);
          lcd.blink();
        }
        
        // Waits for keypad input
        char button = waitForKeypad();
        
        // Handles keypad input
        if (button == '*') section--;
        else if (button == '#') section++;
        else {
          if (ac == 0) ash = String(button);
          else if (ac == 1) ast = String(button);
          if (ac == 1) ac = 0;
          else ac++;
        }
        
        // Disables blink
        lcd.noBlink();
        
      }
      
      // Fourth menu which shows an overview of match times
      while (section == 3) {
        
        // Buffers the match time stats to line 1 and 2 and updates LCD
        wl1("B-" + bmh + bmt + ":" + bsh + bst + ",D-" + dmh + dmt + ":" + dsh + dst);
        wl2("A-" + ash + ast + "     START:#");
        updateDisplay();
        
        // Waits for keypad input
        char button = waitForKeypad();
        
        // Handles keypad input
        if (button == '*') section--;
        else if (button == '*') section++;
        
      }
      
      // Checks if key switch is on and then starts the game
      if (section == 4) {
        
        // Waits for key switch to be turned off
        if (switchOn(KEY_SWITCH)) {
          
          // Tells user to turn off key switch
          wl1("Please turn off");
          wl2("key switch");
          updateDisplay();
          
          // Loops until key switch is turned off or user goes back
          while (switchOn(KEY_SWITCH) && section == 4) {
            
            // Checks for keypad light button
            keypadLights();
            
            // Continually gets keypad input
            char button = getKeypad();
            
            // If user presses *, returns to overview menu
            if (button == '*') section--;            
            
          }
          
          // If section hasn't changed, calculate times and start the match
          if (section == 4) {
            
            // Calculates durations for each phase of the match
            bombT = (bmh.toInt() * 600) + (bmt.toInt() * 60) + (bsh.toInt() * 10) + bst.toInt();
            diffuseT = (dmh.toInt() * 600) + (dmt.toInt() * 60) + (dsh.toInt() * 10) + dst.toInt();
            armT = (ash.toInt() * 10) + ast.toInt();
            
            // Starts the game
            start = true;
            
          }
          
        }
        
      }
      
    }
    
  }
  
  
  
  return;
}

// Gamemode function from Republic Commando
void RECO() {
  return;
}

/** END of MENU & GAME functions **/



// ----------------------------- //
// !!!NON-MENU FUNCTION BELOW!!! //
// ----------------------------- //
//  | 
//  |
// \_/

/** START of LCD functions **/

// Buffers a string to line1
void wl1(String line) {
  if (line.length() > 16) {
    line = "ERROR: TOO LONG";
  }
  for (int i = 0; i < 16; i++) line1[i] = ' ';
  for (int i = 0; i < line.length() && i < 16; i++) line1[i] = line[i];
}

// Buffers a string to line2
void wl2(String line) {
  if (line.length() > 16) {
    line = "ERROR: TOO LONG";
  }
  for (int i = 0; i < 16; i++) line2[i] = ' ';
  for (int i = 0; i < line.length() && i < 16; i++) line2[i] = line[i];
}

// Buffers 2 strings to both lines
void wls(String L1, String L2) {
  if (L1.length() > 16) {
    L1 = "ERROR: TOO LONG";
  }
  if (L2.length() > 16) {
    L2 = "ERROR: TOO LONG";
  }
  for (int i = 0; i < 16; i++) line1[i] = ' ';
  for (int i = 0; i < L1.length() && i < 16; i++) line1[i] = L1[i];
  for (int i = 0; i < 16; i++) line2[i] = ' ';
  for (int i = 0; i < L2.length() && i < 16; i++) line2[i] = L2[i];
}

void updateLcdTimer(unsigned long startTime, int totalTime) {
  int rTotalTime = totalTime-((millis()-startTime)/1000);
  int rMinutes;
  if (rTotalTime>=60) rMinutes = floor(rTotalTime/60);
  else rMinutes = 0;
  int rSeconds;
  if (rTotalTime > 0) rSeconds = rTotalTime - (rMinutes * 60);
  else rSeconds = 0;
  String dm, ds;
  if (rMinutes < 10 && rMinutes > 0) dm = "0" + String(rMinutes);
  else if (rMinutes >= 10) dm = String(rMinutes);
  else dm = "00";
  if (rSeconds < 10 && rSeconds > 0) ds = "0" + String(rSeconds);
  else if (rSeconds >= 10) ds = String(rSeconds);
  else ds = "00";
  wl1("     " + dm + "::" + ds);
}

// Displays a progress bar that can show 1-10 black bars
void progressBar(unsigned short perc) {
  for (int i=0; i<16; i++) {
    if (i<3) line2[i] = ' ';
    else if (i<(13-(10-perc))) line2[i] = char(255);
    else if (i<13 && !(i<(13-(10-perc)))) line2[i] = '-';
    else line2[i] = ' ';
  }
}

// Displays a progress bar that can show 1-10 black bars
void deprogressBar(unsigned short perc) {
  for (int i=0; i<16; i++) {
    if (i<3) line2[i] = ' ';
    else if (i<(13-(10-perc))) line2[i] = '-';
    else if (i<13 && !(i<(13-(10-perc)))) line2[i] = char(255);
    else line2[i] = ' ';
  }
}

// Updates the display
void updateDisplay() {
  lcd.setCursor(0, 0);
  lcd.print(line1);
  lcd.print(line2);
}

/** END of LCD functions **/



/** START of KEYPAD functions **/

// Gets and returns the most recent button press
char getKeypad() {
  // Local Variables
  char nextButt;
  char butt;
  unsigned short timeSincePressed;

  // Updates and stores the oldes item in the stack
  keypad.updateFIFO();
  nextButt = keypad.getButton();

  // Displays error message if keypad is not detected
  if (nextButt == -1) {
    wls("ERROR: keypad", "not detected");
    updateDisplay();
    while (!keypad.isConnected());
  }

  // Loops through the stack and returns the most recent button press
  do {
    butt = nextButt;
    keypad.updateFIFO();
    nextButt = keypad.getButton();
  } while (nextButt != NULL);

  // Gets the time since the button was pressed and
  // returns NULL if the press was older than a second
  timeSincePressed = keypad.getTimeSincePressed();
  if (timeSincePressed > 1000) return NULL;
  else if (butt == NULL) return NULL;
  else {
    quickBeep();
    return butt;
  }
}

// Loops until button on keypad is pressed and then returns the button
char waitForKeypad() {
  char buton;
  do {
    keypadLights();
    buton = getKeypad();
  } while (buton == NULL);
  return buton;
}

// Loops until button on keypad is pressed
void waitForKeypadVoid() {
  while (getKeypad() == NULL) keypadLights();
  quickBeep();
  return;
}

/** END of KEYPAD functios **/



/** START of BUZZER functions **/

// Sets the values for buzzer
// PARAM UNITS - frequency:beeps.per.second, width:milliseconds
void buzzerSet(unsigned int offDuration, unsigned int onDuration) {
  
  // Ensures that the buzzer starts by turning on the buzzer for the duration
  buzzerOnTimerSet = true;
  buzzerOffTimerSet = false;
  
  onD = onDuration;
  offD = offDuration;
}

// Polled to enalbe buzzer and returns true upon completion
void buzzer() {
  
  // Turns the buzzer on and starts a timer
  if (buzzerOnTimerSet) {
    buzzerStart = millis();
    buzzerOnTimerSet = false;
    digitalWrite(BUZZER, HIGH);
    buzzerOn = true;
  }
  // Turns the buzzer off and starts a timer
  else if (buzzerOffTimerSet) {
    buzzerStart = millis();
    buzzerOffTimerSet = false;
    digitalWrite(BUZZER, LOW);
    buzzerOn = false;
  }
  
  // If the buzzer is currently on
  if (buzzerOn) {
    if ((millis()-buzzerStart)>onD) buzzerOffTimerSet = true; 
    else digitalWrite(BUZZER, HIGH);
  } else {
    if ((millis()-buzzerStart)>offD) buzzerOnTimerSet = true;
    else digitalWrite(BUZZER, LOW);   
  }
  
} 

// Makes a quick beep for keypad inputs
void quickBeep() {
  digitalWrite(BUZZER, HIGH);
  delay(QUICK_BEEP_DELAY);
  digitalWrite(BUZZER, LOW);
  delay(QUICK_BEEP_DELAY);
}

/** END of BUZZER functions **/



/** START of miscellaneous COMPONENT functions **/

// Returns true if accelerometer data exceeds preset value
bool hasMoved() {
  float avgX = 0;
  float avgY = 0;
  float avgZ = 0;
  if (acc.available()) {
    int i = 1;
    for (; i <= 5; i++) {
      avgX += abs(acc.getCalculatedX());
      avgY += abs(acc.getCalculatedY());
      avgZ += abs(acc.getCalculatedZ());
    }
    avgX /= i;
    avgY /= i;
    avgZ /= i;
  }
  if ((avgX > ACC_X_THRESH) || (avgY > ACC_Y_THRESH) || (avgZ > ACC_Z_THRESH)) return true;
  else return false;
}

// Returns true if button is pressed
bool buttonPressed(unsigned short buttonPin) {
  if (digitalRead(buttonPin) == HIGH) return true;
  else return false;
}

// Return true if switch is on
bool switchOn(unsigned short switchPin) {
  if (digitalRead(switchPin) == HIGH) return true;
  else return false;
}

// Returns true if both pushbuttons are pressed
bool bothButtonsPressed() {
  if (buttonPressed(LEFT_PUSHBUTTON) && buttonPressed(RIGHT_PUSHBUTTON)) return true;
  else return false;
}

// Turns both red LEDs on or off 
void RED(bool state) {
  if (state == ON) {
    digitalWrite(RED_1, HIGH);
    digitalWrite(RED_2, HIGH);
  } else {
    digitalWrite(RED_1, LOW);
    digitalWrite(RED_2, LOW);
  }
}

// Turns both green LEDs on or off 
void GREEN(bool state) {
  if (state == ON) {
    digitalWrite(GREEN_1, HIGH);
    digitalWrite(GREEN_2, HIGH);
  } else {
    digitalWrite(GREEN_1, LOW);
    digitalWrite(GREEN_2, LOW);
  }
}

// Turns all yellow LEDs on or off 
void YELLOW(bool state) {
  if (state == ON) {
    digitalWrite(YELLOW_1, HIGH);
    digitalWrite(YELLOW_2, HIGH);
    digitalWrite(YELLOW_3, HIGH);
    digitalWrite(YELLOW_4, HIGH);
  } else {
    digitalWrite(YELLOW_1, LOW);
    digitalWrite(YELLOW_2, LOW);
    digitalWrite(YELLOW_3, LOW);
    digitalWrite(YELLOW_4, LOW);
  }
}

// Turns both green and red LEDs off
void allOff() {
  RED(OFF);
  GREEN(OFF);
}

// Turns on keypad yellow LEDs if latching pushbutton is pressed
void keypadLights() {
  if (buttonPressed(LATCH_PUSHBUTTON)) YELLOW(ON);
  else YELLOW(OFF);
}

/** END of miscellaneous COMPONENT functions **/



/** START of miscellaneous GAME functions **/
bool OOT(unsigned long startTime, int totalTime) {
  if (((millis()-startTime)/1000)>totalTime) return true;
  else return false;
}

bool quit(int &duration) {
  digitalWrite(BUZZER, LOW);
  unsigned long pauseStartTime = millis();
  wl1("!!!!!Paused!!!!!");
  wl2("*:Resume  #:Quit");
  updateDisplay();
  delay(250);
  char button;
  do {
    button = waitForKeypad();
  } while (button != '*' && button != '#');
  if (button == '*') {
    duration += (millis() - pauseStartTime)/1000;
    return false;
  }
  else if (button == '#') {
    duration += (millis() - pauseStartTime)/1000;
    return true;
  }
}

bool quit3(int &duration, unsigned long &start1, unsigned long &start2) {
  digitalWrite(BUZZER, LOW);
  unsigned long pauseStartTime = millis();
  wl1("!!!!!Paused!!!!!");
  wl2("*:Resume  #:Quit");
  updateDisplay();
  delay(250);
  char button;
  do {
    button = waitForKeypad();
  } while (button != '*' && button != '#');
  if (button == '*') {
    duration += (millis() - pauseStartTime)/1000;
    start1 += (millis() - pauseStartTime);
    start2 += (millis() - pauseStartTime);
    return false;
  }
  else if (button == '#') {
    duration += (millis() - pauseStartTime)/1000;
    start1 += (millis() - pauseStartTime);
    start2 += (millis() - pauseStartTime);
    return true;
  }
}

/** END of miscellaneous LOGIC functions **/
