# Sudoku-Solver
## This will solve a solveable sudoku board

    sudoku = [
        [0, 0, 0, 7, 9, 0, 0, 5, 0],
        [3, 5, 2, 0, 0, 8, 0, 4, 0],
        [0, 0, 0, 0, 0, 0, 0, 8, 0],
        [0, 1, 0, 0, 7, 0, 0, 0, 4],
        [6, 0, 0, 3, 0, 0, 0, 0, 8],
        [9, 0, 0, 0, 8, 1, 0, 1, 0],
        [0, 2, 0, 0, 0, 0, 0, 0, 0],
        [0, 4, 0, 5, 0, 0, 8, 9, 1],
        [0, 8, 0, 0, 3, 7, 0, 0, 0]
    ]

### We write a function to print our board in the standard 9x9 grid with lines dividing the board into 9 3x3 squares.
    def print_board(board):
        num = -1
        for i in range(len(board)):
            num += 1
            if num % 3 == 0 and num != 0:
                print("- - - - - - - - - - - -")
            num2 = -1
            for j in range(len(board[i])):
                num2 += 1
                if num2 % 3 == 0 and num2 != 0:
                    print(" | ", end="")

                if j == 8:
                    print(board[i][j])
                else:
                    print(str(board[i][j]) + " ", end="")

### This iterates through the board to identify indexes that are 0 (blank spaces in a normal sudoku board), and returns their grid identification.
    def empty_spot(board):
        for i in range(len(board)):
            for j in range(len(board[i])):
                if board[i][j] == 0:
                    return i, j
        return None

### This function, *is_okay*, identifies if any characters in the corresponding row, column, or 3x3 matrix is the number we are inputting.
#### This section states that if the character is in the corresponding row, but not the index of the number we are changing, then we are okay.
    def is_okay(board, num, position):
        for i in range(len(board[0])):
            if board[position[0]][i] == num and position[1] != i:
                return False
            else:
                return True
#### This section does the same process as above, but for the corresponding column.
        for i in range(len(board)):
            if board[i][position[1]] == num and position[0] != i:
                return False
            else:
                return True
#### This section checks to see if any number is the same in the sector, denoted by the 3x3 grid.
        square_x = (position[1] // 3) * 3
        square_y = (position[0] // 3) * 3

        for i in range(square_y, square_y + 3):
            for j in range(square_x, square_x + 3):
                if board[i][j] == num and (i, j) != position:
                    return False
                else:
                    return True

### Here we implement a recursive backtracking function, *solve*. We check to see if the numbers in the range 1-9 are able to be placed. If it is, then we place it and recursively repeat the solve function and go to the next empty index. If not, then we set the index back to 0 and backtrack to the previous index we altered and try a new number.
    def solve(board):
        if not empty_spot(board):
            return True
        else:
            (x, y) = empty_spot(board)

        for i in range(1, 10):
            if is_okay(board, i, (x, y)):
                board[x][y] = i
                
                if solve(board):
                    return True
                    
                board[x][y] = 0
        return False


    solve(sudoku)
    print_board(sudoku)
