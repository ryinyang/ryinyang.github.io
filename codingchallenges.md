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

### Best Solution
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

### Takeaway
I learned that mathematically, 2 ^ 10 is equivalent to 5 * 2 ^ 2. We can
leverage this knowledge to turn a naive O(n) solution into an O(log(n))
solution.