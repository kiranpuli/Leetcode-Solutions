# Russian Peasant
Used to multiply 2 large numbers **Efficiently**

## Recursive
```
def mul(a,b):
    if a==1:return b
    if a&1:
        return b+mul(a>>1,b<<1)
    else:
        return mul(a>>1,b<<1)
```
Time Complexity : O(log(min(a,b)))
Space Complexity : Uses Stack space

## Iterative
```
def mul(a,b):
    res=0
    while a>0:
        if a&1:res+=b
        a=a>>1
        b=b<<1
    return res
```
Time Complexity : O(log(min(a,b)))


## Useful Notes for CP
* Use this method when (a*b) % M is needed
```
def mul(a,b,M):
    res=0
    while a>0:
        if a&1:
            res+=b
            if res>=M:res-=M
        a=a>>1
        b=b<<1
        if b>=M:b-=M
    return res
```