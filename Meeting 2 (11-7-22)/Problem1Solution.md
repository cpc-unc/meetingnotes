# Problem 1 Solution: [Alphabetical Strings](https://codeforces.com/problemset/problem/1547/B)

The **greedy** approach to this problem is to start from the center "a", and one by one, knock out the "b", "c", "d", etc. to the left/right of it. We can make one simple procedure to check the left/right sides, and then this algorithm can be applicable to simply the left and right endpoints of the string.

Here's a simple case for when we need to check the left and right values of "a".

```python
def solve(string):
    a_pos = string.find("a")

    # Define left and right bounds of the "correct" string so far
    left = a_pos
    right = a_pos

    we_have_found_b = False

    if right + 1 < len(string): # If the right endpoint ISN'T the last character of the string
        if string[right + 1] == "b":
            we_have_found_b = True
            right += 1
    if left - 1 >= 0: # If the left endpoint ISN'T the first character of the string
        if string[left - 1] == "b":
            we_have_found_b = True
            left -= 1
```

We check the characters immediately to the left and right of the string we have within the endpoints (which is just "a" at the beginning) to see whether the character "b" is present. And if it is, we shift the endpoint.
We need to keep going until we've exhausted all of our characters. Let's make a for loop for that.

```python
def solve(string):
    a_pos = string.find("a")

    # Define left and right bounds of the "correct" string so far
    left = a_pos
    right = a_pos

    curr_char = ord("a")

    for i in range(len(string) - 1):
        # In here, we test whether the characters to the left 
        # and right of the endpoints is the "next character".
```

Why do we have a for loop that only runs `len(string) - 1` times? Because the maximum number of times we'll need to check the left and right of the endpoints is `len(string) - 1`.

Let's say our string has the characters "a" and "b" (once each). Then we simply need to check the "b", and we're good.
Let's say our string has the characters "a", "b", and "c" (once each). Then we simply need to check the "b" and "c", and we're good.
Let's say our string has the characters "a", "b", "c", and "d" (once each). Then we simply need to check the "b", "c", and "d", and we're good.

See the pattern here? In the loop, when we check the endpoints, we only check for all the characters except for "a", which should (if the string is correctly formatted) `len(string) - 1` times.

The function `ord("a")` finds the numerical ASCII value for the character "a", which is 97. "b" is 98, "c" is 99, all the way up till "z" which is 122. So when we test for the "next character", it should have an `ord()` of `curr_char + 1`.

```python
def solve(string):
    a_pos = string.find("a")

    # Define left and right bounds of the "correct" string so far
    left = a_pos
    right = a_pos

    curr_char = ord("a")

    for i in range(len(string) - 1):
        next_found = False # Just an indicator boolean for whether or not we've found it.

        if right + 1 < len(string): # If the right endpoint ISN'T the last character of the string
            if ord(string[right + 1]) == curr_char + 1:
                next_found = True
                right += 1
        if left - 1 >= 0: # If the left endpoint ISN'T the first character of the string
            if ord(string[left - 1]) == curr_char + 1:
                next_found = True
                left -= 1
        
        if next_found:
            curr_char += 1
        else:
            print("NO")
            return
            break
```

If the next character is neither to the left nor to the right of the endpoints, then the string isn't *alphabetical*. (See problem for the definition). If it is, then we increment the `curr_char` variable to match the character we just found.

```python
def solve(string):
    a_pos = string.find("a")

    # Define left and right bounds of the "correct" string so far
    left = a_pos
    right = a_pos

    curr_char = ord("a")
    if a_pos == -1:
        print("NO")
        return

    for i in range(len(string) - 1):
        next_found = False # Just an indicator boolean for whether or not we've found it.

        if right + 1 < len(string): # If the right endpoint ISN'T the last character of the string
            if ord(string[right + 1]) == curr_char + 1:
                next_found = True
                right += 1
        if left - 1 >= 0: # If the left endpoint ISN'T the first character of the string
            if ord(string[left - 1]) == curr_char + 1:
                next_found = True
                left -= 1
        
        if next_found:
            curr_char += 1
        else:
            print("NO")
            return
            break

    print("YES")
```

At the end, if the for loop passes with no errors, then the string is *alphabetical*. See that we took care of a corner case at the beginning (if "a" isn't found, the string obviously isn't *alphabetical*).

Our solution should work for all 3 of these scenarios:

- **If the string is *alphabetical***, then the loop runs exactly `len(string) - 1` times, which means we have successfully found $n-1$ characters after "a".
- **If the string has duplicate characters**, then the loop will try to find `len(string) - 1` distinct characters, but we have less than `len(string) - 1` distinct characters because of the duplicate. That means the loop must go wrong somewhere, and it won't be able to find some character.
- **If the string skips a character**, then the loop will try to find that skipped character, which it won't find, and will fail.