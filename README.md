# Sudoku-Solver
## This will solve a solveable sudoku board

### define a solveable board.

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


### function to print our board
    def print_board(board):
        height=len(board)
        width=len(board[0])
        for i in board:
            print('----' * width)
            for j in i:
                print(f'| {j}', end = " ")
            print()
        print('----' * width)


#### This function states that if the character is in the corresponding row, column, or sector then we return false.
    def check_entries(board, x, y):
        height = len(board)
        for j in range(height):
            if board[y][x] == board[j][x] and j != y:
                return False
        for i in range(height):
            if board[y][i] == board[y][x] and i != x:
                return False
        new_x = (x // 3) * 3
        new_y = (y // 3) * 3
        for row in range(new_y, new_y + 3):
            for col in range(new_x, new_x + 3): 
                if board[row][col] == board[y][x] and y != row and x != col:
                    return False
        return True
        
### This function, *is_okay*, identifies if any characters in the corresponding row, column, or 3x3 matrix is the number we are inputting.
    def check_board(board):
        height = len(board)
        for j in range(height):
            for i in range(height):
                if check_entries(board,i,j) == False:
                    return False
        return True

### This iterates through the board to identify indexes that are 0 (blank spaces in a normal sudoku board), and returns their grid identification.
    def find_zero(board):
        height = len(board)
        for j in range(height):
            for i in range(height):
                if board[j][i] == 0:
                    return [j,i]
        return "fin"


### Here we implement a recursive backtracking function, *solve*. We check to see if the numbers in the range 1-9 are able to be placed. If it is, then we place it and recursively repeat the solve function and go to the next empty index. If not, then we set the index back to 0 and backtrack to the previous index we altered and try a new number.
    def solve(board):
        entries = range(1,10)
        height = len(board)
        next_zero = find_zero(board)
        if next_zero == "fin":
            return True
        else:
            [row, col] = next_zero
        for i in entries:
            board[row][col] = i
            if check_entries(board, col, row):
                solve(board)
            if check_board(board):
                return True
            board[row][col]=0

        return False

### print our boards before and after solving.

    print_board(board)
    solve(board)
    print_board(board)

