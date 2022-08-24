---
layout: post
title:  "Code Example - Counting Inversions"
date:   2022-08-22 15:25:52 -0400
categories: jekyll update
---
### Overview
In this project, my goal was to create a program that count the number of inversions in an array.
An inversion exists when a smaller number is found to the right of a larger number within the array. In other words, it is a measure of how sorted the array is. 

### Algorithm
**Description of Algorithm:**
Divide the array into halves. Call the algorithm on each half until we have subarrays of size 1 or 2. Perform swaps so that the subarrays are sorted. Then compare the left element of the left subarray to the right element of the right array. If the left value is greater than the right, the count of inversions is increased by the number of indices in the left subarray. The process is repeated for the next right subarray.

**Justification of Algorithm:**
This algorithm works because it breaks the full array into subarrays and then the elements are sorted in ascending order. When elements are moved to accomplish this, we can count an inversion because an inversion essentially means a value was "out of order." This algorithm considers every inversion by breaking down the full array into small arrays making it easier to count each inversion.

### Code
```
#include <iostream>
#include "implement_hw.h"

int countInversions(const vector<int> &arr) 
{
    int sorted[arr.size()]; // initialize an array to hold sorted values
    int left = 0; // we begin with our left pointer at the first index
    int right = arr.size() - 1; // begin right pointer at the last index
    
    if ((int)arr.size() <= 1) {
        return 0; // base case, an array of size 1 will have no flips
    }
    else {
        return mergeSort(arr, sorted, left, right);
    }
}

int mergeSort(vector<int> a, int s[], int l, int r)
{
    int inversionCount = 0; // initilize a flip counter
    
    if (r > l) {
        int middle = (r - l) / 2 + l; // finds the middle of two pointers 
                                // this is where we divide for two subarrays
        
        inversionCount = inversionCount + mergeSort(a, s, l, middle); // left subarray
        inversionCount = inversionCount + mergeSort(a, s, (middle + 1), r); // right 

        inversionCount = inversionCount + merge(a, s, l, (middle + 1), r); 
            // merges sorted arrays
    }
    
    return inversionCount;
}

int merge(vector<int> a, int s[], int l, int middle, int r)
{
    // consider the 'two finger approach'
    int t1 = l; // tracker 1 points to the left subarray
    int t2 = middle; // tracker 2 points to the right subarray
    int t3 = l; // tracker 3 marks the next opening in the sorted array
    int inversionCount = 0;
    
    while ((t1 <= (middle - 1)) and (t2 <= r)) {
        // compare the indices we are pointing to 
        if (a[t1] <= a[t2]) {
            (s[t3]) = a[t1]; // move the smaller one to the sorted array
            t3++;
            t1++;
        }
        else {
            (s[t3]) = a[t2];
            t3++;
            t2++;
            
            // if the current left index is out of place, since the left
            //      subarray is sorted, the other values in that subarray are 
            //      also flipped
            inversionCount = inversionCount + (middle - t1);
        }
    }
    
    // these two loops fill in the rest of the sorted array with values left 
    //      in the subarrays
    if (t1 < middle) {
        while (t1 <= (middle - 1)) {
            (s[t3]) = a[t1];
            t3++;
            t1++;
        }
    }
    if (t2 < r) {
        while (t2 <= r) {
            (s[t3]) = a[t2];
            t3++;
            t2++;
        }
    }
    
    // replace given array with the sorted version
    for (int i = l; i <= r; i++) {
        (a[i]) = s[i];
    }
    return inversionCount;
}
```



