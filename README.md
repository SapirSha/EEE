# About the project
This is a sudoku solver, that solves the following sudokus:
1X1, 4X4, 9X9, 16X16, 25X25,
6X6, 8X8, 10X10, 12X12, 14X14, 15X15, 16X16,
18X18, 20X20, 22X22, 24X24.

The sudoku gives **all** the possible answers to the sudoku (depending on choice).

**Disclaimer** while the program itself can diffrentiate between rectangle sudokus with horizontal and vertical grids and can solve both, since they cant be diffrentiated in the input, only horizontal( which is the most common ) is used

# Input
You can input the sudokus from the console, or by giving the console a path to a text file with the sudoku.

In case of file input the sudoku determines if a line is a sudoku(see [Determining the input](#determining-the-input))

Exiting the program can be done by using (ctrl + c) or (ctrl + z) or the input sapir.


# Sudoku input format
A sudoku is represented by a string, where 0 represents an empty space and every asci symbol above the asci value of 0, represents the value that is the distance from the value of asci 0. (e.g: 1 is 1, 9 is 9, : is 10...).

# Determining the input
The program determines wether the input is a file path or a sudoku with the following critiria:

A string is considered a sudoku if all the asci values that are in the string, are in range of values that are allowed in the sudoku +- Diviation(which is 10).
or in other words, between 0 - diviation to square of length + diviation.

A string is considered a file path if it ends with .txt

otherwise it gives invalid input.

# Output
----------

# requirements
net8.0
MSTest 3.6.1

# How the algorithem works
The sudoku revolves around backtracking and uses hidden groups, pointing groups, and naked singles as heuristics.

