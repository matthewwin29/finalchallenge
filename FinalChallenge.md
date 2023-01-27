# **Final Challenge Feature Documentation**

This **markdown document** will explain the steps necessary to install the required libraries to run the feature's python script, a detailed explanation of the funtions of the code, the reasoning for the theme I selected for the project and my intention on how it would interact with the audience during the End Of Semester showcase.

# Syntax
1. Theme
    1. a
    2. b
    3. c
2. Research
    1. a
    2. b
    3. c
3. Installation Steps
    1. a
    2. b
    3. c

# Theme

The theme I've selected for the project is my classmates. The reason why I've chosen my peers as my project's theme is because I thought that this could be a good way to make the audience familiarize themselves with the people who set up the many fun and interactive features for our show case.

## Intention of use

My hangman game feature is meant to be played one person at a time, and my intention of this feature is that I would like 

# Research





# Installation Steps

## **Libraries Installation**

Below are the libraries needed for the code as well as the steps to import them onto a RaspberryPi 4b running on the Respbian GNU Linux 10 Buster OS onto the polarizer board

## Pre-Installation check

Before downloading any of the libraries before please ensure that you have the latest version of your pi operating system by going into the terminal and typing

>sudo apt-get update

and
>sudo apt-get upgrade

if there are no additional files added, then proceed as normal however if there are new files added, please restart you pi so that any new packages can be added on.




### **MQTT**
First we have the install both the host and client MQTT library to communicate both ways between the host pi and the student pi. This link [here](https://github.com/huats-club/mqttstarterkit/blob/main/README.md) brings you to a guide on setting up the mqtt server written by ywfumav. 

Next download the Client publish python file also written by ywfumav from [here](https://github.com/huats-club/mqttstarterkit/blob/main/client_pub_template.py#L7) and rename it to student.pub.py and follow the instructions that are indented in the code.



### **Hangman.py**
Next I will show the code for the hangman feature along explanations on how some of the functions work to output.

```python
from tkinter import *
from tkinter import messagebox
from string import ascii_uppercase
import random
import student_pub


window = Tk()
window.title('Hangman-GUESS classmates NAME')
word_list= ['MINGJIN','NICHOLAS','MATTHEW','BRYAN','GALLEN','JERREL','JASON','WEIJIE','CHINRONG','DARYL','RAYMOND','TRICIA','RUIEN',
            'EILEEN','YUBING','CELINE','YONGWEI','MALCOM']
            
photos = [PhotoImage(file="images/hang0.png"), PhotoImage(file="images/hang1.png"), PhotoImage(file="images/hang2.png"),
PhotoImage(file="images/hang3.png"), PhotoImage(file="images/hang4.png"), PhotoImage(file="images/hang5.png"),
PhotoImage(file="images/hang6.png"), PhotoImage(file="images/hang7.png"), PhotoImage(file="images/hang8.png"),
PhotoImage(file="images/hang9.png"), PhotoImage(file="images/hang10.png"), PhotoImage(file="images/hang11.png")]


def newGame():
    global the_word_withSpaces
    global numberOfGuesses
    numberOfGuesses =0
    
    the_word=random.choice(word_list)
    the_word_withSpaces = " ".join(the_word)
    lblWord.set(' '.join("_"*len(the_word)))

def guess(letter):
	global numberOfGuesses
	if numberOfGuesses<11:	
		txt = list(the_word_withSpaces)
		guessed = list(lblWord.get())
		if the_word_withSpaces.count(letter)>0:
			for c in range(len(txt)):
				if txt[c]==letter:
					guessed[c]=letter
				lblWord.set("".join(guessed))
				if lblWord.get()==the_word_withSpaces:
					messagebox.showinfo("Hangman","You guessed it!")
		else:
			numberOfGuesses += 1
			imgLabel.config(image=photos[numberOfGuesses])
			if numberOfGuesses==11:
					messagebox.showwarning("Hangman","Game Over")



imgLabel=Label(window)
imgLabel.grid(row=0, column=0, columnspan=3, padx=10, pady=40)

			
			
# x =32; k = 0; outputValue= [0 for i in range(x)]
# for i in range(x):
# 	outputValue[i] = [0 for j in range(x)]
# pixvalue = list(imgLabel.getdata())

# for i in range(x):
# for j in range(x):
# outputValue[i][j] = pixvalue[k]
# k = k + 1
# print(outputValue)
  
lblWord = StringVar()
Label(window, textvariable  =lblWord,font=('consolas 24 bold')).grid(row=0, column=3 ,columnspan=6,padx=10)

n=0
for c in ascii_uppercase:
    Button(window, text=c, command=lambda c=c: guess(c), font=('Helvetica 18'), width=4).grid(row=1+n//9,column=n%9)
    n+=1

Button(window, text="New\nGame", command=lambda:newGame(), font=("Helvetica 10 bold")).grid(row=3, column=8)

newGame()
window.mainloop()

```











 