# This program utilises the cwiid Python library in order to get input over bluetooth from a wiimote.
# The following lines of code demonstrate many of the features realted to wiimotes, such as capturing button presses and rumbling the controller.
# I have managed to map the home button to the accelerometer - simply hold it and values will appear!

# Coded by The Raspberry Pi Guy. Work based on some of Matt Hawkins's!

import cwiid, time
import RPi.GPIO as GPIO
import time

#MOTOR ZOOI
GPIO.setmode(GPIO.BCM)

pinMotorAForwards = 10
pinMotorABackwards = 9
pinMotorBForwards = 8
pinMotorBBackwards = 7

Frequency = 40
DutyCycleA = 30
DutyCycleB = 30
Stop = 0

GPIO.setup(pinMotorAForwards, GPIO.OUT)
GPIO.setup(pinMotorABackwards, GPIO.OUT)
GPIO.setup(pinMotorBForwards, GPIO.OUT)
GPIO.setup(pinMotorBBackwards, GPIO.OUT)

pwmMotorAForwards = GPIO.PWM(pinMotorAForwards, Frequency)
pwmMotorABackwards = GPIO.PWM(pinMotorABackwards, Frequency)
pwmMotorBForwards = GPIO.PWM(pinMotorBForwards, Frequency)
pwmMotorBBackwards = GPIO.PWM(pinMotorBBackwards, Frequency)

pwmMotorAForwards.start(Stop)
pwmMotorABackwards.start(Stop)
pwmMotorBForwards.start(Stop)
pwmMotorBBackwards.start(Stop)

def StopMotors():
  pwmMotorAForwards.ChangeDutyCycle(Stop)
  pwmMotorABackwards.ChangeDutyCycle(Stop)
  pwmMotorBForwards.ChangeDutyCycle(Stop)
  pwmMotorBBackwards.ChangeDutyCycle(Stop)

def Forwards():
  pwmMotorAForwards.ChangeDutyCycle(DutyCycleA)
  pwmMotorABackwards.ChangeDutyCycle(Stop)

def Backwards():
  pwmMotorAForwards.ChangeDutyCycle(Stop)
  pwmMotorABackwards.ChangeDutyCycle(DutyCycleA)

def Left():
  pwmMotorBForwards.ChangeDutyCycle(DutyCycleB)
  pwmMotorBBackwards.ChangeDutyCycle(Stop)

def Right():
  pwmMotorBForwards.ChangeDutyCycle(Stop)
  pwmMotorBBackwards.ChangeDutyCycle(DutyCycleB)
  


button_delay = 0.1

print 'Please press buttons 1 + 2 on your Wiimote now ...'
time.sleep(1)

# This code attempts to connect to your Wiimote and if it fails the program quits
try:
  wii=cwiid.Wiimote()
except RuntimeError:
  print "Cannot connect to your Wiimote. Run again and make sure you are holding buttons 1 + 2!"
  quit()

print 'Wiimote connection established!\n'
print 'Go ahead and press some buttons\n'
print 'Press PLUS and MINUS together to disconnect and quit.\n'

time.sleep(3)

wii.rpt_mode = cwiid.RPT_BTN

while True:

  buttons = wii.state['buttons']

  # Detects whether + and - are held down and if they are it quits the program
  if (buttons - cwiid.BTN_PLUS - cwiid.BTN_MINUS == 0):
    print '\nClosing connection ...'
    # NOTE: This is how you RUMBLE the Wiimote
    wii.rumble = 1
    time.sleep(1)
    wii.rumble = 0
    exit(wii)

  # The following code detects whether any of the Wiimotes buttons have been pressed and then prints a statement to the screen!
  if (buttons & cwiid.BTN_LEFT):
    Backwards()
    time.sleep(button_delay)

  if(buttons & cwiid.BTN_RIGHT):
    Forwards()
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_UP):
    Left()
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_DOWN):
    Right()
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_1):
    print 'Button 1 pressed
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_2):
    print 'Button 2 pressed'
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_A):
    print 'Button A pressed'
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_B):
    StopMotors()
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_HOME):
    wii.rpt_mode = cwiid.RPT_BTN | cwiid.RPT_ACC
    check = 0
    while check == 0:
      print(wii.state['acc'])
      time.sleep(0.01)
      check = (buttons & cwiid.BTN_HOME)
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_MINUS):
    DutyCycleA = DutyCycleA - 5
    if DutyCycleA < 10:
      DutyCycleA = DutyCycleA + 5
    time.sleep(button_delay)

  if (buttons & cwiid.BTN_PLUS):
    DutyCycleA = DutyCycleA + 5
    if DutyCycleA > 100:
      DutyCycleA = DutyCycleA - 10
    time.sleep(button_delay)
