# Typing-Speed-Test
Typing Speed Test GUI using Python 
INT213 Mini Project
ON
“Typing Speed Calculator”




Submitted to
“Cherry Khosla”

Submitted by
S. No.	Name	Roll No.	Registration No.
1	Aadi Padamwar	RK21GPA22	12105265
2	Himanshu Sharma	RK21GPB46	12107151
3	Anjali Singh	RK21GPB51	12112761

Index

1.	Introduction
2.	Project Objectives
3.	Modules Used in Project
4.	Codes and Results
5.	Conclusion
6.	References
7.	GitHub Link


 
   Introduction
The program focuses on providing a GUI to the user for testing his/her WPM (words per minute) speed or typing speed on their local system without any internet connection.
Words per minute, commonly abbreviated wpm is a measure of words processed in a minute, often used as a measurement of the speed of typing, reading or Morse code sending and receiving. Higher the wpm, higher the typing speed.
The program also demonstrates the use of Python in-build GUI making library, Tkinter. The whole user interface of this program is made with the help of Tkinter. 


Project Objective
The main objective of this project is to create a program with Tkinter (GUI) which can check the typing speed of the user and display his/her wpm (words per minute) on the GUI interface.
Another objective of this project is to learn and demonstrate the use of GUI in the python using the Tkinter. Tkinter is a very easy to use and beginner-friendly GUI library that can be used to design interactive interfaces. Tkinter provides various controls, such as buttons, labels and text boxes used in a GUI application.       




Modules Used in Project

tkinter 
Tkinter is a Python binding to the Tk GUI toolkit. It is the standard Python interface to the Tk GUI toolkit and is Python's de facto standard GUI. Tkinter is included with standard Linux, Microsoft Windows and macOS installs of Python. The name Tkinter comes from Tk interface.

ctypes
ctypes is a foreign function library for Python. It provides C compatible data types and allows calling functions in DLLs or shared libraries. It can be used to wrap these libraries in pure Python.

random
Random module is an in-built module of Python which is used to generate random numbers. These are pseudo-random numbers means these are not truly random. This module can be used to perform random actions such as generating random numbers, print random a value for a list or string, etc.









Codes and Results 
Codes
from tkinter import *
import ctypes
import random
import tkinter
 
# For a sharper window
ctypes.windll.shcore.SetProcessDpiAwareness(1)

# Setup
root = Tk()
root.title('Type Speed Test')

# Setting the starting window dimensions
root.geometry('700x700')

# Setting the Font for all Labels and Buttons
root.option_add("*Label.Font", "consolas 30")
root.option_add("*Button.Font", "consolas 30")
root.configure(background="powder blue")

# functions
def keyPress(event=None):
    try:
        if event.char.lower() == labelRight.cget('text')[0].lower():
            # Deleting one from the right side.
            labelRight.configure(text=labelRight.cget('text')[1:])
            # Deleting one from the right side.
            labelLeft.configure(text=labelLeft.cget('text') + event.char.lower())
            #set the next Letter Lavbel
            currentLetterLabel.configure(text=labelRight.cget('text')[0])
    except tkinter.TclError:
        pass

