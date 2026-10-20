\<.> -> Required Argument(Positional)
\[.] -> Optional Argument(Keyword)
### Difference between NumPy and Lists:
 - NumPy is Faster than Lists
	 - value vs size, reference count, object type, object value
 - Faster to read less bytes of memory 
 - No type checking when iterating through objects
 - NumPy/Arrays utilize "Contiguous Memory"
	 - **SIMD Vector Processing**
	 - Effective Cache Utilization

### Applications of NumPy:
- Mathematics(MATLAB Replacement)
- Plotting (Matplotlib)
- Backend(Pandas)
- Machine Learning

### Basics Of NumPy:
```Python
#Import
import numpy as np
a = np.array([1,2,3], dtype='int32')
print(a)
#Auto Detects dtype, Auto detects dimension of array(2)
b = np.array([[9.0,8.0,7.0],[6.0,5.0,4.0]])
print(b)
#O/P
#[[9. 8. 7.]
#[6. 5. 4.]]
#Common Atrributes:
a.ndim
b.shape # return order of array(2x3)
a.dtype
a.itemsize #size of dtype
a.size #len of array 'a'
a.nbytes

#Accessing/Changing elements, rows, columns:
a[<row>,<col>] #General Case
a[<row>, <start>:<end>] #end -> exclusive
a[<start>:<end>:<step>,<col>]
	 
#Assignment
a[<row>,<col>] = <num: dtype> 
a[<start>:<end>:<step>:<col>] = <iterable of same order>
#Initialization of Different Arrays:
np.zeros((2,2)) #row first implementation
np.ones(<shape: iterable>)
np.full(<shape>, num) #makes an array full of specified num value
#weirdly the dtype will be float instead of int(even if num: int)

#Both of these are equivalent(although there is some dtype discrepency)
np.full_like(<iterable>, num)
#weirdly this dtype will whatever dtype num is
np.full(<iterable>.shape, num)

np.random.rand(<rowSize: int>,<colSize: int>) #array of random numbers b/w 0 and 1
np.random.random_sample(<iterable>.shape)
np.random.randint([startValue: int],[stopValue: int], <size>) #stopValue exclusive

np.identity(<size: int>) #square identity matrix of order sizexsize will be created
np.repeat(<arr: npArray>, <repetition: int>, [axis = int]) """not sure.."""

```

```Python
a = np.array([1,2,3])
b = a
b[0] = 100
print(a) # will print [100 2 3]

#To solve #ShallowCopyDeepCopyBS, use:
b = a.copy() 
```

### Element-Wise Operations:
```Python
a = np.array(<List(dtype)>)

a + 2 #adds 2 to each element
a - var #similarily subtracts
a * var #similarily multiplies
a / var #similarily divides
a ** var #dtype increases?

b = np.array([1,0,1,0])
a + b #will add correspoing element of b to corresponding element of a...

np.sin(a) #outputs sin of all values
```

#### Linear Algebra:
```Python
"""Consider 2 arrays a and b which are matrix multipliable"""
np.matmul(a,b) #returns multiplied matrix with correct order
#If a and b were not matrix multipliable, then prompts ValueError

np.linalg.det(<arr: npArray>)# return determinant as a float
#...
```

#### Statistics:
```Python
np.min(<arr: npArray>, [axis = int])
np.max(<arr: npArray>, [axis = int])
np.sum(<arr: npArray>, [axis = int])
```

### Reshaping Arrays: 
```Python
arr1 = <arr2: npArray>.reshape(<size: List>) #size must be consistent with arr2 size
#Example:
before = np.array([[1,2,3,4],[5,6,7,8]])
print(before)

after = before.reshape((2,3))
print(after)

"""Vertically stacking vectors"""
v1 = np.array([1,2,3,4])
v2 = np.array([5,6,7,8])

np.vstack([v1,v2,v1,v2])
#similarily horizontal stack exists
```

## Miscellaneous:
pass