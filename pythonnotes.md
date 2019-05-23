# Python 3 notes
By Sarah Worley ([@smworley](https://github.com/smworley) on github :sparkling_heart: ).  

Python is an interpreted object oriented scripting language. It can serve a wide variety of roles due to the number of packages and modules available for it. These notes only apply to Python 3 versions.

**Editors:**   
I highly recommend [Anaconda's Spyder](https://www.spyder-ide.org/) editor to those who are getting started but still want more features than the IDE from the Python site. Personally use [JetBrain's PyCharm](https://www.jetbrains.com/pycharm/) (community ver.) to write and debug python, but for some applications it's overkill.

**Purpose of this document:**  
These are personal notes but also for anyone who may find them of use. It will get you started and can serve as a cheat sheet for those who are already familiar with the language. I specifically write this in response to a lot of pedantic tutorials. There's a lot of examples here because that's the best way to learn code. If you're reading this to learn, I recommend testing the examples yourself breaking them. There's no better way to find out how something ticks than to break it. :smiling_imp:

## Table of Contents:  
- [Printing](#printing)
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



## Custom Libraries[](#libraries)

1. Create a folder in the directory above the one that the python script is in.

2. Include an empty file in your package directory just called ```__init__.py```

  The file hierarchy should look like this:
  - targetscript.py
  - packages/
    - library.py  

3. Create your functions in a python file.

4. In the target python file, import your library like this:
```Python
from package import primary as prime
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



## Credits[](#credits)
The tutorials I used to learn python are credited below, but since I learned it in school a lot of this comes from personal knowledge. Credits are in no particular order.  

- [Geeksforgeeks: python formatted printing](https://www.geeksforgeeks.org/python-output-formatting/)  
- [Stackoverflow: How to create custom python libraries, arcseldon's answer specifically](https://stackoverflow.com/questions/15746675/how-to-write-a-python-module-package)
