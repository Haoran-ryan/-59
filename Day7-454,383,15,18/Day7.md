# Leetcode - 454 4Sum II

## Takeaway

1. Is the combnation of 2sums for every 2 arrays efficient?
   1. Yes, it is efficient. The time complexity is O(n^2) for the 2-sum approach, which is manageable for n up to 1000.
   2. The space complexity is O(n) for storing the sums in a hash map.
2. What is the best way to store the sums?
   1. Using a hash map (dictionary) to store the sums and their counts is efficient for lookups.
   2. This allows for quick access to the number of times a specific sum appears, which is crucial for counting valid quadruplets.

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        sum1 = dict()
        res = 0

        for a in nums1:
            for b in nums2:
                total1 = a + b
                if total1 not in sum1:  ## TODO: use .get to simplify the if statement
                    sum1[total1] = 1
                else:
                    sum1[total1] += 1

        for c in nums3:
            for d in nums4:
                total2 = - (c+d)
                if total2 in sum1:
                    res += sum1[total2]

        return res
```

# leetcode - 383 Ransom Note

## Takeaway

1. What's the problem in the following code?

```pytohn
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        res = dict()

        for i in ransomNote:
            res[i] = res.get(i,0) + 1

        for j in magazine:
            if j in res:
                res[j] -= 1

        return True if sum(res.values()) <= 0 else False
```

- The 'return' statement is incorrect.
- if some values are still greater than 0, it means that the ransom note cannot be constructed from the magazine. However other values are less than 0. By using sum(res.values()) <= 0, it will return True even if some values are greater than 0.
- The correct way to check if all values are less than or equal to 0 is to use all(res.values()) <= 0.

2. Checkout reference codes provided by 代码随想录

```python
# defaultdict

from collections import defaultdict

class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:

        hashmap = defaultdict(int)

        for x in magazine:
            hashmap[x] += 1

        for x in ransomNote:
            value = hashmap.get(x)
            if not value:
                return False
            else:
                hashmap[x] -= 1

        return True


# dictionary
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        counts = {}
        for c in magazine:
            counts[c] = counts.get(c, 0) + 1
        for c in ransomNote:
            if c not in counts or counts[c] == 0: # TODO: less code than my version
                return False
            counts[c] -= 1
        return True

# Counter
from collections import Counter

class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return not Counter(ransomNote) - Counter(magazine)

# count
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        for char in ransomNote:
            if char in magazine and ransomNote.count(char) <= magazine.count(char):
                continue
            else:
                return False
        return True
