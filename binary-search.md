# What is binary search
Binary search is an efficient algorithm to find a target value in a sorted array by repeatedly dividing the search interval in half, achieving O(log n) time complexity.

Beyond arrays, binary search can also be applied to the solution space of problems (e.g., searching for the minimum feasible value), making it a versatile technique in algorithm design and coding interviews.

# When to use binary search
Generally speaking, consider binary search in the following cases:
- Find a target value in sorted input (e.g., an array)
- Monotonic nature in the answer, a simple example would be to find the "first bad version of a program". If version 1.1 is valid (is a bad version), then version 1.2 is also valid (also a bad version).

# Code templates

Binary search the index of the target value in a sorted array:
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

Binary search in solution space, [LeetCode 1891. Cutting Ribbons](https://leetcode.com/problems/cutting-ribbons/description/?envType=problem-list-v2&envId=binary-search)

Notice the monotonic nature in the answer: if we can cut at least k ribbons with max length of ribbon `x`, `x-1` is also valid
```
int maxLength(vector<int>& ribbons, int k) {
    int low = 1, high = *max_element(ribbons.begin(), ribbons.end());

    while (low <= high) {
        int mid = low + (high - low) / 2;
        int cuts = 0;
        for (int r : ribbons) {
            cuts += r / mid;
            if (cuts >= k) {
                break;
            }
        }

        if (cuts >= k) { // mid is a valid answer, look for a bigger answer
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return high; // return high if we're looking for max / rightmost, etc. return low otherwise
}
```
