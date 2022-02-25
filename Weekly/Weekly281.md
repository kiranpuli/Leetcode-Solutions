# [2182. Construct String With Repeat Limit](https://leetcode.com/contest/weekly-contest-281/problems/construct-string-with-repeat-limit/)

## Approach
Conditions given are:
* Lexographically largest
* Max limit for commom element
To solve the above problemsm, we need
* Priority queue to get the best element to add.
* Add max possible number of repetitions

## Code
```
class Solution:
    def repeatLimitedString(self, s: str, limit: int) -> str:
        heap=[(-ord(k),v) for k,v in Counter(s).items()]
        heapify(heap)
        res=[]
        while heap:
            k,v = heappop(heap)
            if res and res[-1]==k:
                if not heap:break
                k2,vv=heappop(heap)
                res.append(k2)
                if vv-1>0:
                    heappush(heap,(k2,vv-1))
                heappush(heap,(k,v))
            else:
                m=min(limit,v)
                res.extend([k]*m)
                if v-m>0:
                    heappush(heap,(k,v-m))
        return "".join([chr(-i) for i in res])
```
## Complexity
### Time
**O(NlogN)**
### Space
**O(N)**

# [2183. Count Array Pairs Divisible by K](https://leetcode.com/problems/count-array-pairs-divisible-by-k/)

## Approach
So the first approach that anyone would try is O(N^2) which is brute force, But this leads to TLE, so we need to optimize this 

> If a*b%k==0 then GCD(a,k)*GCD(b,k)==0

The above theorom is used to solve the problem.
Using this, We still have to traverse through all the elements, But in the 2nd loopwe will only traverse atmost sqrt(N) times

Explanation:
consider a=GCD(a,k)
Now we need to find b such that a*b%k==0

## Code
```
from math import gcd
class Solution:
    def countPairs(self, nums: List[int], k: int) -> int:
        res=0
        N=len(nums)
        gcds=defaultdict(int)
        for i in nums:
            a=gcd(i,k)
            for b in gcds:
                if (a*b)%k==0:
                    res+=gcds[b]
            gcds[a]+=1
            
        return res
```
## Complexity
### Time
**N O(sqrt(N))** where the 
### Space
**O(sqrt(N))** where N is the number of elements

Author : [Kiran Puli](https://kiranpuli.github.io/Portfolio/)
Github Repo : [Link](https://github.com/kiranpuli/Leetcode-Solutions)