```

# Leetcode - 15 3Sum

## Takeaway

1. remove duplicates
   1. if the smallest number num[i] is greater than 0, then there would be no 'zero-sum' combination hereafter
   2. the same `nums[i]` duplicates - NOTE: indexing !
   3. the `left` `right` duplicates
2. Can I use `set` to remove duplicates in the result list ?
   1. Not easy since lists are not hashable in Python. 
   2. 
   ```python
    class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
            
        nums.sort()
        # Using a set of tuples to store unique triplets
        res_set = set()
        
        for i in range(len(nums) - 2):
            # Skip if first element is positive (can't sum to zero)
            if nums[i] > 0:
                break
                
            left = i + 1
            right = len(nums) - 1
            
            while left < right:
                cur = nums[i] + nums[left] + nums[right]
                if cur == 0:
                    # Add as tuple (hashable) to the set
                    res_set.add((nums[i], nums[left], nums[right]))
                    left += 1
                    right -= 1
                elif cur > 0:
                    right -= 1
                else:
                    left += 1

        # Convert set of tuples back to list of lists
        return [list(triplet) for triplet in res_set]

   ```

### E.g.: duplicates for the left and right pointers:

Consider the array: `[-2, -2, -2, -1, -1, 0, 0, 1, 1, 2, 2, 2]`

Let's say `i` is at index 0 (value `-2`), and we're looking for triplets that sum to 0:

1. `i = 0, left = 1, right = 11`
   - We have `-2 + -2 + 2 = -2`, which is < 0
   - Increment left to 2

2. `i = 0, left = 2, right = 11`
   - We have `-2 + -2 + 2 = -2`, which is < 0
   - Increment left to 3

3. `i = 0, left = 3, right = 11`
   - We have `-2 + -1 + 2 = -1`, which is < 0
   - Increment left to 4

...eventually we get to:

4. `i = 0, left = 9, right = 11`
   - We have `-2 + 2 + 2 = 2`, which is > 0
   - Decrement right to 10

5. `i = 0, left = 9, right = 10`
   - We have `-2 + 2 + 2 = 2`, which is > 0
   - Decrement right to 9

6. `i = 0, left = 9, right = 9`
   - This violates `left < right`, so inner loop ends

Now imagine if we had found a triplet that sums to 0:

Let's say with `i = 5, left = 6, right = 9`:
- We have `0 + 0 + 1 = 1`, which is > 0
- Decrement right to 8

With `i = 5, left = 6, right = 8`:
- We have `0 + 0 + 1 = 1`, which is > 0
- Decrement right to 7

With `i = 5, left = 6, right = 7`:
- We have `0 + 0 + 0 = 0`, so we found a triplet!
- We add `[0, 0, 0]` to our result

Without duplicate skipping, we would just increment left and decrement right:
- `left = 7, right = 6`
- This violates `left < right`, so inner loop ends

But with proper duplicate skipping:
1. We check if the next left element is a duplicate (if `nums[left] == nums[left+1]`)
2. We check if the next right element is a duplicate (if `nums[right] == nums[right-1]`)

Since both `nums[7]` and `nums[6]` are `0` (duplicates), we would skip them and move to the next unique elements, avoiding duplicate triplets in our result.


## Mistake analysis

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) == 3:
            return nums if sum(nums) == 0 else []
            # ERROR: Should return [[nums[0], nums[1], nums[2]]] instead of the array itself
            # This is returning a 1D array when the function should return a 2D array of triplets
        
        nums.sort()
        res = []
        i = 0  # Unnecessary since you're using a for loop
        
        if nums[0] > 0:
            return res
        
        for i in range(len(nums) - 2):
            left = i + 1
            right = len(nums)-1
            
            # ERROR: Wrong duplicate checking logic
            if nums[i] == nums[i + 1]:
                continue
                # This skips the current element if it's equal to the NEXT element
                # Should instead skip if current element equals PREVIOUS element
                # Current approach leads to missing valid triplets
            
            while left < right:
                cur = nums[i] + nums[left] + nums[right]
                if cur == 0:
                    res.append([nums[i], nums[left], nums[right]])
                    
                    # ERROR: Missing duplicate checking for left and right pointers
                    # Need to skip duplicate values for left and right here
                    # Without this, you'll get duplicate triplets in the result
                    
                    left += 1
                    right -= 1
                elif cur > 0:
                    right -= 1
                else:
                    left += 1

        return res
```

# Leetcode - 18 4Sum

## My crapy code 

```python

class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        res = []
        
        # ISSUE: Missing sorting step
        # FIX: Add 'nums.sort()' here which is critical for:
        # 1. Duplicate handling
        # 2. Efficient two-pointer technique
        # 3. Pruning the search space effectively
        
        for i in range(len(nums)):
            # ISSUE: No duplicate checking for first number
            # FIX: Add 'if i > 0 and nums[i] == nums[i-1]: continue'
            
            left = i + 1
            right = len(nums) - 1
            
            # ISSUE: This while loop's purpose is unclear
            # FIX: This should be a second for loop to fix the second number
            while left < right - 1:
                # ISSUE: Only calculating sum of 3 numbers, not 4
                cur = nums[i] + nums[left] + nums[right]

                # ISSUE: Incorrect pruning logic 
                # Without considering the 4th number properly
                if cur >= target:
                    right -= 1
                    continue
                    
                else:
                    # ISSUE: Nested while loop with same variables creates confusion
                    # The nested while loop should be replaced with a cleaner approach
                    while left < right - 1:
                        # ISSUE: This for loop changes the dynamics of the two pointers
                        # FIX: Remove this for loop and use a single mid pointer instead
                        for j in range(left + 1, right):
                            candidate = nums[j]
                            
                            # ISSUE: Logic doesn't handle all cases correctly
                            # and pointer movements are inconsistent
                            if cur + candidate > target:
                                # ISSUE: Decreasing right inside the j-loop breaks the logic
                                right -= 1
                                continue
                            elif cur + candidate < target:
                                # ISSUE: Increasing left inside the j-loop breaks the logic
                                left += 1
                                continue
                            else:
                                # ISSUE: Syntax error - missing closing bracket
                                # ISSUE: No duplicate handling after finding a valid quadruplet
                                res.append([nums[i],nums[left],nums[j],nums[right])
                                break
                                # ISSUE: After finding a quadruplet, the pointers aren't
                                # properly advanced to look for more solutions
```

## Takeways
1. How to remove duplicates?
2. Clean up the loop logics in the old version !