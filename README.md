# Archeology-of-the-Present
# Sand Box and projection mapping

// Archeology of the Present; San Box, is a sensitive surface structure with photo resistors coupled in six groups of three.
// The code calls out video projections to be played on top of the sand. Tanner Settles as creator, with help from Nathan Villicana-Shaw and Bibiana Bauer.

import processing.serial.*;
import cc.arduino.*;
import processing.video.*;

Arduino arduino;
Movie [] mov = new Movie[6];

// These constants won't change. They're used to give names to the pins used:
final int analogInPin1 = 5;// lower left frame//5
final int analogInPin2 = 2;// upper middle frame_my pv top left//2
final int analogInPin3 = 1;// upper right frame//1
final int analogInPin4 = 4;// lower left frame//3
final int analogInPin5 = 3;// lower middle frame//4
final int analogInPin6 = 0;// lower right frame//0

// top left is first number 
int thresh[] = {550, 550, 550, 
  550, 550, 550}; // Each photo resistor has a threshold to trigger the video playback. 
// When the resistors are completely isolated in darkeness the readings are around 950, and 0 in complete light.

int sensorValue1 = 0; // the individual name of the photo resistor sets
int sensorValue2 = 0;
int sensorValue3 = 0;
int sensorValue4 = 0;
int sensorValue5 = 0;
int sensorValue6 = 0;

void setup() {
  // initialize serial communications at 9600 bps:
  size(1920, 1080);
  // Prints out the available serial ports.
  println(Arduino.list());
  arduino = new Arduino(this, Arduino.list()[4], 57600);
  println("initialized arduino");
  // The video files used for projection, each file belonging to a sensorValue
  mov[0] = new Movie(this, "0.mp4");//14 sec
  mov[1] = new Movie(this, "1.mp4");//23 sec
  mov[2] = new Movie(this, "2.mp4");//14 sec
  mov[3] = new Movie(this, "3.mp4");//16 sec
  mov[4] = new Movie(this, "4.mp4");//14 sec
  mov[5] = new Movie(this, "5.mp4");//14 sec
  mov[0].play();
  mov[0].jump(0);
  mov[0].pause();
  mov[2].play();
  mov[2].jump(0);
  mov[2].pause();
  print(mov[0].duration());
}