def resetWritingLabels():
    # Text List
    possibleTexts = [
        'For writers, a random sentence can help them get their creative juices flowing. Since the topic of the sentence is completely unknown, it forces the writer to be creative when the sentence appears. There are a number of different ways a writer can use the random sentence for creativity. The most common way to use the sentence is to begin a story. Another option is to include it somewhere in the story. A much more difficult challenge is to use it to end a story. In any of these cases, it forces the writer to think creatively since they have no idea what sentence will appear from the tool.',
        'The goal of Python Code is to provide Python tutorials, recipes, problem fixes and articles to beginner and intermediate Python programmers, as well as sharing knowledge to the world. Python Code aims for making everyone in the world be able to learn how to code for free. Python is a high-level, interpreted, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Python is dynamically-typed and garbage-collected. It supports multiple programming paradigms, including structured (particularly procedural), object-oriented and functional programming. It is often described as a "batteries included" language due to its comprehensive standard library.',
        'As always, we start with the imports. Because we make the UI with tkinter, we need to import it. We also import the font module from tkinter to change the fonts on our elements later. We continue by getting the partial function from functools, it is a genius function that excepts another function as a first argument and some args and kwargs and it will return a reference to this function with those arguments. This is especially useful when we want to insert one of our functions to a command argument of a button or a key binding.',
    ]
    # Chosing one of the texts randomly with the choice function
    text = random.choice(possibleTexts).lower()
    # defining where the text is split
    splitPoint = 0
    # This is where the text is that is already written
    global labelLeft
    labelLeft = Label(root, text=text[0:splitPoint], fg='grey')
    labelLeft.place(relx=0.5, rely=0.5, anchor=E)

    # Here is the text which will be written
    global labelRight
    labelRight = Label(root, text=text[splitPoint:])
    labelRight.place(relx=0.5, rely=0.5, anchor=W)

    # This label shows the user which letter he now has to press
    global currentLetterLabel
    currentLetterLabel = Label(root, text=text[splitPoint], fg='grey')
    currentLetterLabel.place(relx=0.5, rely=0.6, anchor=N)

    # this label shows the user how much time has gone by
    global timeleftLabel
    timeleftLabel = Label(root, text=f'0 Seconds', fg='grey')
    timeleftLabel.place(relx=0.5, rely=0.4, anchor=S)

    global writeAble
    writeAble = True
    root.bind('<Key>', keyPress)

    global passedSeconds
    passedSeconds = 0

    # Binding callbacks to functions after a certain amount of time.
    root.after(60000, stopTest)
    root.after(1000, addSecond)

def stopTest():
    global writeAble
    writeAble = False
    
    # Calculating the amount of words
    amountWords = len(labelLeft.cget('text').split(' '))
    
    # Destroy all unwanted widgets.
    timeleftLabel.destroy()
    currentLetterLabel.destroy()
    labelRight.destroy()
    labelLeft.destroy()

    # Display the test results with a formatted string
    global ResultLabel
    ResultLabel = Label(root, text=f'Words per Minute: {amountWords}', fg='black')
    ResultLabel.place(relx=0.5, rely=0.4, anchor=CENTER)

    # Display a button to restart the game
    global ResultButton
    #ResultButton = Button(root, text = f'Retry',bd=8,font=('arial',16,'bold'),width=14,fg="red",bg="powder blue", command=restart).grid(row=0,column=0)
    #ResultButton = Button(root, text=f'Retry', fg = 'red', command=restart, anchor=CENTER).grid(row = 0 , column = 0)
    ResultButton = Button(root, text=f'Retry', command=restart, fg= 'red')
    ResultButton.place(relx=0.5, rely=0.6, anchor=CENTER)

def restart():
    # Destry result widgets
    ResultLabel.destroy()
    ResultButton.destroy()

    # re-setup writing labels.
    resetWritingLabels()

def addSecond():
    # Add a second to the counter.

    global passedSeconds
    passedSeconds += 1
    timeleftLabel.configure(text=f'{passedSeconds} Seconds')

    # call this function again after one second if the time is not over.
    if writeAble:
        root.after(1000, addSecond)

# This will start the Test
resetWritingLabels()

# Start the mainloop
root.mainloop()








Result
 
 

Conclusion 
The purpose of this project is to calculate the typing speed of the user by providing a Graphical interface to them using Tkinter.
Tkinter provides various controls, such as buttons, labels and text boxes used in a GUI application. These controls are commonly called widgets. It is an element of Graphical User Interface (GUI) that displays or illustrates information and gives a way for the user to interact with the OS. 






References 
This project is made possible by taking help from various sources generally websites and YouTube videos along with knowledgeable friends who helped us in the making of the project some of the sources which are useful in this project are :-
YouTube
•	https://www.youtube.com/watch?v=quBb--IJPPc 
•	https://www.youtube.com/watch?v=LgkmgleybeI 
•	https://www.youtube.com/watch?v=YXPyB4XeYLA 
•	https://www.youtube.com/watch?v=p_LUzwylf-Y 

Website
•	https://docs.python.org/3/library/tkinter.html 
•	https://www.geeksforgeeks.org/how-to-test-typing-speed-using-python/ 
•	https://geekyhumans.com/test-your-typing-speed-using-python/ 


GITHUB LINK 



