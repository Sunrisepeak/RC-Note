---
title: Divide-and-Conquer >> Maximum Contiguous Subsequence Sum Algorithms
mathjax: true
tags:
  - Divide-and-Conquer
  - Online-Algorithms
  - Recursive
  - Algorithms
  - 分治
  - 在线处理
  - 递归
categories:
  - cs
  - DS&Algorithms
abbrlink: 282068989
---



#### Algorithms 1

> 'Divide-and-Conquer' by Recursive



#####  Algorithms Thinking

> ​	For one thing, take a set divided into two part with left and right and work out max-sub-sum respectively. To left and right hand by recursive to process. In addition, computation maximum border subseq-sum of mid to left and to hand then add get new maxSubSum. In the end, return a max value in the three 'maxSubSum'.

<!-- more -->

##### Code

```c
int maxSubSum(const vector<int> &arr, int left, int right) {
    if (left == right) {
        if (arr[left] > 0) {
            return arr[left];
        } else {
            return 0;
        }
    } else {
        int mid = (left + right) / 2;
        int leftMaxSSum = maxSubSum(arr, left, mid);
        int rightMaxSSum = maxSubSum(arr, mid + 1, right);

        int maxLeftBorderSum = 0, leftBorderSum = 0; 
        for (int i = mid; i >= left; i--) {
            leftBorderSum += arr[i];
            if (leftBorderSum > maxLeftBorderSum) {
                maxLeftBorderSum = leftBorderSum;
            }
        }

        int maxRightBorderSum = 0, rightBorderSum = 0; 
        for (int i = mid + 1; i <= right; i++) {
            rightBorderSum += arr[i];
            if (rightBorderSum > maxRightBorderSum) {
                maxRightBorderSum = rightBorderSum;
            }
        }

        return max(maxLeftBorderSum + maxRightBorderSum, max(leftMaxSSum, rightMaxSSum));
    }
}
```



##### Analysis

###### time complexity: $O(nlogn)$



#### Algorithms 2

> Online Algorithms



#####  Algorithms Thinking

```
	[---------------------------]
           |    |    |
           L    p    i       index-pointer
```

##### Code

```c
int maxSubMax(const vector<int> &arr) {
    int sum = 0, maxSum = 0;
    for (auto v : arr) {
        sum += v;
        if (sum > maxSum) {
            maxSum = sum;
        } else if (sum < 0) {
            sum = 0;
        }
    }
    return maxSum;
}
```



##### Proof

> Why L-pointer skip to i when sum less than zero?
>
> assumption:	exist L < p < i , let [p, i] > 0
>
> > we known [L, p) > 0
>
> > again, obvious [L, i] = [L, p) + [p, i]  > 0;
>
> > but [L, i] < 0, so [p, i] > 0 is mistake assumption.
>
> > so, [p, i] <= 0, can let L = i when [L, i] < 0;



##### Analysis

> tiem complexity: O(n)

#### Reference 

+ [Data Structures and Algorithms Analysis in C++](https://book.douban.com/subject/26910665/)

#### QuickLink

+ [SPeakNote Blog](https://sunrisepeak.github.io/)
+ [Makedown File Address](https://github.com/Sunrisepeak/RC-Note)
+ [Github](https://github.com/Sunrisepeak)