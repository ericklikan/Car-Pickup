#include <Servo.h>

int motor2 = 25;
int motor3 = 24;
int motor4 = 18;
int motor5 = 17;

int led1 = 6;
int led2 = 7;
int beep = 22;

const int sen1 = 30;
const int sen2 = 31;

const int but1 = 12;
Servo myservo;
int pos = 1;
void setup() {

  pinMode (motor2, OUTPUT);
  pinMode (motor3, OUTPUT);
  pinMode (motor4, OUTPUT);
  pinMode (motor5, OUTPUT);

  pinMode (sen1, INPUT);
  pinMode (sen2, INPUT);
  
  pinMode(but1,INPUT);
  
  pinMode (led1, OUTPUT);
  pinMode (led2, OUTPUT);
  pinMode (beep, OUTPUT);
  
  myservo.attach(53);
}

void forward();
void right();
void backward();
void ccw();
void beeptwice();
void stopM();
void spin();
void pickup();
void drop();

int butState1 = 0;

int senState1 = 1;
int senState2 = 1;

boolean work = false;
boolean pickup1 = false;
void loop() { 
  digitalWrite(beep,LOW);
  senState1 = digitalRead(sen1);
  senState2 = digitalRead(sen2);
  butState1 = digitalRead(but1);
  if(pickup1 == false){
    myservo.write(0);
  }
  else{
    myservo.write(30);
  }
  
  if(butState1 == true){
    work = true;
  }
  
  if(work == true){
    if(senState1 == 1 && senState2 == 1){
      backward();
    }
    else if(senState1 == 1 && senState2 == 0){
      right();
    }
    else if(senState1 == 0 && senState2 == 1){
      left();
    }
    else if(senState1 == 0 && senState2 == 0){
      if(pickup1 == false){
        pickup1 = true;
        stopM();
        pickup();
        spin();
      }
      else{
        pickup1 = false;
        stopM();
        drop();
        spin();
      }
    }
  }
  /*else{
    stopM();
  */
  /*digitalWrite(beep,LOW);
  buttonState1 = digitalRead(but1);
  if (buttonState1 == HIGH){
    backward();
    beeptwice();
    ccw();
    beeptwice();
    backward();
    beeptwice();
  }
  */

}
// forwards
void forward(){
  digitalWrite (motor3, HIGH);
  digitalWrite (motor5, HIGH);
  digitalWrite (motor2, LOW);
  digitalWrite (motor4, LOW);
  /*delay (3000);
  digitalWrite (motor3, LOW);
  digitalWrite (motor5, LOW);
  digitalWrite (motor2, LOW);
  digitalWrite (motor4, LOW);
  */
}
//rotate cw
void right(){
  digitalWrite (motor4, LOW);
  digitalWrite (motor5, LOW);
  digitalWrite (motor2, LOW);
  digitalWrite (motor3, HIGH);
}
//backwards
void backward(){
  digitalWrite (motor2, HIGH);
  digitalWrite (motor4, HIGH);
  digitalWrite (motor3, LOW);
  digitalWrite (motor5, LOW);
  /*delay (3000);
  digitalWrite (motor2, LOW);
  digitalWrite (motor4, LOW);
  digitalWrite (motor3, LOW);
  digitalWrite (motor5, LOW);
  */
}
//rotate ccw
void left(){
  digitalWrite (motor4, LOW);
  digitalWrite (motor5, HIGH);
  digitalWrite (motor2, LOW);
  digitalWrite (motor3, LOW);
}

void beeptwice(){
    digitalWrite (led1, HIGH);
    digitalWrite (led2, HIGH);
    digitalWrite (beep, HIGH);
    delay(200);
    digitalWrite (led1, LOW);
    digitalWrite (led2, LOW);
    digitalWrite (beep, LOW);   
}

void stopM(){
  digitalWrite (motor4, LOW);
  digitalWrite (motor2, LOW);
  digitalWrite (motor5, LOW);
  digitalWrite (motor3, LOW);  
}
void spin(){
  forward();
  delay(1000);
  digitalWrite (motor4, HIGH);
  digitalWrite (motor2, LOW);
  digitalWrite (motor5, LOW);
  digitalWrite (motor3, HIGH); 
  beeptwice();
  delay(1200);
  digitalWrite (motor4, HIGH);
  digitalWrite (motor2, HIGH);
  digitalWrite (motor5, LOW);
  digitalWrite (motor3, LOW); 
  delay(300);
  digitalWrite (motor4, LOW);
  digitalWrite (motor2, LOW);
  digitalWrite (motor5, LOW);
  digitalWrite (motor3, LOW); 
}

void pickup(){
    for (pos = 0; pos <= 30; pos ++) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15); 
    }    // waits 15ms for the servo to reach the position
}

void drop(){
    for (pos = 30; pos >= 0; pos --) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15); 
    }    // waits 15ms for the servo to reach the position
}