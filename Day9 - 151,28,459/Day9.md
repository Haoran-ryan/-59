# 151 - Reverse Words 

## Two pointers 

### takeaways 

1. Understand the role of 'fast' & 'slow' pointers
   1. fast - identify the start of a word
   2. slow - identify the end of a word 
   3. Though we are moving backwards, we still use the `fast` pointer as the 'drive' for the outer while loop
   4. Multiple loops are needed to find `end` and then `start` of a valid word
2. IMPORTANT : don't jump to define the moving condition for the outer while loop, in this case 

    ```python
    while left >= O:
        left -= 1 # THIS IS NOT THE UNIVERSAL PROGRESSION CONDITION! 
    ```

 3. a word does not need to be defined in any inner loop

### Code with comments to review 

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # Remove leading and trailing whitespace
        s = s.strip()
        res = []

        # Initialize right pointer at the end of the string
        # This acts as our "fast" pointer that drives the traversal
        right = len(s) - 1

        while right >= 0:
            # Skip spaces to find the end of a word
            # NOTE: We're looking for spaces here, not letters
            # The condition terminates when right points to a letter (end of a word)
            while right >= 0 and s[right] == ' ':
                right -= 1
            
            # If we've gone past the beginning of the string, exit
            if right < 0: 
                break

            # Now right points to the last character of a word
            # Set left pointer to find the beginning of this word
            left = right

            # Move left until we find a space or reach the beginning
            # This condition terminates when left points to a space or goes out of bounds
            while left >= 0 and s[left] != ' ':
                left -= 1
            
            # At this point:
            # - left points to the space before the word (or -1 if it's the first word)
            # - right points to the last character of the word
            # Extract the word using left+1 and right+1 as the boundaries
            word = s[left+1:right+1]
            res.append(word)
            
            # Move right to left to continue searching for the next word
            # This efficiently positions right at the next space to continue the process
            right = left
            
        # Join all words with a single space between them
        return ' '.join(res)
```