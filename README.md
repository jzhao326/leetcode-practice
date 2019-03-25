# leetcode-practice
practice of algorithm
## 15. 3Sum
**Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.**

**example input**:     
[-1, 0, 1, 2, -1, -4]

**example output**:      
[
  [-1, 0, 1],
  [-1, -1, 2]
]

 First of all, the most natrual thought I have is simply search all possible combinations of 3 numbers in the given array and find out combinations that satisfies the request. How to do this? We'll go through every single element in the array, and for each element we'll search all the numbers behind it: are there 2 numbers sum up to this element's opposite number? If there are, append these two numbers to our list later on we'll return. The code is following:
 ```
 class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        def search_sum(target,num_idx,nums):#search whether two elements sum up to target
            ls=[]
            for i in range(num_idx+1,len(nums)):
                if nums[i]+nums[num_idx]==target:
                    ls.append([nums[num_idx],nums[i]])
            return(ls)
            
        l=[]
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                ls=search_sum(-nums[i],j,nums)
                if ls!=[]:
                    for k in ls:
                        l.append([nums[i]]+k)                  
        return(l)
            
 ```
 However, after running these code we'll find that we didn't remove duplicates. And if we try to remove duplicate by comparing the new combination with the combination we already have, it might take quite a long time to do so. So we consider another possible way to solve this problem. If we observe the expected output we'll notice that the list is ordered increasingly, so we try to order the number array before we deal with it.
 
 ```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        def search_sum(target,search_ls):#search whether 2 elements in l sum up to target
            l=[]
            if len(search_ls)<=1:
                return l
            
            search_ls=[search_ls[0]-1]+search_ls+[search_ls[-1]+1]#prevent index out range
            i,j=1,len(search_ls)-2
            
            while i<j:
                if search_ls[i]+search_ls[j]==target:
                    l.append([search_ls[i],search_ls[j]])
                    i+=1
                    j-=1
                    while search_ls[i]==search_ls[i-1]:
                        i+=1
                    while search_ls[j]==search_ls[j+1]:
                        j-=1             
                elif search_ls[i]+search_ls[j]>target:
                    j-=1
                    while search_ls[j]==search_ls[j+1]:
                        j-=1
                else:
                    i+=1
                    while search_ls[i]==search_ls[i-1]:
                        i+=1
            return l 
        
        ls=[]
        nums.sort()
        if len(nums)>=3:
            for i in range(len(nums)):               
                if (i>=1 and (nums[i]!=nums[i-1])) or i==0:
                    l = search_sum(-nums[i],nums[i+1:]) 
                    if l!=[]:
                        for j in l:
                            ls.append([nums[i]]+j)
                
                    
        return ls
 ```
 Basically this method improved the function of searching. It will check both ends and adjust the index according to the bias to target. To eliminate the duplicates, we skip the index that correspond to the same value in the list.
 
 ## 227.Basic Calculator 2
**Implement a basic calculator to evaluate a simple expression string.
The expression string contains only non-negative integers, +, -, \*, / operators and empty spaces . The integer division should truncate toward zero.**

**example input**:     
"3+2*2"

**example output**:      
7

Use three variables to store the current result, sign and number while marching through the string. Considering the precedence of \* and /, we calculate out the result related to these 2 signs.

```
class Solution:
    def calculate(self, s):
        num, stack, sign = 0, [], "+" #initiate 3 temp
        for i in range(len(s)):
            if s[i].isdigit():
                num = num * 10 + int(s[i])
            if s[i] in "+-*/" or i == len(s) - 1:
                if sign == "+":
                    stack.append(num)
                elif sign == "-":
                    stack.append(-num)
                elif sign == "*":
                    stack.append(stack.pop()*num)
                else:
                    stack.append(int(stack.pop()/num))#do * and / first
                num = 0
                sign = s[i]
        return sum(stack)#do + and - in the end

```
