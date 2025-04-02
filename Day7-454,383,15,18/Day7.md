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
