+++
title = "procedural dungeon generator"
date = "2020-09-03"
description = "making a universal file converter with python with built in file compression"
tags = ["python", "learning", "beginner"]
+++

[[learning programming in python through chatgpt]]
### abstract
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
        print("png file")
    elif file_path.endswith(".csv"):
        print("csv file")
    else:
        print("unsupported file type.")
```

the keywords `if` `elif` and `else` are used here to execute code based on desired conditions.
using `filepath.endswith()` allows the system to check what the selected file extension is.
##### project aims
- create a grid for the dungeon map
- randomly assign rooms within the grid and make sure elements such as doors connect
- add encounters and treasure
##### key python concepts
- grids and lists
- randomisation, using the `random` module to generate room layouts and contents
- loops and conditionals, using loops you will look through the grid to place rooms and check connections
### step 1: creating the grid
##### theory
in python, grids can be represented with lists of lists. each sub-list is a row, and each part of that list is a cell within the row. here is an example.
```python
grid = [
  [0, 0, 0],  # first row with 3 columns
  [0, 0, 0],  # second row
  [0, 0, 0]   # third row
]
```
***
to initialise a a grid dynamically you can use loops, more specifically in this case the `for` loop. the `for` loop, in simple terms, is a loop type used to scan through and iterable object (something you can loop over, such as a list because it has multiple components within) and perform the desired action to each component within the object.
say we have the line "`for item in sequence`", the keyword `for` tells python were beginning a loop, `item` is a temp variable that takes the value from each different element in the `sequence` one at a time. lets see this in a more concrete example.
```python
for i in range(5):
    print(i)
```
***
in the above example, `range(5)` generates a sequence of numbers from 0 - 4 (this does not include 5). the loop runs 5 times, as specified by the range, each time with `i` extracting values from `range(5)`, after which the `print` function prints the value that `i` obtained in each iteration.
##### implementation
for our program, were going to use something called a nested loop, all this means is that there is a loop contained within a loop. this can be done as so.
```python
rows, cols = 3, 3
grid = []

for i in range(rows):  # creates each row
    row = []
    for j in range(cols):  # creates each column in row
        row.append(0)  # initialise with 0 
    grid.append(row)  # add completed row to grid

print(grid)
```
***
- as defined by the variables, the outer loop `rows` runs 3 times, and same with the inner loop `cols`
- `row.append(0)` allows us to append the value `0` to each element in the current row
- `grid.append(row)` allows us to add the row to the grid

while this program does what we want it to do, it can become a hassle to write this out every time we want a new grid, to change the grid size, or any other parameters.
to fix this lets use functions.
```python
def create_grid(rows, cols):
    grid = []
    for i in range(rows):
        row = []
        for j in range(cols):
            row.append(0) 
        grid.append(row)
    return grid

# call function and print grid
dungeon_grid = create_grid(3, 3)
print(dungeon_grid)
```
***
- here we defined `rows` and `cols` as parameters to the `create_grid` function, this allows us to change grid size dynamically through something like `create_grid(10,10)`
- `return grid` allows the grid to be returned once created so it can be used outside the function
### step 2: procedural room placement
##### theory
the `random` module in python allows you to generates random numbers or make random choices as seen below. 
```python
import random

random_number = random.randint(0, 9)  # generates random integer from 0-9
```
***
- `random.randint(x,y)`returns a random integer between x - y, inclusive.
##### implementation
the following code will implement the `random` module and make a new function to place rooms.
```python
import random

def create_grid(rows, cols):
    grid = []
    for i in range(rows):
        row = []
        for j in range(cols):
            row.append(0)
        grid.append(row)
    return grid

def place_rooms(grid, num_rooms):
    rows = len(grid)
    cols = len(grid[0])

    for _ in range(num_rooms):
        while True:  # keep trying till empty spot
            row = random.randint(0, rows - 1)
            col = random.randint(0, cols - 1)

            if grid[row][col] == 0:  # if spot is empty
                grid[row][col] = 1  # place room
                break
            
            elif grid[row][col] == 1:  # if spot is filled
                grid[row][col] = 2  # place special room

grid = create_grid(5, 5)
num_rooms = random.randint(5, 10)

place_rooms(grid, num_rooms)
for row in grid:
    print(row)
```
***
- in the above code we have added a `place_rooms()` function. this function takes the `grid` and `num_rooms` parameters as inputs
- `len(grid)` gets the number of rows in the grid, and then `len(grid[0])` gets number of columns (in python the `len()` function is used to return the number of items in an object or string)
- a while loop allows us to continue running a command until a condition is met. in our code this means it keeps running till `0` (an empty spot) is found to place a room
- if the command lands on a spot its already selected `1` it becomes a special room `2` using `elif` 
- if the spot is already occupied, the loop tries again with new random coordinates
### step 3: connecting rooms with corridors
##### theory
// key concepts in this step:
- coordinate systems > the grid's rows and columns act as coordinates, so we will be connecting coordinates on our grid this way.
- pathfinding > moving horizontally and vertically from one room to another forms corridors inbetween
