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
        def search_negative(target,num_idx,nums):#search whether two elements sum up to target
            ls=[]
            for i in range(num_idx+1,len(nums)):
                if nums[i]+nums[num_idx]==target:
                    ls.append([nums[num_idx],nums[i]])
            return(ls)
            
        
        l=[]
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                ls=search_negative(-nums[i],j,nums)
                if ls!=[]:
                    for k in ls:
                        l.append([nums[i]]+k)          
            
        return(l)
            
 ```
 However, after running these code we'll find that we didn't remove duplicates. And if we try to remove duplicate by comparing the new combination with the combination we already have, it might take quite a long time to do so.
