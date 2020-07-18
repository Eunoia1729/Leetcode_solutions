# Overview
I would recommend readers to solve [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) before trying this since XOR is a commutative operation similar to addition.
The problem is easy once you realise precomputation of prefixes can be used and the fact that similar to subtraction being the inverse operation for addition, XOR is the inverse operation for XOR.

        
Let ***range_xor(L, R)*** denote the XOR of elements from indices L to R in the given array and `^` denote the XOR operation.

# Theory
1. XOR of any number, *A* with itself is 0. 
   >     A ^ A = 0
2. XOR of any number, *A* with 0 is the number itself. 
   >     A ^ 0 = A

# Approach (Caching)
## Intuition:

As a naive approach, we can use a for loop to XOR each element in the range [L<sub>*i*</sub>, R<sub>*i*</sub>] for each query.
If the number of queries are large, for an arbitary range [l, r] we might have computed *range_xor(l, r)* more than once. Thus, there are repeated subtasks and caching might help you cut-off repeated same computations for the range queries.
Here, the precomputation/caching is storing the prefix XORs.

The meaning of prefix XOR is easy to understand. `prefix_xor[i]` is the XOR of `arr[0..i]` or in other words, *range_xor(0, i)*.

*prefix_xor* can be precomputed as follows:    
1. As there is only `arr[0]` in the range [0,0]:    
>       range_xor(0,0) = arr[0]    
2. For i > 0:          
>       range_xor(0, i) = arr[i] ^ range_xor(0, i - 1)   
    
The same is implemented in C++ as follows:
```C++       
prefix_xor[0] = arr[0];
for(int i = 1; i < arr.size(); ++i)
    prefix_xor[i] = arr[i] ^ prefix_xor[i - 1];   
```    
Mathematically,    

![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%28L_i%2C%20R_i%29)   
![equation](https://latex.codecogs.com/png.latex?%5Ctextup%7BXOR%20both%20sides%20with%20%7D%20range%5C_xor%280%2C%20L_i%20-%201%29%3A)   
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20R_i%29%20%3D%20range%5C_xor%28L_i%2C%20R_i%29)    
![equation](https://latex.codecogs.com/png.latex?%5Ctextup%7BAfter%20re-arranging%2C%20we%20get%3A%7D)   
![equation](https://latex.codecogs.com/png.latex?range%5C_xor%28L_i%2C%20R_i%29%20%3D%20range%5C_xor%280%2C%20L_i%20-%201%29%20%5Coplus%20range%5C_xor%280%2C%20R_i%29)         

Thus, the precomputed XOR's can be used to obtain *range_xor(L<sub>*i*</sub>, R<sub>*i*</sub>)* using the above formula. 

The same is implemented in C++ as follows:
```C++
query_res = prefix_xor[left - 1] ^ prefix_xor[right];
```
## Code
```C++   
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
## Complexity Analysis   
### Time complexity
![equation](https://latex.codecogs.com/gif.latex?O%28n%20&plus;%20m%29)    
where n is the number of elements, and m is the number of queries.   
Precomputation takes n units of time as a for-loop is run over the whole array. For each query, it takes constant time to get the result. 
Thus, it takes n units for caching and m units for queries.

### Space complexity    
![equation](https://latex.codecogs.com/gif.latex?O%28n%29)    
The linear extra space is required to store the precomputation array, `prefix_xor`.


### Bonus
Extra space can be made contsant by computing the prefix XOR in-place on the array, `arr`.
The same is implemented in C++ as follows:
```C++
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        
        for(int i = 1; i < arr.size(); ++i)
            arr[i] = arr[i] ^ arr[i - 1];
        
        vector<int> res;
        int left, right;
        for( auto query: queries)
        {
            left = query[0], right = query[1];
            if( left == 0)
                res.push_back( arr[right] );
            else
                res.push_back( arr[left - 1] ^ arr[right] );
        }
        
        return res;
    }
};
```
