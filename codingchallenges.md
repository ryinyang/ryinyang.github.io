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

## [3. Happy Number](https://leetcode.com/problems/happy-number/description/)

### Description
Given a number n, return true if it is a Happy Number. A Happy Number found like
this: Take the digits of n and find the sum of squares. Continue until the sum
becomes 1. If n becomes 1, then it is a Happy Number, if the sum loops endlessly
in a cycle, it is not a Happy Number.

Example:
Input: 19
Output: true
Explanation: 
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1

### Solution

```
class Solution:
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        seen = set()
        
        while n != 1:
            seen.add(n)
            s = str(n)
            n = 0
            for digit in s:
                n += int(digit) ** 2
            
            if n in seen:
                return False
        
        return True
```
This solution is a simple implementation of the algorithm described in the 
problem. It converts n into a string to get the digits easily, then finds the 
sums. It keeps track of cycles by adding each seen number into a set data
structure. If we've seen a number before, we return false, otherwise, we return 
true.

### Takeaways
The only misstep I had was trying to split the newly converted sum with 
str.split(). I originally thought that this function would split the string
into an array of characters, but when given no parameters, split doesn't
know how to split the string, so it returns the whole string in an array.
To loop through the string, I simply needed to use a for loop.

## [4. Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning/description/)

### Description
Given a BST whose nodes only contain 1 or 0, return a tree where every subtree
not containing 1 is removed.

### Solution 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pruneTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
    
        def postOrder(curr):
            if curr is not None:
                left = postOrder(curr.left)
                right = postOrder(curr.right)
                
                # Remove subtree
                if left == 0:
                    curr.left = None
                if right == 0:
                    curr.right = None
                    
                # Return status
                if curr.val == 1:
                    return 1
                else:
                    if left == 1 or right == 1 or curr.val == 1:
                        return 1
                    else:
                        return 0
            return -1
        
        postOrder(root)
        return root
```
My solution does a post order traversal through the tree. When a subtree
without any 1's is detected, remove the subtree. When a leaf node is reached,
return -1, indicating that a leaf node has been reached. If a node is a 1,
return 1. If a node is a 0, but has a subtree with 1, return 1, otherwise,
return 0. When a node detects that one of the children has no 1's, remove.

### Takeaways
The biggest thing I learned from this problem is how to apply a post order
traversal. I think the traversals are one of those algorithms that I learned
in class but never really applied. Doing a post order traversal meant that
by the time that I visit a node, I have information about both the subtrees,
meaning that if they returned 0, I know I can remove them from the tree.