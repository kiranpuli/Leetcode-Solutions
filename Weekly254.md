# [1967. Number of Strings That Appear as Substrings in Word](https://leetcode.com/contest/weekly-contest-254/problems/number-of-strings-that-appear-as-substrings-in-word/)

## Approach
Just find all the occurences and count
## Code
```
class Solution:
    def numOfStrings(self, patterns: List[str], word: str) -> int:
        res=0
        for i in patterns:
            if word.find(i)!=-1:
                res+=1
        return res
```
## Complexity
### Time
**O(N*K)**
Where N is length of word and K is the avg length the words in pattern
### Space
**O(1)**
We are not using any extra space. Apart from the "res" variable

Author : [Kiran Puli](https://kiranpuli.github.io/Portfolio/)

# [1968. Array With Elements Not Equal to Average of Neighbors](https://leetcode.com/contest/weekly-contest-254/problems/array-with-elements-not-equal-to-average-of-neighbors/)

## Approach
The easiest way to solve this problem is to swap the values of the index until we satisfy the given conditions, which are
* every i in the range 1 <= i < nums.length - 1, (nums[i-1] + nums[i+1]) / 2 is not equal to nums[i].
## Code
```
class Solution:
    def rearrangeArray(self, nums: List[int]) -> List[int]:
        isValid=False
        while not isValid:
            isValid=True
            for i in range(1,len(nums)-1):
                if nums[i]== (nums[i-1]+nums[i+1])//2:
                    isValid=False
                    nums[i],nums[i+1]=nums[i+1],nums[i]
        return nums
```
## Complexity
### Time
**O(N)**
As we will be traversing through the nums and swapping the invalid pairs
### Space
**O(1)**
We are not using any extra auxiallry space

Author : [Kiran Puli](https://kiranpuli.github.io/Portfolio/)

# [1970. Last Day Where You Can Still Cross](https://leetcode.com/contest/weekly-contest-254/problems/last-day-where-you-can-still-cross/)

## Approach
Pre-requisites: Disjoint Set Union data structure

In this problem instead of checking whether we can reach last row after each flooding (from top to left)

Consider the water to be land and vice-versa
Now, the problem is converted into 
Whether we can reach the last column after each flooding (from left to right)

WHY?
We can reiterate the given question into this becoz, the only condition that can stop us from reaching from top to bottom is a flood that occurs from left to right
Refer below diagrams
![explanation](https://assets.leetcode.com/users/images/e83b0afe-0029-446b-97e6-8d65ca1d9d1b_1629000054.7985728.png)

Now, the problem is reduced to DSU as we need to add the newly flooded cells to out dsu and check if we can cover from left to right
    if YES: return the day
    else: move forward to the next day

## Code
```
class DSU:
    def __init__(self,N):
        self.par=[i for i in range(N)]
        self.span=[(N,0) for _ in range(N)]
    
    def find(self,a):
        while a!=self.par[a]:
            a=self.par[a]
        return a
    
    def union(self,a,b,col):
        a,b=self.find(a),self.find(b)
        if a==b:return
        self.par[b]=a
        tempA,tempB=a%col,b%col
        self.span[a]=(min(self.span[a][0],self.span[b][0],tempA,tempB),max(self.span[a][1],self.span[b][1],tempA,tempB))
    
class Solution:
    def latestDayToCross(self, row: int, col: int, C: List[List[int]]) -> int:
        dsu=DSU(row*col)
        grid=[[0 for _ in range(col)]for _ in range(row)]
        
        def isValid(x,y):
            if not(0<=x<row and 0<=y<col):return False
            if grid[x][y]==0:return False
            return True
        
        
        for day,(x,y) in enumerate(C):
            x,y=x-1,y-1
            grid[x][y]=1
            for cx,cy in [(x-1,y-1),(x-1,y),(x-1,y+1),(x,y+1),(x,y-1),(x+1,y-1),(x+1,y),(x+1,y+1)]:
                if isValid(cx,cy):
                    a=x*col+y
                    b=cx*col+cy
                    dsu.union(a,b,col)
                    if dsu.span[a]==(0,col-1):return day
```
## Complexity
### Time
**O(M*N)**
M,N are row,col
As length of cells is given as row*col. So evetually all cells are flooded 
### Space
**O(M*N)**
We are using extra space to store all the flooded cells

Author : [Kiran Puli](https://kiranpuli.github.io/Portfolio/)
Credits : [Bakerston](https://leetcode.com/problems/last-day-where-you-can-still-cross/discuss/1403988/Python-3-Union-Find-join-the-water-not-the-land-explanation-with-pictures.)