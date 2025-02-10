# Ω Sudoku Solver


# About the project
This is a sudoku solver, that solves the following sudokus:
1X1, 4X4, 9X9, 16X16, 25X25,
6X6, 8X8, 10X10, 12X12, 14X14, 15X15, 16X16,
18X18, 20X20, 22X22, 24X24.

The sudoku gives **all** the possible answers to the sudoku (depending on choice).

**Disclaimer:** while the program itself can differentiate between rectangle sudokus with horizontal and vertical grids and can solve both, since they cant be differentiated in the input, only horizontal( which is the most common ) is used.

# Input
You can input the sudokus from the console, or by giving the console a path to a text file with the sudoku.

In case of file input, the sudoku determines if a line is a sudoku(see "determining-the-input") and solves it, or when multiple lines are detected it will solve all of them.

Exiting the program can be done by using (ctrl + c), (ctrl + z) or the input sapir.


# Sudoku input format
A sudoku is represented by a string, where 0 represents an empty space and every asci symbol above the asci value of 0, represents the value that is the distance from the value of asci 0. (e.g: 1 is 1, 9 is 9, : is 10...).

# Determining the input
The program determines wether the input is a file path or a sudoku with the following critiria:

A string is considered a sudoku if all the asci values that are in the string, are in range of values that are allowed in the sudoku +- Deviation(which is 10).
or in other words, between 0 - Deviation to square of length of the input + Deviation.

A string is considered a file path if it ends with .txt

otherwise it gives invalid input.

# Output
The program outputs solved sudokus in the same format as the input(see "Sudoku input format").
in cases of files, the sudoku will be printed both in the file the sudoku appears and in the console.
the program will also output the time it took to get to the first solution.

# requirements
net8.0
MSTest 3.6.1

# How the algorithm works
The sudoku revolves around backtracking and uses hidden groups, pointing groups, and naked singles as heuristics.
it also uses a stack to return to previous positions in case of backtracking.

There are quite a lot of structures the algorithem uses and here are some:

**Notice:** Bitset is a data structure i created that uses an array of ints and their bits to store positive natural values (some sort of a set). this data structure will be used in some of the structures here.

2d Array of integers called sudoku which will hold the values that are inserted to the sudoku.

An array of hashsets called squarePossibilitesCounter that will hold the rows and columns of each cell (frequency array):
the index in the array where the position(row and column) appeares represents the number of possibilities that can be inserted in that cell. (e.g. if row 3 column 2 appeares in the hashmap in index 4: there are 4 possibilities for insertion in that cell).
- This structure is used when guessing, it allowes searching the cell with the least amount of possibilities in Θ(n) time.

2d array of Bitsets called squarePossibilities, where each cell in the array will hold the possibilities for the cell in the same position.
- This structure is in most of the heuristics, which will be mentioned in the end.
  
An array of Bitsets called rowAvailability, An array of Bitsets called colAvailability,
An array of Bitsets called gridAvailability, each holding the possibilities that can be inserted (or were yet to be inserted) in their respected row, column or grid.
- Theese structures are used in the hidden groups heuristics, which will be mentioned in the end.

A Bitset called full, that is mainly used to initialize the other structures with all thier possibilities.
- Allowes for faster initialization since it uses bit operations instead of inserting the values one by one.






# In conclusion
1/10 not recommended, this made me addicted to caffeine. <3 Omega ΩΩΩ
