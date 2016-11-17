# SmartCharger
A Charge which will Automatically Off the power supply when Smartphone battery is full
hear is the code it works on Raspberrypi and and other developers boards
it is written in python

import sched, time

import subprocess

import time

import RPi.GPIO as GPIO

import os

GPIO.setwarnings(False)

GPIO.setmode(GPIO.BCM)

GPIO.setup(17,GPIO.OUT)

GPIO.output(17,False)







s = sched.scheduler(time.time, time.sleep)

def do_something(sc): 

    print ("Doing stuff...")

    os.system("adb shell cat /sys/class/power_supply/battery/capacity > curr_batt.txt") 

    from itertools import islice

    with open("full_batt.txt",'r') as myfile:

        head = list(islice(myfile, 3))

    from itertools import islice

    with open("curr_batt.txt",'r') as myfile:

        headh = list(islice(myfile, 3))

    

    if headh != head:

        os.system("adb shell cat /sys/class/power_supply/battery/capacity > curr_batt.txt")

    else:

       GPIO.output(17,True)

    sc.enter(1, 1, do_something, (sc,))



s.enter(1, 1, do_something, (s,))

s.run()

