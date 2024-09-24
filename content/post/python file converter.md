+++
title = "universal file converter + compressor with python"
date = "2020-05-11"
description = "making a universal file converter with python with built in file compression"
tags = ["python", "files", "beginner"]
+++

### intro
i thought of this while converting a bunch of `.MTS` files from my sony handicam (i absolutely adore 2000s digicam technology) to `.MP4` for easier file sharing since not everything supports `.MTS` files to be embedded and played natively, such as discord. while usually I simply import all the videos into premiere pro or kdenlive, depending on the os i'm using, and put all the clips together from whatever was recorded an export, effectively converting from a bunch of `.MTS` clips to a single full `.MP4` video. however the problem was raised when i simply wanted to record a guitar riff i had been messing around with for my band and my phone microphone just could not pick up the sound from the amp (my phone is an old hand-me-down from my father who works in construction so everything is broken or clogged with bits of cement and other shit). it ended up being a 15 minute task to upload the video to a converter site, download it again, realise discord only allows 8mb video upload without nitro, go find a compressor site and repeat the process then finally i could upload the video.

while i am well aware there are about a million python scripted file converters and compressors i could easily download from github, i have wanted to dip my toes into python as i begin my journey of coding and other such stuff so i thought i may aswell make it myself. as i'm writing this i think i would like to also make this a basis for my own personal suite of file management and conversion to fit my workflow so for ease of use and redistribution i will make this a gui converter with room to expand and add many other file manipulation features.
### body
with lack of really any python coding experience i have one of two options, read documentation on the basics or learn as i go. have a wild guess at what i always choose. this post will be a small mix of instructions and just my processes used so sorry if it reads a bit weird i only really make these posts to keep myself motivated and get down my thoughts.
##### basic python syntax
before getting started its best you (or more fittingly i) know the basics of python, assuming you know python already you can skip this part, its more just note taking for me to learn python.

// variables:
in python, variables are used to store data. you can assign these values to these variables using the `=` operator as seen below.
```python
name = "converter"
version = 1.0
```

in this example, the variable `name` is assigned the string `converter` as its value, and the variable `version` has been assigned a floating point number (whole number containing decimal point) of `1.0`

// functions:
functions let you turn logic into a shorthand function and reuse it through out your project without the hassle of re-writing the code.
this is easily done through defining the function, naming it then writing the desired function
an example of this is shown beneath.
```python
def greet():
    print("welcome to a3cc-file-manipulator")

greet()  # calls the function and prints the message
```

the `def` keyword unlocks new problem solving by enabling us to think of solutions with reduced code mess. this can be seen with this simple addition calculator.
```python
def add(a, b):
    return a + b

result = add(3, 5)
print(result)  # Outputs: 8
```

// conditionals:
in python `if` statements are used to check conditions such as if a certain file type is selected.
```python
def check_file_type(file_path):
    if file_path.endswith(".png"):
        print("It's a PNG file.")
    elif file_path.endswith(".csv"):
        print("It's a CSV file.")
    else:
        print("Unsupported file type.")
```

the keywords `if` `elif` and `else` are used here to execute code based on desired conditions.
using `filepath.endswith()` allows the system to check what the selected file extension is.
##### file handling with python
python makes it quite easy to access and read or write to a file, you could use something simple like this chunk of code to open a file.
``` python
with open("filename.txt", "r") as file:
    content = file.read()
    print(content)
```
***
syntax explanation:
- `open("filename.txt", "r")` opens the selected file in read mode (hence the `r` statement), this can be changed to `w` for write, `a` for append and so on
- `with` is a crucial statement to allowing the file to be properly closed once used
##### file paths and directories
through the use of libraries (collections of code simplified to shorthand to make code neater and just easier) you can quickly and effectively interact with file paths. Using the libraries `os` and `pathlib` you can make make a small script for opening and saving files with our converter.
``` python
import os
from pathlib import Path

# get current directory
current_directory = os.getcwd()
print(f"Current Directory: {current_directory}")

# combine paths safely
file_path = Path(current_directory) / "folder" / "file.txt"
print(f"full file path: {file_path}")
```
***
syntax explanation:
- `os.getcwd()` returns what the current working directory is (where the commands are being executed)
- `Path(current_directory) / "folder" / "file.txt"` selects the name of the directories and files to combine them to a single complete path
##### using tkinter for file dialogue
`tkinter` is a standard python package for the tcl/tk gui toolkit. using the module `tkinter.filedialog` you enable the user to select a file from their system. using `filetypes=` you are able to specify what files the user can upload. the below code will display how you can configure `filedialog` to change the file specifications.
``` python
from tkinter import filedialog

# open file dialog to select file
file_path = filedialog.askopenfilename(filetypes=[("img files", "*.png *.jpg"), ("CSV files", "*.csv")])
print(f"selected file: {file_path}")
```

##### finally beginning the project
to begin we will construct a simple gui window to build upon.
### conclusion