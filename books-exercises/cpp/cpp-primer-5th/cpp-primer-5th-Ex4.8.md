---
title: CppPrimer5th Exrecise Section 4.8
mathjax: false
tags:
  - c++
  - Bit-Op
  - 位操作
  - Logic-Op
categories:
  - c++
  - cpp-primer
abbrlink: 1776624505
---

### Ex: 4.25

在`int`为32bit，`q` 的位模式为 `01110001` 的情况下`~'q' << 6`的结果是？



####  Program Test(g++ for linux)

##### C++ Code

```c++
#include <iostream>
int main() {
    unsigned char c = 'q';  // 01110001
    
    int ans = ~c << 6;
    
    std::cout << ans << std::endl;
    return 0;
}
```

##### Output:

```
-7296
```

#### Analysis 

##### Code Perform Step

+ left-associative:  `~'q' << 6` equivalence with `(~'q') << 6`

+ promoted to int for `~'q'`:  `~(01110001b)` to `~(00000000 00000000 00000000 01110001 b)`

+ Bitwise Not operator(`~`): `(11111111 11111111 11111111 1000110b) << 6`
<!-- more -->
+ left-shift operator(`<<`) : `11 11111111 11111111 1000110 000000b`

+ computation binary number:

     11111111 11111111 1110001 10000000b
     True form: 10000000 00000000 0001110 10000000 b

  ​	-(2^11 + 2^10 + 2^9 + 2^7)  =  - (2048 + 1024 + 512 + 128) = -3712 what? Oh, No


##### Debug
```
 oh, lose an zero in Bitwise Not operator step. so computation:
 modify:
    (11111111 11111111 11111111 100011 1 0b) << 6
     11111111 11111111 11100011 10000000 b
 new [true form] : 10000000 00000000 00011100 10000000 b
   - (2^12 + 2^11 + 2^10  + 2^7) = - (4096 + 2048 + 1024 + 128) = -7296 Oh, Yell
            
```

​    

### Ex:4.26

+ Because of Bits of int type could different in machine.As usual, it's is that great than and quals 16bit.


### Ex:4.27

What is the result of each of these of expressions?

```c++
unsigned long ull = 3, ul2 = 7;
    (a) ull & ul2 
    (b) ull | ul2 
    (c) ull && ul2 
    (d) ull || ul2 
```



#### Program Test(g++ for linux)

##### C++ Code

```c++
#include <iostream>

using std::endl;
using std::cout;

int main() {
    unsigned long ul1 = 3, ul2 = 7;
    cout << (ul1 & ul2) << endl;
    cout << (ul1 | ul2) << endl;
    cout << (ul1 && ul2) << endl;
    cout << (ul1 || ul2) << endl;
    return 0;
}
```

##### Output

```
3
7
1
1
```



#### Analysis

##### Binary Number View(bit op & logic op)

```
ul1 = 3  ->  000...00011
ul2 = 7  ->  000...00111
```

###### Bit-Op (&, |)

& op

```
   000...00011
&  000...00111
------------------------
   000...00011  --> 3
```

| op

```
   000...00011
|  000...00111
------------------------
   000...00111  --> 7
```

###### Logic-Op (&&, ||)

```
7 && 3 -->  true && true --> true(1)
```

|| op

```
7 || 3 -->  true && true --> true(1)
```



### Environment
+ Ubuntu 18.x

+ g++ 

  ```
  g++ (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
  Copyright (C) 2017 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
  ```



### Reference

+ cpp primer 5th
+ Youdao Dictionary (ha ha)[This is the worst he had ever been back a pot]


