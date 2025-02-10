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
- This structure is not only used in most heuristics, but is the structure that is used to apply and save those heuristics (i will talk about specific heuristics in the end)
  
An array of Bitsets called rowAvailability, An array of Bitsets called colAvailability,
An array of Bitsets called gridAvailability, each holding the possibilities that can be inserted (or were yet to be inserted) in their respected row, column or grid.
- Theese structures are used in the hidden groups heuristics, which will be mentioned in the end.

And now.. for the diamonds of my program. my application form to why i am better then a 20$ ai:
3 2d arrays of bitsets called rowAvailabilityCounter, colAvailabilityCounter and gridAvailabilityCounter
where each row index represents a row, column or grid respectively, and each column index represents every possible value in the sudoku, and the values stored inside the array are the columns, row or positions respectively where they appear in the row, column or grid respectively.
**Explanation** i will take rowAvailabilityCounter for an example, the row index in the 2d array will represent the same row index in the sudoku, while the column index in the 2d array will represent a value from the values that are possible in the sudoku (e.g for a 9X9 its 1-9). for example, if i will take rowAvailabilityCounter[3, 2] it will give me a BitSet that represents the columns where the value 2(or 3 in reality because index starts at 0) in row 3 appears as a possibility.
or if i will take rowAvailabilityCounter[3,2] it will give me a BitSet that represents the rows where the value 2(or 3 in reality because index starts at 0) in column 3 appears as a possibility.
- Theese structures are used in every single line of code, and if they arent, its a mistake. they are used in all heuristics, except for naked singles and minimum possibility insertions. and even more importantly they hint when to check for a possible heuristics.

and lastly a stack of Guesses that hold the insertions and possibility removal between guesses.
- this allows the program to return to before the latest guess.



There are some more less important structures but theese were the main ones.

**Heuristics:**
1. **Minimum possibility insertion**: Every time the program doesnt know for sure where to change the current state, and it has to guess an insertion, we put the insertion in the place with the least possible possibilities. this is done by using the frequency array(squarePossibilitesCounter) that holds the amount of possibilities corresponding to the index in the array, and inserting a value that is possible in the first cell we find.
2. **Naked singles**: every cell that has only one possible value, can only have this value. thus its garunteed to be there. this Heuristic is similar to the first one, but i added it because its very fast to implement and it minimizes recursion. every time you remove a possibility from a cell (from squarePossibilities) you can just count the amount of possibilities left and if its one its a naked single and you insert it.
3. **Pointing groups**: if in grid, a value can only be inserted in one row or one column, it cannot be inserted in other grids in the same row or column. this check happens only when value appears less time in a grid then the grid width or hieght (the amount of possible positions in rowAvailabilityCounter[grid, value] is less the grid width or hieght). if a pointing group is found, you remove the value as from the possibilities in the cells that appear in the same row or column in other grids.
4. **Hidden Singles**: if a value appears once as a possibility in a row, col or grind. it must be there and it needs to be inserted. every time a possibility is removed from a cell, if the amount of possibilities for the value in the same row(rowAvailabilityCounter[row, value].Count), col(colAvailabilityCounter[col, value].Count) or grid(gridAvailabilityCounter[grid ,value].Count) is one, its a hidden single and it must be inserted.
5. **Hidden Groups**: if X possible values have X cells they appear in and they appear only in those cells in the same row, column or grid, those X values must be in one of those X cells, and any other value has to be removed from the cells. for example: if 2 possible value(lets say 5 and 7) appears in 2 cells and only those cells in a row: 5 and 7 must be in those two cells, and any other possible values in those cells have to be removed. Every time we remove a possibility from a cell, we can count the amount of possibilities it has in the row(rowAvailabilityCounter[row, value].Count), col(colAvailabilityCounter[col, value].Count) or grid(gridAvailabilityCounter[grid ,value].Count) and if there are X values (where X is the amount of time the possibility appears in the row, column or grid) that appear only where the removed possibility appear(ther are subsets of the position), they are hidden groups and numbers that arent in the group can be removed from the cell.

# In conclusion
1/10 not recommended, this made me addicted to caffeine. <3 Omega ΩΩΩ
