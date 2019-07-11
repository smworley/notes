# Python 3 notes
By Sarah Worley ([@smworley](https://github.com/smworley) on github).  

Python is an interpreted object oriented scripting language. It can serve a wide variety of roles due to the number of packages and modules available for it.

It is not a strongly typed language, which means that variables do not have to stay the data type that they are. You could use a variable 'i' as an integer and then later use it as a string, so a certain level of discipline is required to keep python code from turning into nonsensical spaghetti.

**Editors:**   
I highly recommend [Anaconda's Spyder](https://www.spyder-ide.org/) editor to those who are getting started but still want more features than the IDE from the Python site. It also comes with conda and the conda terminal, which makes it very easy to download and install new packages. Personally use [JetBrain's PyCharm](https://www.jetbrains.com/pycharm/) (community ver.) to write and debug python, but for some applications it's overkill.

- **Note:** As of late, PyCharm 2019.1.2 doesn't play well with Anaconda. This has been confirmed as a problem on JetBrain's side but be warned, none of your library imports will work right if your installation of PyCharm isn't 100% exactly what they use. The team hopes to have this fixed in 2019.1.3 but I don't know when that's coming out. View issue thread [here](https://youtrack.jetbrains.com/issue/PY-35141).  

**Purpose of this document:**  
These are personal notes but also for anyone who may find them of use. It will get you started and can serve as a cheat sheet for those who are already familiar with the language. I specifically write this in response to a lot of pedantic tutorials. There's a lot of examples here because that's the best way to learn code. If you're reading this to learn, I recommend testing the examples yourself breaking them. There's no better way to find out how something ticks than to break it.

## Table of Contents:
- [Printing](#printing)
- [Conditionals](#conditionals)
- [Looping](#looping)
- [File I/O](#fileio)
- [Functions](#functions)
- [Libraries](#libraries)
- [Custom Libraries](#libraries)
- [Credits](#credits)


## Printing[](#printing)

**Printing with float and integer values:**  
This example uses % replacement. %d specifies that the field needs to be replaced with an integer, the %5.2f specifies that the field needs to be replaced with a floating point value of precision 2. Note that this will not round it, it will just truncate values after.
```python
print("Groceries to buy: %2d, square feet of room cleaned: %.2f" %(15, 05.333))  
#OUTPUT: Groceries to buy: 15, square feet of room cleaned: 5.33
```

**Printing with the .format() method:**  

```python
print('Do you love {}? No! We love {}!'.format('pancakes', 'French toast'))
# OUTPUT: Do you love pancakes? No! We love French toast!
```
Something to note here is that this is not just restricted to printing. This works in any instance where a string has a wildcard.

## Conditionals[](#conditionals)

Lets say you're checking the weather. You don't take an umbrella when it's sunny, but **if** it's going to rain later, **then** you would very much like to have your umbrella.

In code, that process would look like this.

**If statments:**
```Python
weather = "rainy"

if weather == "rainy":
	print("You should take an umbrella!")
elif weather == "sunny":
	print("An umbrella might be nice for shade.")
else:
	print("Don't worry about taking an umbrella.")
```

What this says is that if the condition that weather is rainy is TRUE, then it will tell you to bring your umbrella. If it's not, it will check the elif statement to see if it is sunny. If that also fails, it will move to the 'umbrella' statement of sorts and execute the else. If you want to check multiple possibilities before hitting the else clause, add more elifs after your first else.

Python groups each if with the thing beneath it, so if you have two if statements above one another, **it will check both.** Run this in your IDE to see what I'm talking about.

```Python
weather = "rainy"

if weather == "rainy":
	print("You should take an umbrella!")
if weather == "sunny":
	print("An umbrella might be nice for shade.")
else:
	print("Don't worry about taking an umbrella.")

# output:
# You should take an umbrella!
# Don't worry about taking an umbrella.
```
It will execute the first if statement, then enter the second if set, and since it fails the second if set, it will default to the else statement. Keep this in mind if you are trying to chain ifs and it's not working right.  

**Note: Python does not natively support switch statements.**

## Looping[](#looping)  

When doing tasks that are repeatable (or even different), looping is the answer. It makes your code smaller, more readable, and when combined with conditional statements, more robust.

```Python
print("Hello World")
print("Hello World")
print("Hello World")
print("Hello World")
print("Hello World")
```

That works but, it's ugly. And if you had a longer process that you wanted to do, then this would really be a pain to write and read.

In this case, I know I want to print it five times.

**For loops:**  
```python
for i in range(0,5):
	print("Hello World")
```

Ah, that's better. Easier to read, and if I want to change that print statement I don't have to modify five of them.

For loops are for situations where you _know_ how many times you want the loop to execute _before_ you start it.

But lets say I didn't know how many times I wanted to say hello to the world.

**While loops:**
```Python
world = 0
while (world != 5):
	print("Hello World")
	world += 1
```
This is a loop that executes unless ```world``` is equal to five. Once it does, the statement in the loop is false and the loop breaks.

But what if I want to iterate through a list and my bracket key is broken? Never fear. There is also iterators.

**Iterators:**  
```Python
lyric = ["Hello", "My", "Honey", "Hello", "My", "Baby", "Hello", "My", "Ragtime", "Gal"]
for word in lyric:
	print(word, end ="")
```
This loop is kind of special, because it hinges on the number of elements in lyrics list. We can add as many more words to the lyric as we want without touching a single other piece of code. That's the power of iterators, however it comes at a cost. Once an iterator is used, it cannot be reused, and an iterator negates the ability to go backwards. We can only move forwards in this list, where as indexing with a while or a for loop would allow us to use negative indexes.



## File I/O[](#fileio)  

**Opening files with the open() function:**  

```python
filename = open(r"C:\cats\list-of-good-cats.csv","w+")
...
filename.close()
```

open() takes two arguments in this example, the filename and the mode. You may have noticed the 'r' I put beside the file path, this is a note to the interpreter to read the slashes as the character, not an escape sequence. With file paths containing slashes this is very important. If the file is in the same directory as the script, you don't need a full path.  

| mode | Function                        |  
|--------|---------------------------------|  
| r      | read only                       |  
| w      | write only (overwrite)          |  
| a      | append only (add to bottom)     |  
| +      | create file if it doesn't exist |  

This particular piece of code also requires the connection to the file to be manually closed. If you're only reading a file this isn't as critical. If you are writing or appending a file, **you must close the connection or it will not save what you wrote.**

So check that you've got a .close() at the end of your code before you throw your mouse at the wall (sorry mouse).

**Opening files through the with statement:**  

```python
with open(r"C:\cats\list-of-good-cats.csv") as filename:
	...
```
When a file is opened with the 'with' statement, filename is assigned that file descriptor for the duration of the indention. Once you unindent, the file is automatically closed. This is considered the preferred method of file opening for python purists because of its idiomatic nature, but it doesn't solve every problem.  

## Functions[](#functions)
It can be very useful to define reusable code to improve readability and maintainability.

**Arguments and return values:**  
```python
def find_string(string, A, B):
    '''
        FUNCTION DEFINITION: find_string(STRING, CHAR OR STRING, CHAR OR STRING) returns STRING.
        DESC.: Uses a string as a delimiter as finds the string between A and B.
        A and B should be different strings, otherwise just split the string on that delimiter
        and pull it from the list.
    '''

    if string.find(A) < 0:
        return None
    else:
        begin = string.find(A) + len(A)
        end = string.find(B)
        result = string[begin:end]
        return result

print(find_string("mary -loves& cats", "-", "&"))
# output:
# loves
```
In this example I have put a comment above that explains the types that this function will accept and return, since we know python is not strongly typed. This function takes a string, such as ```mary -loves& cats``` and ```-``` as A and ```&``` as B, and would return the string ```loves``` since something does exist between that delimiter. If there wasn't anything, it would return ```none```.

Also note it's generally bad practice to have a variable name be a single character. But, I have a permit.

**No arguments or return values:**  
```python
def permit():
	print("PERMIT:\n\tI do what I want.")

permit()

# output:
# PERMIT:
#		I do what I want.
```
My permit function doesn't take any arguments, and doesn't return any values.

**Optional arguments:**  
Functions can also have optional arguments. To have an optional argument, give the parameter a default value.

For example, if we wanted our delimiter string to default to a dash character.

```python
def find_string(string, a , b = '-'):
	....
```
In this definition, if we don't supply a third argument to ```find_string()``` when we call it, b will default to ```-```. This can be very useful in cases where a function has a lot of options, but the programmer may not always want to set them.  

## Libraries[](#libraries)
The biggest advantage of python is not having to write all the code. As a very high level language, you can get a lot done in a few lines. This comes at the expense of speed, but we're not here to talk grievances.  
If you're looking to do something particular in python, I recommend googling it first and seeing what comes up. For example, if you want to get the time. We could invoke the ```time.time()``` function to get it in seconds, but its a method in the ```time``` library.

```Python
import time
print(time.time())
```

This code imports the time library and prints the current time.

You can also do several imports on one line.

```python
import os, time, csv, sys
```

Or you can import modules with a different name.

```python
import pandas as import pd

data = pd.dataframe()
```
This code imports a module and renames it, you can see here that when we invoke something from that module we have to use its name.

## Custom Libraries[](#libraries)

1. Create a folder in the directory above the one that the python script is in.

2. Include an empty file in your package directory just called ```__init__.py```

  The file hierarchy should look like this:
  - targetscript.py
  - /packages/
    - library.py  

3. Create your functions in a python file.

4. In the target python file, import your library like this:
```Python
from package1 import primary as prime      # import one module from package1
# import our library
prime.myfunction(200)  
# reference a function in the primary library
```

## Debugging [](#debugging)  

**Timing how long something took:**  
I find it very useful to know how long something took to run. Sometimes it takes too quick and the function will say that it took 0 seconds. The example below uses a sleep function so that the output shows that it took two seconds for the bottom of the code to execute.  
```Python
import time
start = time.time()
# Code that you want to put a timer on here!
time.sleep(2)
#sleep for two seconds lil computer!
end = time.time()
print("I took %d seconds!" %(end-start))
```

## Numpy notes
Numpy is a very important python library for doing fast data analysis, however it's method of indexing can be difficult to understand at first. Hence, I wrote a small guide here on the matter. For clarity, examples in this section are written on the console.

### Creating arrays

To understand indexing, lets first create a 2d array with numpy.
```python
import numpy as np
# declare the array and fill it with our values.
x = np.array([[0,1,2,3],[4,5,6,7],[8,9,10,11],[12,13,14,15]])
# return shape (dimensions) of our array
x.shape
```
So, we just made an array that looks like this:

| 0 | 1 | 2 | 3 |
|--|--|--|--|
| 4 | 5 | 6 | 7 |
| 8 | 9 | 10 | 11 |
| 12 | 13 | 14 | 15 |

Indexes run from 0 to N-1 on both axis with N being the number of rows or columns.

### Indexing and slicing in numpy
**Commas** between coordinates indicates which dimension.  

`[rows, columns, ... optional axis]`

On the column and rows, the index of these begins at 0. So `[0,0]` would be equal to the value 0 here.
With that in mind, here's some examples of single dimension indexing on a numpy array.
```Python
>> x[0,1]			# return 2nd element in first row. (y,x)
>> 1
>> x[1,0]			# return 1st element in second row. (y,x)
>> 4
```
Just like you would find a coordinate on a standard Cartesian plane. But we don't just want cells, we want rows and columns sometimes.

**Colons** between numbers indicates ranges within that dimension.   

`[start:stop:step]`  

Keep in mind that the default arguments for the colons is `0:N:1`, where N is the total number of elements in that dimension. Note that is N, not N-1.  
AND NOTE, when indexing, if you do [1:4] it will return rows 1,2, and 3. It will not return row 4, even if row 4 exists. This also applies to columns.

```Python
>> # Get the first row
>> x[:1]
>> array([[0, 1, 2, 3]])
>> # Get the first column
>> x[:,:1]
>>
array([[ 0],
       [ 4],
       [ 8],
       [12]])
```

If we want to get a 2 dimensional slice of something, use both arguments for the colons.
```Python
>> # Get a slice containing of the first 4 rows and first 2 columns.
>> x[:4,:2]
>>
array([[ 0,  1],
       [ 4,  5],
       [ 8,  9],
       [12, 13]])
>> # Get a slice containing the first 3 rows last two columns
>> x[:3,2:]
>>
array([[ 2,  3],
       [ 6,  7],
       [10, 11]])
```
To change the ordering, utilize the step argument for the colons.
```Python
>> # Transpose on the X axis
>> x[::-1,]
>>
array([[12, 13, 14, 15],
       [ 8,  9, 10, 11],
       [ 4,  5,  6,  7],
       [ 0,  1,  2,  3]])
>> # Transpose on the Y axis.
>> x[:,::-1]
>>
array([[ 3,  2,  1,  0],
			[ 7,  6,  5,  4],
			[11, 10,  9,  8],
			[15, 14, 13, 12]])
>> # get odd numbered columns.
>> x[:,::2]
>>
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10],
       [12, 14]])
```
**Indexing with Boolean operators:**  
`nparray Boolean operation`  - Return an array with true or false values for where the Boolean evaluates true or false. The returned array will be the entire length of the evaluated array.  
`nparray[nparray boolean operation]` - Return an array containing the values which evaluate true. The returned array will only be as long as the number of elements that pass.  

```Python
>> # Return Boolean evaluations.
>> x > 4
>>
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True],
       [ True,  True,  True,  True]])
>> # Return values where the Boolean was true.
>> x[x > 4]
>> array([ 5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15])
```
**Arithmetic operations on entire ndarrays:**  
`nparray1 (operator) nparray2` - Both ndarrays must be the same dimensions, the operation will be performed as if the two were stacked on top of one another.  
`nparray arithmetic expression` - Perform the arithmetic expression to every element in the array.  
```Python
>> y = np.array([[10,10,10,10],[20,20,20,20],[30,30,30,30],[40,40,40,40]])
>> # Add x and y together
>> x + y
>>
array([[10, 11, 12, 13],
       [24, 25, 26, 27],
       [38, 39, 40, 41],
       [52, 53, 54, 55]])
>> (5*x)/20
>>
array([[0.  , 0.25, 0.5 , 0.75],
       [1.  , 1.25, 1.5 , 1.75],
       [2.  , 2.25, 2.5 , 2.75],
       [3.  , 3.25, 3.5 , 3.75]])
```

## Credits[](#credits)
The tutorials I used to learn python are credited below, but since I learned it in school a lot of this comes from personal knowledge. Credits are in no particular order.  

- [Geeksforgeeks: python formatted printing](https://www.geeksforgeeks.org/python-output-formatting/)  
- [Stackoverflow: How to create custom python libraries, arcseldon's answer specifically](https://stackoverflow.com/questions/15746675/how-to-write-a-python-module-package)  
- [Guru99: Python File I/O](https://www.guru99.com/reading-and-writing-files-in-python.html)
- [Learn NUMPY in 5 minutes - BEST Python Library! by Python Programmer on YouTube](https://youtu.be/xECXZ3tyONo)
- [Numpy Array Slicing - Tutorial on Array Slicing Numpy](https://youtu.be/7YvKhcs7sb0)

