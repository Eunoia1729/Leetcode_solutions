# Overview
I would recommend readers to solve [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) before trying this since XOR is a commutative operation similar to addition.
The problem is easy once you realise precomputation of prefixes can be used and the fact that similar to subtraction being the inverse operation for addition, XOR is the inverse operation for XOR.

![equation](https://latex.codecogs.com/gif.latex?range\\_xor(L,&space;R))
        
Let range_xor(L, R) denote the XOR of elements from indices L to R in the given array and ^ denote the XOR operation.

# Theory
1. XOR of a number with itself is 0. 
   >     A ^ A = 0
2. XOR of a number with 0 is the number itself. 
   >     A ^ 0 = A

# Approach (Caching)
## Intuition:

As a naive approach, we can use a for loop to XOR each element in the range [L<sub>*i*</sub>, R<sub>*i*</sub>] for each query.
If the number of queries are large, for an arbitary range [l, r] we might have computed range_xor(l, r) more than once. Thus, there are repeated subtasks and caching might help you cut-off repeated same computations for the range queries.
Here, the precomputation/caching is storing the prefix XORs.

The meaning of prefix XOR is easy to understand. `prefix_xor[i]` is the XOR of arr[0..i] or in other words, range_xor(0, i).

*prefix_xor* can be precomputed as follows:    
As there is only arr[0] in the range [0,0]:    
    >   range_xor(0,0) = arr[0]    
For i > 0:          
    >   range_xor(0, i) = arr[i] ^ range_xor(0, i - 1)

The same is implemeneted in C++ code as follows:
```       
prefix_xor[0] = arr[0];
for(int i = 1; i < arr.size(); ++i)
    prefix_xor[i] = arr[i]^prefix_xor[i - 1];   
```    
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%28L_i%2C%20R_i%29)         
![equation](https://latex.codecogs.com/png.latex?%5Ctextup%7BXOR%20both%20sides%20with%20%7D%20range%5C_xor%280%2C%20L_i%20-%201%29%3A)      
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%28L_i%2C%20R_i%29)       
![equation]()
  > range_xor(0, Ri) = range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>)   
    
  > range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(0, R<sub>*i*</sub>) = range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(0, L<sub>*i*</sub> - 1)^range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>)    
  > # Overview
I would recommend readers to solve [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) before trying this since XOR is a commutative operation similar to addition.
The problem is easy once you realise precomputation of prefixes can be used and the fact that similar to subtraction being the inverse operation for addition, XOR is the inverse operation for XOR.

![equation](https://latex.codecogs.com/gif.latex?range\\_xor(L,&space;R))
        
Let range_xor(L, R) denote the XOR of elements from indices L to R in the given array and ^ denote the XOR operation.

# Theory
1. XOR of a number with itself is 0. 
   >     A ^ A = 0
2. XOR of a number with 0 is the number itself. 
   >     A ^ 0 = A

# Approach (Caching)
## Intuition:

As a naive approach, we can use a for loop to XOR each element in the range [L<sub>*i*</sub>, R<sub>*i*</sub>] for each query.
If the number of queries are large, for an arbitary range [l, r] we might have computed range_xor(l, r) more than once. Thus, there are repeated subtasks and caching might help you cut-off repeated same computations for the range queries.
Here, the precomputation/caching is storing the prefix XORs.

The meaning of prefix XOR is easy to understand. `prefix_xor[i]` is the XOR of arr[0..i] or in other words, range_xor(0, i).

*prefix_xor* can be precomputed as follows:    
As there is only arr[0] in the range [0,0]:    
    >   range_xor(0,0) = arr[0]    
For i > 0:          
    >   range_xor(0, i) = arr[i] ^ range_xor(0, i - 1)

The same is implemeneted in C++ code as follows:
```       
prefix_xor[0] = arr[0];
for(int i = 1; i < arr.size(); ++i)
    prefix_xor[i] = arr[i]^prefix_xor[i - 1];   
```    
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%28L_i%2C%20R_i%29)   
![equation](https://latex.codecogs.com/png.latex?%5Ctextup%7BXOR%20both%20sides%20with%20%7D%20range%5C_xor%281%2C%20L_i%20-%201%29%3A)   
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%28L_i%2C%20R_i%29)    
![equation](https://latex.codecogs.com/png.latex?%5Ctextup%7BAfter%20re-arranging%2C%20we%20get%3A%7D)   
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%28L_i%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20R_i%29)         
> range_xor(0, Ri) = range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>)   
    
  > range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(0, R<sub>*i*</sub>) = range_xor(0, L<sub>*i*</sub> - 1) ^ range_xor(0, L<sub>*i*</sub> - 1)^range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>)    
  > range_xor(0, L<sub>*i*</sub> - 1)^range_xor(0, R<sub>*i*</sub>) = range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>).   
Re-arranging, we get:    
  > range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>) = range_xor(0, L<sub>*i*</sub> - 1)^range_xor(0, R<sub>*i*</sub>)   
   
Thus, the precomputed XOR's can be used to obtain range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>) using the above formula. 

## Code
```   
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {

        vector<int> prefix_xor( arr.size()); 

        prefix_xor[0] = arr[0];
        for( int i = 1; i < arr.size(); ++i)
            prefix_xor[i] = arr[i] ^ prefix_xor[i - 1];

        vector<int> res;
        int left, right;
        for( auto query: queries) {
            left = query[0], right = query[1];
            if( left == 0)
                res.push_back( prefix_xor[right] );
            else
                res.push_back( prefix_xor[left - 1] ^ prefix_xor[right] );
        }

        return res;
    }
};   
```
## Time Complexity
    Time: ![equation](https://latex.codecogs.com/gif.latex?O(n&space;&plus;&space;m)) , where n is the number of elements, and m is the number of queries.
    Memory: O(n) extra space to store the precomputation array, prefix_xor.
    

### Bonus:
    Extra space can be made O(1) by computing the prefix XOR in-place on the array, *arr*..   
Re-arranging, we get:    
  > range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>) = range_xor(0, L<sub>*i*</sub> - 1)^range_xor(0, R<sub>*i*</sub>)   
   
Thus, the precomputed XOR's can be used to obtain range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>) using the above formula. 

## Code
```   
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {

        vector<int> prefix_xor( arr.size()); 

        prefix_xor[0] = arr[0];
        for( int i = 1; i < arr.size(); ++i)
            prefix_xor[i] = arr[i] ^ prefix_xor[i - 1];

        vector<int> res;
        int left, right;
        for( auto query: queries) {
            left = query[0], right = query[1];
            if( left == 0)
                res.push_back( prefix_xor[right] );
            else
                res.push_back( prefix_xor[left - 1] ^ prefix_xor[right] );
        }

        return res;
    }
};   
```
## Time Complexity
    Time: ![equation](https://latex.codecogs.com/gif.latex?O(n&space;&plus;&space;m)) , where n is the number of elements, and m is the number of queries.
    Memory: O(n) extra space to store the precomputation array, prefix_xor.
    

### Bonus:
    Extra space can be made O(1) by computing the prefix XOR in-place on the array, *arr*.