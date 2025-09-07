# What is binary search
Binary search is an efficient algorithm to find a target value in a sorted array by repeatedly dividing the search interval in half, achieving O(log n) time complexity.

Beyond arrays, binary search can also be applied to the solution space of problems (e.g., searching for the minimum feasible value), making it a versatile technique in algorithm design and coding interviews.

# When to use binary search
Generally speaking, consider binary search in the following cases:
- Find a target value in sorted input (e.g., an array)
- Monotonic nature in the answer, a simple example would be to find the "first bad version of a program". If version 1.1 is valid (is a bad version), then version 1.2 is also valid (also a bad version).
