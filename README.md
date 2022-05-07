#otp generator
import math
import os
import random
digits="0123456789"
OTP=""
for i in range(6):
    OTP+=digits[math.floor(random.random()*10)]
msg='Your OTP Verification for app is '+OTP+' Note..  Please enter otp within 2 minutes and 3 attempts, otherwise it becomes invalid'
file2=open("otp.txt","w")
file2.write(OTP)
file2.close()
os.system('python second.py')



#verification screen
from tkinter import *
from tkinter import messagebox
import tkinter
import time
import  os
Window=Tk()
Window.geometry("1280x720")
Window.title("Verification Screen")
count = 3  #global variable for count calculation. Initially there are 3 attempts. So I set as 3
def verify():
    global count
    global Window
    end=time.time()          # timers ends when the user clicks verfiy
    t = format(end - start)  # calculate the difference between end and start timer
    print(float(t))          #  print the time in seconds
    if float(t) >= 60:      # Check it the user enters above 1 minutes. So i set as >=60
        messagebox.showinfo("Time out", "Session Expired ...Time out Please regenerate OTP")
        Window.destroy()
    else:
        cmd1=str(a.get())             # Get the entered OTP
        cmd='python verify.py '+cmd1  
        os.system(cmd)                # call the verify program
        ok='Invalid OTP: '+str((count-1))+' attempts remaining' 
        count=count-1
        f1=open("status.txt","r")
        bh=f1.read()

        if count>=1 and bh != "success":

            tkinter.messagebox.askretrycancel("Error", ok)
            f1.close()
        elif count == 0 and bh != "success":
            f=open("otp.txt","w")
            f.write("")
            f.close()
            messagebox.showinfo("Oooo","Your 3 attempts was over. Please regenerate OTP")
            f1.close()
            Window.destroy()
        elif bh == "success":
            f1.close()
            Window.destroy()

start=time.time() # Timer started once the screen is entered
label1=Label(Window,text="Verification Screen",relief="solid",font=("arial",20,"bold"),fg='blue').pack(fill=BOTH)
a=StringVar()
Re=Label(Window,text="Enter your Otp",font=("arial",15,"bold")).place(x=0,y=50)
w1=Entry(Window,width=20,textvariable=a)
w1.place(x=400,y=50)
Re1=Label(Window,text="Please enter within 1 minute",font=("times new roman",15,"bold")).place(x=550,y=50)
ver = Button(Window, text="Verify",relief="raised", bg='blue', font=("arial", 15, "bold"), fg='white',command=verify).place(x=100,y=150)
Window.mainloop()


#verification process
import sys
from tkinter import messagebox

from tkinter import *
import time
b=sys.argv[1]
f1=open("otp.txt","r")
b1=f1.read()
f1.close()
if b==b1:
    f = open("status.txt", "w")
    f.write("success")
    f.close()
    Window = Tk()
    Window.geometry("800x600")
    Window.title("Success")
    messagebox.showinfo("Congratulations", "Your OTP was verified Successfully!!")
    Window.destroy()
    Window.mainloop()
else:
    f = open("status.txt", "w")
    f.write("failure")
    f.close()