void draw() {
  // print the results of the individual photo resistor sets to the Serial Monitor:
  print("sensor values: A");
  print(sensorValue1, "B");
  print(sensorValue2, "C");
  print(sensorValue3, "D");
  print(sensorValue4, "E");
  print(sensorValue5, "F");
  println(sensorValue6);

  background(255);
  // read the analog in value:
  sensorValue1 = arduino.analogRead(analogInPin1);
  sensorValue2 = arduino.analogRead(analogInPin2);
  sensorValue3 = arduino.analogRead(analogInPin3);
  sensorValue4 = arduino.analogRead(analogInPin4);
  sensorValue5 = arduino.analogRead(analogInPin5);
  sensorValue6 = arduino.analogRead(analogInPin6);

  //  if the sensoValue1 is greater than the threshhold ("thresh[0]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 0
  if (sensorValue1 > thresh[0]) {
    // testing the sensorValue readings 
    // fill(255, 0, 0);
    // for map the first varaible is the number you want to remap to a new range
    // the next varaible (50) is the lowest value you expect from sensorValue1
    // the next variable (900) is the largest value you expect
    // the next variable (0), is what you want to remap the "low" to
    // the last variable (255) is what you want to remap the "high" to
    //sensorValue1 = constrain(sensorValue1, 50, 800);
    //fill(map(sensorValue1, 50, 800, 0, 255), 0, 0);
    //fill(255, 50, 50);
    // rect(0, 0, width/3, height/2);
    //reveal unit 1
    mov[0].read();
    float currentLocation = mov[0].time();
    if (currentLocation > mov[0].duration() - 0.35) {
      mov[0].jump(0);
    }
    mov[0].play();
    // scale(0.25);
    image(mov[0], 0, 0);
    // scale(4.0);
    // reveal video playback in lower left frame
  } else {
    mov[0].jump(0);
    mov[0].pause();
    // hide unit 1 by painting a white rectangle over while restarting video file 0 to the beginning
    noStroke();
    fill(255);
    rect(2*(width/3), 0, width/3, height/2); //
  }

  //  if the sensoValue2 is greater than the threshhold ("thresh[1]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 4
  if (sensorValue2 > thresh[1]) {

    mov[4].read();
    float currentLocation = mov[4].time();
    if (currentLocation > mov[4].duration() - 0.23) {
      mov[4].jump(0);
    }
    mov[4].play();
    //scale(0.25);
    image(mov[4], (width/3), 0);
    //scale(4.0);
    // reveal video playback in upper middle frame
  } else {
    mov[4].jump(0);
    mov[4].pause();
    // hide unit 2 by painting a white rectangle over while restarting video file 4 to the beginning
    noStroke();
    fill(255);
    rect(2*(width/3), 0, width/3, height/2);
  }


  //  if the sensoValue4 is greater than the threshhold ("thresh[3]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 3
  if (sensorValue4 > thresh[3]) {
    mov[3].read();
    float currentLocation = mov[3].time();
    if (currentLocation > mov[3].duration() - 0.23) {
      mov[3].jump(0);
    }
    mov[3].play();
    //scale(0.25);
    image(mov[3], 0, height/2); 
    // reveal video playback in lower left frame
  } else {
    mov[3].jump(0);
    mov[3].pause();
    // hide unit 4 by painting a white rectangle over while restarting video file 3 to the beginning
    noStroke();
    fill(255);
    rect(2*(width/3), 0, width/3, height/2);
  }

  //  if the sensoValue5 is greater than the threshhold ("thresh[4]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 1
  if (sensorValue5 > thresh[4]) {
    mov[1].read();
    float currentLocation = mov[1].time();
    if (currentLocation > mov[1].duration() - 0.23) {
      mov[1].jump(0);
    }
    mov[1].play();

    image(mov[1], width/3, height/2); 
    // reveal video playback in lower middle frame
  } else {
    mov[1].jump(0);
    mov[1].pause();
    // hide unit 5 by painting a white rectangle over while restarting video file 1 to the beginning
    noStroke();
    fill(255);
    rect(2*width/3, 0, width/3, height/2);
  }

  //  if the sensoValue6 is greater than the threshhold ("thresh[5]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 5
  if (sensorValue6 > thresh[5]) {
    mov[5].read();
    float currentLocation = mov[5].time();
    if (currentLocation > mov[5].duration() - 0.23) {
      mov[5].jump(0);
    }
    mov[5].play();

    image(mov[5], 2*width/3, height/2);  
    // reveal video playback in lower right frame
  } else {
    mov[5].jump(0);
    mov[5].pause();
    // hide unit 6 by painting a white rectangle over while restarting video file 5 to the beginning
    noStroke();
    fill(255);
    rect(2*width/3, 0, width/3, height/2);
  }

  //  if the sensoVvalue3 is greater than the threshhold ("thresh[2]") of [550] meaning exposer to light with a measuring greater than 550, will play the video file 2
  if (sensorValue3 > thresh[2]) {
    mov[2].read();
    float currentLocation = mov[2].time();
    if (currentLocation > mov[2].duration() - 0.23) {
      mov[2].jump(0);
      // println("entered loop");
    }
    mov[2].play();
    //scale(0.25);
    image(mov[2], 1200, 0);
    // reveal video playback in upper right frame
  } else {
    mov[2].jump(0);
    mov[2].pause();
    // hide unit 3 by painting a white rectangle over while restarting video file 2 to the beginning
    noStroke();
    fill(255);
    rect(2*(width/3), 0, width/3, height/2);
  }
}
