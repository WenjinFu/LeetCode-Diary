

## LeetCode Problem Solution 

## *   Binary Search & Time Complexity - logn



## 4.Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```mathematica
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```mathematica
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```



### CODE:

### Solution 1 (Hard-coding/Ugly/Time Complex Wrong)

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        result = nums1+nums2
        result.sort()
        if len(result) % 2.0 == 0:
            return (result[ (len(result)/2) -1]  +  result[(len(result)/2)])/2.0
        else:
            return(result[len(result)/2])
```

### Solution 2 (Google master explanation/Super/Complex)

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        '''
        1. Finding a pivot point where all elements to the left are smaller and all elements to the left are smaller and all elements to the right are greater,
           
            x x [x]|[x] x x  lLong, rLong
              y [y]|[y] y   lShort, rShort
              
            x x y [y] [x] | [y] y x [x] x
            
        2. Any pivot point in the smaller array has a corresponding point on the large array that divides the total # of elements in two
            x x x x x|x
              y y y y
              
        3. After picking a pivot point, it's possible to determine whether we need to go left or right
            1 2 3|4 5 6 
              2 3|4 5
              
        4. Code outline:
           1. Binary search on small array
           2. getIndices: m -> lLong, rLong, etc.
           3. getDirection: lLong, rLong, etc. -> get direction (left or right)
           4. getResult: rLong, lLong, etc. -> calculate the median
           
        ## odd case           
           y y [y]|[y] y
               [x]|[x] x x
           y [x] y [y] | [x] [y] y x y
           9/2 -> 4
        
        Time O(log(n))
        Space O(1)
        
        n is the number of the elements in the smaller array
        '''
        
        
        #get value at index i of array arr
        def getVal(arr, i):
            if i == -1:
                return float('-inf')
            if i == len(arr):
                return float('inf')
            return arr[i]
        
        #return (rlShort, rShort, lLong, rLong)
        def getIndices(rShort, aShort, aLong):
            midIndex = (len(aShort) + len(aLong)) / 2
            rLong = midIndex - rShort
            return (rShort - 1, rShort, rLong - 1, rLong)
        
        #determine whether to go left or right
        def getDirection(lShort, rShort, lLong, rLong, aShort, aLong):
            if getVal(aShort, lShort) > getVal(aLong, rLong):
                return -1
            elif getVal(aLong, lLong) > getVal(aShort, rShort):
                return 1
            else:
                return 0
        #calculate the median 
        def getResult(lShort, rShort, lLong, rLong, aShort, aLong):
            odd = (len(aShort) + len(aLong))%2
            if odd:
                return min(getVal(aLong, rLong), getVal(aShort, rShort))
            else:
                return (max(getVal(aShort, lShort), getVal(aLong, lLong))
                       + min(getVal(aShort, rShort), getVal(aLong, rLong))) / 2.0
            
        
        aShort = nums1
        aLong = nums2
        if len(nums1) > len(nums2):
            aShort = nums2
            aLong = nums1
            
        #temporary variables
        lShort = rShort = lLong = rLong = d =1
        
        #Binary search on the smaller array
        l = 0
        r = len(aShort)
        while d != 0:
            m = (l + r) / 2
            #get indices from our m
            lShort, rShort, lLong, rLong = getIndices(m, aShort, aLong)
            #get out d (direction)
            d = getDirection(lShort, rShort, lLong, rLong, aShort, aLong)
            #determine our direction
            if d < 0:
                r = m - 1
            elif d > 0:
                l = m + 1
                
        #calculate median
        return getResult(lShort, rShort, lLong, rLong, aShort, aLong)
        
        
        
```

###  Solution 3 (Shorter version)

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        # sort array based on the length
        a, b = sorted((nums1, nums2), key=len)
        m, n = len(a), len(b)
        after = (m + n - 1) / 2
        lo, hi = 0, m
        # binary search on smaller array
        while lo < hi:
            i = (lo + hi) / 2
            if after-i-1 < 0 or a[i] >= b[after-i-1]:
                hi = i
            else:
                lo = i + 1
        i = lo
        nextfew = sorted(a[i:i+2] + b[after-i:after-i+2])
        return (nextfew[0] + nextfew[1 - (m+n)%2]) / 2.0
```



