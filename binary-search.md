# What is binary search
Binary search is an efficient algorithm to find a target value in a sorted array by repeatedly dividing the search interval in half, achieving O(log n) time complexity.

Beyond arrays, binary search can also be applied to the solution space of problems (e.g., searching for the minimum feasible value), making it a versatile technique in algorithm design and coding interviews.

# When to use binary search
Generally speaking, consider binary search in the following cases:
- Find a target value in sorted input (e.g., an array)
- Monotonic nature in the answer, a simple example would be to find the "first bad version of a program". If version 1.1 is valid (is a bad version), then version 1.2 is also valid (also a bad version).

# Code templates

Finding the index of the target value in a sorted array:
```
int binary_search(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            return mid;
        }

        if (arr[mid] < target) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return -1;
}
```