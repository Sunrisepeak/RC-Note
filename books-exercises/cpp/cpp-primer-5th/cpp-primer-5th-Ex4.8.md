---
title: CppPrimer5th : Exrecise Section 4.8 
mathjax: false
tags: 
	- c++
	- bitewise op
	- 位操作
categories: c++

---

### Ex: 4.25

在`int`为32bit，`q` 的位模式为 `01110001` 的情况下`~'q' << 6`的结果是？



####  Program Test(g++ for linux):

```c++
#include <iostream>
int main() {
    unsigned char c = 'q';  // 01110001
    
    int ans = ~c << 6;
    
    std::cout << ans << std::endl;
    return 0;
}
```

Output:

```
-7296
```

#### Analysis :

##### code perform step: 

+ left-associative:  `~'q' << 6` equivalence with `(~'q') << 6`

+ promoted to int for `~'q'`:  `~(01110001b)` to `~(00000000 00000000 00000000 01110001 b)`

+ Bitwise Not operator(`~`): `(11111111 11111111 11111111 1000110b) << 6`
<!-- more -->
+ left-shift operator(`<<`) : `11 11111111 11111111 1000110 000000b`

+ computation binary number:

  ```c++
   11111111 11111111 1110001 10000000b
   
   True form: 10000000 00000000 0001110 10000000 b
   	- (2^11 + 2^10 + 2^9 + 2^7) = - (2048 + 1024 + 512 + 128) = -3712 what? Oh, No
  ```
##### debug:
```c++
     	oh, lose an zero in Bitwise Not operator step. so computation:
     modify:
     	(11111111 11111111 11111111 100011 1 0b) << 6
     	11111111 11111111 11100011 10000000b
     new [true form] : 10000000 00000000 00011100 10000000 b
     	- (2^12 + 2^11 + 2^10  + 2^7) = - (4096 + 2048 + 1024 + 128) = -7296 Oh, Yell
            
```

​    







