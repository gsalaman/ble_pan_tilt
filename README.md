# ble_pan_tilt
Goal:  produce a BLE controlled pan-tilt application, based on  ble_stepper 

# Hardware
* One ESP32
* Two Big-easy boards, one for pan, one for tilt

Big-easy connection summary:
* MS1, MS2, and MS3 can be shared between both boards...will likely stick with 16th steps for all.
* Separate pins for Step
* Can either share or do dedicated enables.  Going to start with sharing.
* Shared Dir...see phase 1 assumption below.
* Power:  will start by seeing if one 12v wart has enough juice to drive both boards.   Can go to two if necessary.

# BLE interface update
Inherited from ble_stepper:
* Set step size (1,2,4,8,16)
* Set ms between steps
* Set dir (0 or 1)
* Set number of steps
* Motion:
  * Go
  * Stop, release motor
  * Stop, keep motor engaged

For phase 1, I'm going to make an assumption that only one stepper will be moving at a time (either pan or tilt).

I'll preserve the interface for step-size and ms between steps, but for the pan-tilt, I'll probably keep them at default values.  They'll need to apply to both big-easys.

I'll expand my "go" command to be either "pan" or "tilt"...and it'll start that stepper moving.   The general movement flow:
* set the desired direction
* set the number of steps
* send a "pan" or "tilt"

# Application interface
Note that the tilt stepper has a 5:1 gear, but the pan does not.  This means each microstep for Pan is 0.1125 degrees (360/200/16...or 6.75 arc-minutes), but the tilt direction is 0.0225 degrees or 1.35 arc-minutes.  I'm gonna want a "fine control" and "rough control"...maybe 4 buttons (up down left right) times two (one for a small jump,  one for "go") and then  one in the middle for "stop".  For my first iteration, I'll deal with the fact that pan is faster than tilt.  Future iterations may try and normalize this...as well as give me "move one degree" control, but the math doesn't easilly work out. 

In addition to this "control" screen, I'll preserve the  "connect" screen from the stepper app. 
Future mod will be to add a "config" screen that lets me tweak with step size and ms between steps...but I won't need that right away.
