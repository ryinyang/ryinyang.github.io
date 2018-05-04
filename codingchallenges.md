# Coding Challenges
Here are a bunch of different coding problems that I've tried out, it includes
my thought processes and different attempts at solving them! 

## [1. Pow(x, n)](https://leetcode.com/problems/powx-n/description/)

### Description
Implement a mathematical power function such that Pow(x, n) = x ^ n.

### Naive Solution
```
class Solution:
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        ans = 1
        if n < 0:
        	n *= -1
        	x = 1 / x
        for i in range(n):
        	ans *= x
        return ans
```
While this solution does work, it does not run very efficiently. Observe that 
this solution has to multiply by x at every loop for n loops. Thus we get a
time complexity of O(n)

### Better Solution
```
class Solution:
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        ret = 1
        if n < 0:
            n *= -1
            x = 1 / x
        while n > 0:
            if n % 2 != 0:
                ret *= x
            x *= x
            n = int(n/2)
        return ret
```
This solution is more efficient! We can see that at each iteration, n is halved,
meaning that it runs at O(log(n)).

### Takeaway
I initially didn't think that O(n) would be slow for this problem. In my
experience (especially in school), we learn that slow algorithms are ones that
have O(n^2) or O(n!). In general, we learn that algorithms that run in linear
time are actually quite fast, especially when we are doing anything graph
related. However it turned out this linear solution was, in fact, too slow, 
especially when we are trying to find the 1000000th power of something. 
I learned that just because something is linear, doesn't mean that it's 
necessarily the fastest solution.

## [2. N Queens II (Status: Unfinished)](https://leetcode.com/problems/n-queens-ii/description/)

### Description
Find the number of solutions to the N-Queens problem. 
The N-Queens problem: given an NxN chess board and N queens, find a 
configuration of queens such that there are no two queens that attack
each other.

### First Solution
```
class Solution:
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        def print_board(board):
            for row in board:
                print(row)
            print('\n')
            
        def check(n, board):
            # print('Input:')
            # print_board(board)
            board = deepcopy(board)
            
            # Check for solution & end
            num_queens = 0
            num_open = 0
            for row in board:
                for item in row:
                    if item == 1 or item == 2:
                        num_queens += 1
                    if item == 0:
                        num_open += 1
            if num_queens == n:
                # print('solution found')
                return 1
            if num_open == 0:
                # print('solution not found')
                return 0
            
            # Block off spaces attacked by new queen
            for row in range(n):
                for col in range(n):
                    if board[row][col] == 2:
                        for r in range(n):
                            board[r][col] = -1
                        for c in range(n):
                            board[row][c] = -1

                        # Diagonals
                        i = 0
                        j = 0
                        while row + i < n and col + j < n:
                            if board[row+i][col+j] == 0:
                                board[row+i][col+j] = -1
                            i += 1
                            j += 1
                        i = 0
                        j = 0
                        while row + i >= 0 and col + j >= 0:
                            if board[row+i][col+j] == 0:
                                board[row+i][col+j] = -1
                            i += -1
                            j += -1
                        i = 0
                        j = 0
                        while row + i < n and col + j >= 0:
                            if board[row+i][col+j] == 0:
                                board[row+i][col+j] = -1
                            i += 1
                            j += -1
                        i = 0
                        j = 0
                        while row + i >= 0 and col + j < n:
                            if board[row+i][col+j] == 0:
                                board[row+i][col+j] = -1
                            i += -1
                            j += 1
                        board[row][col] = 1
                        break
            
            # print('After Blocking:')
            # print_board(board)
            
            # Recurse
            num_solutions = 0
            for col in range(n):
                for row in range(n):
                    if board[col][row] == 0:
                        board[col][row] = 2
                        # print_board(board)
                        num_solutions += check(n, board)
                        board[col][row] = -2
            
            return num_solutions
        
        board = [[0 for i in range(n)] for j in range(n)]
        return check(n, board)
```
This first solution I came up with involved using recursive calls to search
through the possible game states.

First I checked if a solution has been found ie if there are 4 queens on the
board. If a solution had been found, I'd return a 1, indicating the number of
solutions found. Then I'd check for a dead end, meaning that there were no more
spaces to check. 

Then mark the spaces that are being attacked by queens. These spaces won't be
further searched.

Then I recurse through the open spaces, effectively searching deeper down the
game state tree.

Finally, I summed up all the solutions that had been found and output that 
number.

I had 5 states for each cell on the board:
* -2 means that the cell is open but has been checked before
* -1 means that the cell is blocked (or being attacked) by an existing queen
* 0 means that the cell is empty and unblocked
* 1 means that the cell is occupied by a queen
* 2 means that the cell is newly occupied by a queen (makes it faster to block
new cells)

Currently, this solution exceeds the time limit allowed, meaning there is a
faster solution. 