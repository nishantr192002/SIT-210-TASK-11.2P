from tkinter import *
import tkinter.font
import RPi.GPIO
import serial
import tkinter as tk


RPi.GPIO.setwarnings(False)
RPi.GPIO.setmode(RPi.GPIO.BCM)

if _name_ == '_main_':
    ser = serial.Serial('/dev/ttyACM0', 9600, timeout=1)  # Port and baud rate for the temperature sensor
    ser.flush()
current_value.set("Current Value: 0")
if ser.in_waiting>0:
    current= ser.readline().decode('utf-8').rstrip()
win = Tk()
win.title("Robot Control Panel")
myFont = tkinter.font.Font(family='Helvetica', size=12, weight='bold')
win.option_add('*TButton*Font', myFont)
win.option_add('*TButton*Foreground', 'blue')  # Change the title color
win.option_add('*TButton*Background', 'lightgray')  # Change the background color


def Forward():
    Front["text"]="PRESS STOP"
    ser.write(b"f\n")
def Backward():
    
    Back["text"]="PRESS STOP"
    ser.write(b"b\n")
def Rightward():
    Right["text"]="PRESS STOP"
    ser.write(b"r\n")
def Leftward():
    Left["text"]="PRESS STOP"
    ser.write(b"l\n")
def Conveyor():
    Conveyor_belt["text"] = "PRESS STOP"
    ser.write(b"c\n")
def Current():
    current_value.set(f"Current Value: {current}")

def Stop():
    #Stop["text"]="PRESS STOP"
    Front["text"]="Move Forward"
    Back["text"]="Move Backward"
    Right["text"]="Move Right"
    Left["text"]="Move Left"
    Conveyor_belt["text"] = "Turn on Conveyor belt"
    ser.write(b"s\n")
def close():
    RPi.GPIO.cleanup()
    win.destroy()

Front = Button(win, text='Move Forward', font=myFont, command=Forward, bg='white', height=2, width=24)
Front.grid(row=0, column=3, padx=10, pady=10)

Back = Button(win, text='Move Backward', font=myFont, command=Backward, bg='white', height=2, width=24)
Back.grid(row=2, column=3, padx=10, pady=10)

Right = Button(win, text='Move Right', font=myFont, command=Rightward, bg='white', height=2, width=24)
Right.grid(row=1, column=4, padx=10, pady=10)

Left = Button(win, text='Move Left', font=myFont, command=Leftward, bg='white', height=2, width=24)
Left.grid(row=1, column=2, padx=10, pady=10)

Stop = Button(win, text='STOP', font=myFont, command=Stop, bg='red', height=1, width=24)
Stop.grid(row=1, column=3, padx=10, pady=10)

Conveyor_belt = Button(win, text='Turn on Conveyor belt', font=myFont, command=Conveyor, bg='lightgreen', height=1, width=24)
Conveyor_belt.grid(row=3, column=3, padx=10, pady=10)

Ampere_data = Button(win, text='Current value', font=myFont, command=Current, bg='grey', height=1, width=18)
Ampere_data.grid(row=4, column=3, padx=10, pady=10)

label = tk.Label(win, textvariable=current_value)
label.grid(row=5, column=3, padx=10, pady=10)

ExistledButton = Button(win, text='EXIT', font=myFont, command=close, bg='red', height=1, width=6)
ExistledButton.grid(row=6, column=3, padx=10, pady=10)

win.configure(bg='lightblue')
win.mainloop()