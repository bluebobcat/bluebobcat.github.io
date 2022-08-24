---
layout: post
title:  "Code Example - Maximizing a Trade"
date:   2022-08-22 15:25:52 -0400
categories: jekyll update
---
### Overview
In this project, I wanted to practice top down and bottom up programming. Imagine having a personal inventory with varying values (these values are stored in an array). Suppose you want to trade for an item of a certain value (let this be the 'goal'). This program determines the minimum number of items of each value you should trade for the desired item. Essentially, you want to maximize the trade.

### Algorithm
**Base Case:** when collected items value equals or exceeds goal.\\
**Resursion:** We recurse while the goal has not been met. In each recursive step we subtract an item value from the goal. So, the value of the goal is changing in each recursion. If the goal reaches 0, we have successfully drained it as stated in the base case. If the goal falls below 0, there is possibly a more efficient set of items to complete the trade. We should therefore recurse through a different item value. 
The recursion can be written as T(n)=sT(n)+O(1).
To find this recurrence, I considered an example. Suppose we have an array of item values =[2,3,4]. The recurrence would therefore be T(n)=sT(n-2)+sT(n-3)+sT(n-4)+O(1). I then simplified this for an overall recurrence to represent the problem.

### Code
{% highlight cpp %}
#include <iostream>
#include "implement_hw.h"

int tradeBottomup(const vector<int> &items, int goal)
{
    // initilize a table to store minimum number of items required
    int table[goal + 1];
    
    // base case
    table[0] = 0; // when there is 0 goal remaining, 0 items are needed
    
    //fill the rest of the table using INT_MAX
    for (int i = 1; i <= goal; i++) {
        table[i] = INT_MAX;
    }
    
    // now update the table by computing minimum items in bottom-up manner
    for (int i = 1; i <= goal; i++) {
        // consider the items with value less than the goal
        for (int j = 0; j < (int)items.size(); j++) {
            if (items[j] <= i) {
                int itemCount = table[i - items[j]];
                if (itemCount != INT_MAX and (itemCount + 1) < table[i]) {
                    table[i] = itemCount + 1;
                }
            }
        }
    }
    return table[goal];
}

int tradeTopdown(const vector<int> &items, int goal) 
{
    // table will hold previously calculated values (memos)
    int table[goal + 1];
    
    // fill the memos table with zeros for now 
    //      will also act as marker for which ones have been filled out
    for (int i = 0; i <= goal; i++) {
        table[i] = 0;
    }
    
    // call helper function with memo table as a parameter
    int itemCount = topDown_helper(items, goal, table);
    
    return itemCount;
}

int topDown_helper(const vector<int> &items, int goal, int memos[])
{
    // base cases:
    //    goal has been reached, exceeded, or itemCount already found
    if (goal < 0) {
        return -1;
    }
    if (goal == 0) {
        return 0;
    }
    if (memos[goal] != 0) {
        return memos[goal];
    }
    
    int itemCount = INT_MAX;
    // tries calculations starting with the different given item values
    // loops through each value
    for (int i = 0; i < (int)items.size(); i++) {
        // recursive call for topdown approach
        int temp = topDown_helper(items, goal - items[i], memos);
        
        // if we have found a better combination, update it
        if (temp >= 0 and temp < itemCount) {
            itemCount = temp + 1;
        }
    }
    // stores already found values as memos
    memos[goal] = itemCount;
    return memos[goal];
}
{% endhighlight %}



