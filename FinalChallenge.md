# **Final Challenge Feature Documentation**

This **markdown document** will explain the steps necessary to install the required libraries to run the feature's python script, a detailed explanation of the funtions of the code, the reasoning for the theme I selected for the project and my intention on how it would interact with the audience during the End Of Semester showcase.

# Syntax
1. ## Theme
    1. Theme choice
    2. Choice of Feature
    3. Intention of use
2. ## Research
    1. Benefits of Interactive Features
3. ## Installation Steps
    1. Libraries Installation
    2. Pre-Installation check
    3. Code Instructions
    4. Connection diagram
    5. Hardware specifiactions

# Theme

## **Theme choice**
The theme I've selected for the project is my **Classmates**. The reason why I've chosen my peers as my project's theme is because I thought that this could be a good way to make the audience familiarize themselves with the people who set up the many fun and interactive features for our show case. In a way, it serves as a reminder near and dear to me about the people I spent my polytechnic life with even when I am no longer in Nanyang Polytechnic anymore

![](images/class.jpg)
[class photo of the first polarized image sent onto the board after the board was assembled]

## **Choice of Feature**

The reason I picked a game type of feature is because as I would further explain later in the Research segment promotes,

-  greater retention of attention span
-  greater retention of information
-  higher levels  of engagement
- motivation through gamification


## **Intention of use**

My hangman game feature is meant to be played one person at a time, with each game lasting about a minute, my other intention of this feature is that I would like the audience to remember the names of my classmates and I through this fun game that promotes greater retention of information, and motivation through gamification of the process.

An example of this would be like for example, when a parent plays red light green light with the child so she/he would intuitively learn to stop at red lights and walk at greens at crossing lights in the future.

# Research

## **Benefits of Interactive Features**
the benefit of using an interactive game like activity such as the hangman.py on kids of a young age between 7-12, is that according to this [article](https://trainingindustry.com/articles/content-development/6-benefits-of-interactivity-in-corporate-training/) written by Lance Noland.
He wrote that and I quote 

***"Studies show that a higher level of engagement during training activities results in greater retention and recall of knowledge on the part of the learner. And interactivity strategies such as the use of multimedia elements, real-world scenarios, and even basic achievement levels and badges can help to transform the most mundane training modules into engaging, thought-provoking and memorable learning experiences."***

Not only does interactive activities increase the retention of information and attention span in children but it also promotes a higher level of engagement compared to passively viewing an activities as it immerse the kid in the activity which could never be achieved in a traditional non-interactive activity. This has been proven time and time again like for example how kids would rather perform science experiments than just reading about the concept on a text book.

Interactive activities also have positive impacts on young adults with the first example being **Motivation through gamification** as explained by Mr Snehnath Neendoor
in this [article](https://kitaboo.com/7-benefits-of-interactive-corporate-training/), ***"Gamification motivates people to perform better. Introducing gaming elements"***
such as who could guess the name with the least amount of tries which creates a sort of leaderboard, creating a competitive enviroment making the experience much more memoriable for them as they are immersed in the activity




# Installation Steps

## **Libraries Installation**

All the libraries used inside the hangman.py script are already inbuilt and thus dont need any external downloads. The in-built libraries used are listed below

- tkinter
    * used to make the Graphic User Interface(GUI)
    * used to display the hangman images on the GUI as well as the letter and other function buttons such as 'new game' which resets the game
- string
    * used to save and return variables as string format
    * specifically The ascii() function in the library returns a readable version of any object
- random
    * its a random variable generator used in the code to randomly draw different names from my name list in the hangman game


## **Pre-Installation check**

Before downloading any of the libraries before please ensure that you have the latest version of your pi operating system as well as pip3 installed by going into the terminal and typing

>sudo apt-get update

and
>sudo apt-get upgrade

next check that your pi has python3 installed by typing in the terminal

>pip3 --version

![](images/pip3.png)

if its not installed, type into the terminal

>sudo apt-get install python3-pip

if there are no additional files added, then proceed as normal however if there are new files added, please restart you pi so that any new packages can be added on after the restart.






### **MQTT**
First we have the install both the host and client MQTT library to communicate both ways between the host pi and the student pi. This link [here](https://github.com/huats-club/mqttstarterkit/blob/main/README.md) brings you to a guide on setting up the mqtt server written by ywfumav. 

Next download the Client publish python file also written by ywfumav from [here](https://github.com/huats-club/mqttstarterkit/blob/main/client_pub_template.py#L7) and rename it to student.pub.py and follow the instructions that are indented in the code.

## **Code Instructions**

### **Hangman.py**
Next I will show the code for the hangman feature along explanations on how some of the functions work to output.

```python
from tkinter import *
from tkinter import messagebox
from string import ascii_uppercase
import random



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


### **Functions** 

***newGame***
```python
def newGame():
    global the_word_withSpaces
    global numberOfGuesses
    numberOfGuesses =0
    
    the_word=random.choice(word_list)
    the_word_withSpaces = " ".join(the_word)
    lblWord.set(' '.join("_"*len(the_word)))
```
When this function is called it resets the numberOfGuesses integer back to 0 from whatever number it was, uses the random fuction to pull a different name from the 'word_list' and set it as 'the_word' which is then set as the next words on the GUI

***guess***
```python
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
```
This function is multiple if else statement with a nested for loop inside which basically displays a letter on the gui every time a letter is guess correctly, else the number of guesses would increase by 1 each time the letters does not match and also displays 1 out of the 12 images of the hangman, which also increase by 1 in numerical order until the 'numberOfGuesses' global int hits 11 in which case the messagebox is triggered.

**Tkinter Window**

```python
window = Tk()
window.title('Hangman-GUESS classmates NAME')

window.mainloop()
```
this is to create the main tkinter window and loop it for the game's gui 

output:

![](images/gui.PNG)

*this is the default GUI that appears when the code is run*

When you get a correct answer a text window will pop up saying:
![](images/correct.PNG)

While if you fail to get the correct name after 11 guesses the game will end and a different text window will pop up saying:
![](images/wrong.PNG)

## **System diagram**

### **Hardware**

```mermaid
graph TD
A[Laptop] --> B[VNC Viewer]
B --> A
A --> C
B --> D
A --> C[MQTT]
C --> D[Raspberry Pi]
D --> E[Main Server]
E --> F[ESP32]
F --> G[Servo Motors]
```
Credit: ywfumav [link](https://github.com/huats-club/EGL314starterkit) 


## **Hardware specifiactions**

### **Hardware Used** ###
RaspberryPi 4 Model B Version: Raspbian GNU Linux 10 Buster

![](images/Pi4.jpg)

[source](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)








 