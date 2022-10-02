# Python Homework 

In this assignment a text file consisting of coordinates and cardinal points (North, East, South and West) will be read using a Python program which will output a csv file.  When the program outputs a cvs file, the file will demonstrate ordered coordinates which will generate a maze of desired size. 

To create program a template was provided which was edited using Visual Studio with added Python Extension.  


# Input File  
 
 The input file below (inputfile.txt) contains cell positions given as coordiates and cardinal points (order E,W,N,S) which represent cell distances from walls surrounding maze. This corresponds to a 4X4 maze.
 
 
 ```
 "(3, 2)",0.22,1.2,0.14,1.8
"(4, 2)",1.1,1.2,1.3,0.12
"(1, 3)",0.7,0.8,0.21,1.1
"(2, 3)",1.2,0.15,1.3,0.16
"(3, 3)",1.2,0.12,0.11,1.4
"(4, 3)",0.2,1.2,1.5,0.17
"(1, 4)",0.1,1.01,0.05,0.09
"(1, 1)",0.1,0.15,0.23,1.2
"(3, 1)",1.1,0.2,1.0,0.1
"(2, 4)",0.2,1.2,0.1,1.2
"(3, 4)",0.18,1.1,1.2,1.4
"(4, 4)",0.17,0.15,1.4,0.1
"(4, 1)",1.2,0.2,0.21,0.19
"(1, 2)",0.8,0.2,0.14,1.2
"(2, 2)",0.2,1.1,1.3,0.21
"(2, 1)",0.9,0.21,1.3,1.2
"(3, 1)",1.1,0.2,1.0,0.1
"(2, 1)",0.9,0.21,1.3,1.2
 ```

The Python program will read the cell position i.e "(3,2)" locating object at (3,2) and reading the distances left to right.  In this position the program will determine that the distances from wall are 0.22m from the East, 1.2m from the West, 0.14m from North, and 1.8m  from South.  Cell positions input file are not organized in order. Input file can be updated depending on size of maze.  


# Pyamaze.py File


The file/code below is an import which is used in the Python program. Pyamaze creates the maze. 

https://github.com/MAN1986/pyamaze/blob/7486dca7c613ef3d4c874ed2a7def01a3e0a56ae/pyamaze/pyamaze.py


# Python Program (yaymaze.py)

The Python program below was created to read the input txt file and output a csv file.  

  1. Class YayMaze is used to classify and organize data.  
  2. self. is used to have access variables under class. In this code we have 9 
     instances.  These instances are useful for the data needed and formatting of 
     program. 
  3. def is used to define different functions.
  4. str is used to create values to a string.  
  5. for and else loops were used to iterate sequences in program.
  5. float was used to retun a floating point number from string.
  6. .format was implemented in program to format values which will be input in 
     a placeholder within the string which is identified by curly brackets 
     {}. 
  7.  if and else statements were using in program to execute actions based on c
     certain conditions.  
  8.  Maze size can be changed if integer in size= is changed to a different value. 
      A proper input file needs to be imported that complements maze size.  
  
  
  
  
  
```
from pyamaze import maze,COLOR, agent 
import shlex
import re

class YayMaze:
    def __init__(self, ifile, origPos=(1,1),size= 4):  
        self.ifile = ifile
        self.csvfile = 'mazerunning.csv'
        self.origPos = origPos
        self.size = size
        self.maze = maze(rows=self.size, cols=self.size)
        self.dict = {}
        self.read_and_process(ifile)
        self.fix_format(ifile)
        self.generate_csv()
        
        


    def read_and_process(self, ifile):
        file1 = open(self.ifile, 'r')
        Lines = file1.readlines()
        count = 0


        # Strips the newline character
        for line in Lines:
            count += 1
            output = self.fix_format(line.strip())
            wall = ''
            for indx in range(1,5):
                if float(output[indx]) > 0.25:
                    wall = wall + '1,'
                else:
                    wall = wall + '0,'
            
            self.dict[str(output[0]).replace('  ',' ')] = wall[0:-1]
            
   
    def fix_format(self,line):
        line = line.replace(",", ", ")
        output = shlex.split(line)
        output = [re.sub(r",$","",w) for w in output]
        return output

    def generate_csv(self):
        with open(self.csvfile,'w') as csv_file:
            csv_file.write("  cell  ,E,W,N,S")
            csv_file.write("\n")
            for i in range (1,self.size+1):
                for j in range (1,self.size+1):
                    strtoken = '({}, {})'.format(j,i)  
                    if strtoken in self.dict:
                        csv_file.write('"{}",{}'.format(strtoken,self.dict[strtoken]))
                        csv_file.write("\n")
                    else:
                        csv_file.write('"{}",{}'.format(strtoken,"0,0,0,0"))
                        csv_file.write("\n")
                   

    def createMaze(self):
        self.maze.CreateMaze(x=self.origPos[0],y=self.origPos[1],loopPercent=100, loadMaze=self.csvfile)
        self.maze.run()

if __name__=="__main__":
    a = YayMaze('inputfile.txt', (3,1), size = 4)
    a.createMaze()


```


# Output File (mazerunning.csv)

The following output file is generated in format "cell coordinates (i,j), distances from wall (E,W,N,S)".   

The program determines that if a distance is >0.25m a 0 will generate in its place, while if it is <0.25m the value will be replaced by 1. A 0 represents open space while a 1 represents a wall.


```
 cell  ,E,W,N,S
"(1, 1)",0,0,0,1
"(2, 1)",1,0,1,1
"(3, 1)",1,0,1,0
"(4, 1)",1,0,0,0
"(1, 2)",1,0,0,1
"(2, 2)",0,1,1,0
"(3, 2)",0,1,0,1
"(4, 2)",1,1,1,0
"(1, 3)",1,1,0,1
"(2, 3)",1,0,1,0
"(3, 3)",1,0,0,1
"(4, 3)",0,1,1,0
"(1, 4)",0,1,0,0
"(2, 4)",0,1,0,1
"(3, 4)",0,1,1,1
"(4, 4)",0,0,1,0

```

# Generated Maze 

4x4 Maze generated using Pyamaze package and output file generated using Python program writted for this assignment.  

![Pymaze_Maze](https://user-images.githubusercontent.com/93830830/193432790-b8a6b32b-696c-46bb-81f0-449eee7cc7f8.png)
