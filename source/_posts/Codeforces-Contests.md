---
title: "Codeforces Contests"
date: 2021-01-06
categories:
  - codeforces
---

# Main Focus

*   First 4 Problems in Div. 3
*   First 3 Problems in Div. 2

# Round #694 (Div. 2)

The first problem is summed up as following:

1.  Given…

*   an array a of length n
*   an integer x

You can…

*   Replace two adjacent elements of the array by their sum (random choice of numbers is allowed)

Define…

*   The beauty of an array is defined as the sum of all array elements after each of them is divided by x:
    
    with $b\_i=$one element in the array b \[n\], beauty $=\\sum\_{i=1}^{k}\\lceil\\frac{b\_i}{x}\\rceil$
    
*   Say there is an array \[ 6 , 4 , 11 \] and the beauty of it can be = 7 or = 8. In the case of 7 we take the last two numbers, sum them up and calculate the beauty, which yields an result of 7
    

**Task: Define the maximum and minimum beauty of a given input**

The crux of the problem is to realize that we are taking the ceiling and it means that we will get the most out of a division if the number $b\_i$ is not divisible by x. If $b\_i\\%x=0$ , then the ceiling makes no difference.

The maximal beauty= $\\lceil\\frac{\\text{every number in the array}}{x}\\rceil$

The minimal beauty= $\\lceil\\frac{\\sum\\text{all numbers in the array}}{x}\\rceil$

Solution without any data structure

```c++
#include <iostream>
#include <math.h>
using namespace std;

int main()
{
    int t;
    cin >> t;
    while (t > 0)
    {
        long n;
        double x;
        cin >> n >> x;
        long long max = 0, min = 0;
        while (n > 0)
        {
            long long tmp;
            cin >> tmp;
            min += tmp; // minimum = ceiling of (sum / x)
            max += ceil(tmp/x); // maximum = sum of ceiling (number / x)
            n--;
        }
        min = ceil(min/x);
        cout << min <<" "<< max << endl;
        t--;
    }
}
```

1.  **Problem:**
    

Simply put, we will have an array a with length n and an divisor x. Iterate over the array and sum up the elements, when the current element, name it $q$, can be divided by x, put x copies of this element/x at the back of the array, else stop dividing the current element and do the sum only.

We know that we will have to sum up all elements anyway, so it’s alright if we do that first and figure out the result later

1.  Store the array. Sum up all the elements. .
    
2.  Enter a while(true) loop and Initilize a variable “count=1” of type long.
    
    **Now consider the problem:**
    
    We have the sum of all elements now. What else we need is the $\\frac{\\text{current element}}{x}\\cdot x$ and this gives us the current element again.
    
    For example, we have an array \[4,6,8\] and x=2. While we read the array, we do the summing at the same time which yields us 4+6+8=18.
    
    That’s not final, since we know that all 4, 6, 8 are divisible by 2→ we need to divide every number by 2 and sum these numbers up. 4/2=2, add x copies of 2, 6/2=3, add x copies of 3, 8/2=4, add x copies of 4.
    
    After the division, the array is going to be \[4,6,8,2,2,2,3,3,4,4\]. It seems that we will have to start to divide the first pink 2 by x and carry on until we get to the 3 since 3 cannot divide x.
    
    **However**, we notice that we will always get x copies of ( current element/x ), yielding us the current element. So instead of actually inserting the x copies of (current element/x), we can add the x\*(current element/x) to our sum directly and divide the current element by x.
    
    **Note** that $x\\cdot\\frac{\\text{current element}}{x}=\\text{current element}$ in the first time looping over the array,
    
    Second time: $x\\cdot\\frac{\\frac{\\text{current element}}{x}}{x}=\\frac{\\text{current element}}{x}$
    
    and so on. So we need to multiply the current element with x every time we do the sum.
    
    **Demo:**
    
    Again, array = \[4,6,8\], x= 2:
    
    \[4,6,8\], sum = 18 → start dividing. Divide every element since they’re all divisible by x
    
    → \[2,3,4\], sum = 18+ 2_x+3_x+4x= 36 → keep dividing since the 1. number is 2
    
    → \[1,…hit 3 ..\] **but 3%2!=0** stop summing up right there
    
    → \[1,stop here\], sum=36+1x=38
    
    One last thing to add: we have to multiply the additional numbers we add to the original sum (in the example, sum=18) with count\*x every time after we loop over the whole array
    
    ```cpp
    long count=1;
    while(condition){
      for(loop through array with index i )
      {...sum+=count*(arr[i])...}
      count*=x;
    }
    ```
    
    My solution
    
    ```c++
    #include <iostream>
    using namespace std;
    
    int main()
    {
        int t;
        cin >> t;
        while (t > 0)
        {
            long long n, x, count = 1, sum = 0;
            cin >> n >> x;
            long long a[n];
            for (int i = 0; i < n; i++)
            {
                cin >> a[i];
                sum += a[i];
            }
            
            bool run = true;
            while (run)
            {
                for (int i = 0; i < n; i++)
                {
                    if (a[i] % x == 0&&run)
                    {
                        sum += count * a[i];
                        a[i] /= x;
                    }
                    else
                    {
                        run = false;
                        break;
                    }   
                }
                 count *= x;
            }
            cout << sum << endl;
            t--;
        }
    }
    ```
