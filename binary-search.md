# What is binary search
Binary search is an efficient algorithm to find a target value in a sorted array by repeatedly dividing the search interval in half, achieving O(log n) time complexity.

Beyond arrays, binary search can also be applied to the solution space of problems (e.g., searching for the minimum feasible value), making it a versatile technique in algorithm design and coding interviews.

# When to use binary search
Consider binary search in the following cases:
- Find a target value in sorted input (e.g., an array)
- Monotonic nature in the answer, e.g., if answer `x` is valid, `x-1` is guaranteed to be valid as well

# Examples

Binary search the index of the target value in a sorted array
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

A more complex "solution space" binary search, marked as LeetCode hard, [2040. Kth Smallest Product of Two Sorted Arrays](https://leetcode.com/problems/kth-smallest-product-of-two-sorted-arrays/?envType=problem-list-v2&envId=binary-search)

But once you understand the pattern, it should really be a medium:
```
long long kthSmallestProduct(vector<int>& nums1, vector<int>& nums2, long long k) {
    // find the smallest value "guess" s.t., number of values that are smaller or equal than "guess" >= k

    // optimization tip: linear search on smaller input and binary search on the larger input
    // think about the extreme case when one array has 1 element and the other has 1 million, we'd want to binary search on the million array!
    if (nums1.size() > nums2.size()) {
        swap(nums1, nums2);
    }

    // solution space lower/upper bound has to be one of the following values
    vector<long long> candidates = { (long long)nums1.front() * nums2.front(), (long long)nums1.front() * nums2.back(),
        (long long)nums1.back() * nums2.front(), (long long)nums1.back() * nums2.back() };
    sort(candidates.begin(), candidates.end());

    long long low = candidates.front(), high = candidates.back();
    while (low <= high) {
        long long mid = low + (high - low) / 2;
        long long count = 0;
        for (auto x1 : nums1) {
            count += countSmaller(nums2, x1, mid); // number of elements "x2" in nums2 s.t., x1 * x2 <= "mid"
        }
        if (count >= k) { // valid, look for a smaller answer
            high = mid - 1;
        }
        else {
            low = mid + 1;
        }
    }
    return low; // we're looking for smallest
}

int countSmaller(vector<int> const& nums2, long long x1, long long target) {
    int n2 = nums2.size();
    int left = 0, right = n2 - 1;
    while (left <= right) { // find the first index of nums2 s.t., x1 * x2 > target
        int mid = left + (right - left) / 2;
        if ((x1 >= 0 && nums2[mid] * x1 <= target) || (x1 < 0 && nums2[mid] * x1 > target)) { // note negative values
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    if (x1 >= 0) {
        return left;
    }
    else {
        return n2 - left;
    }
}
```
