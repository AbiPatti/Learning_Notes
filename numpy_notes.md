# NumPy Notes

## Basics
- `import numpy as np`
- Each array has **one** data type (`dtype`)
- Better for math than Python lists

## Creating Arrays
- `np.array([1,2,3])`
- `np.array(python_list)`
- `np.zeros((2,3))` - 2x3 array of zeros
- `np.ones((3,3))` - 3x3 array of ones
- `np.arange(0,10,2)` - [0 2 4 6 8]
- `np.linspace(0,1,5)` - 5 numbers between 0–1
- `np.random.random(2,2)` - 2x2 array of random floats 0-1

## Info
- `arr.shape` - rows, cols
- `arr.ndim` - number of dimensions  
- `arr.size` - total elements  
- `arr.dtype` - data type

## Indexing & Slicing
- Slices give views, not copies - modifying a slice affects the original array
- `arr[0]` - first element  
- `arr[-1]` - last element  
- `arr[1:4]` - elements 1,2,3
- `arr2d[3:6, 3:6]` - Slicing in 2D
- `arr2d[0,1]` - row 0, column 1
- `arr2d[:,0]` - all rows, first column

## Math & Operations & Broadcasting
- Works elementwise (applied to each corresponding element)
- `arr + 5`, `arr * 2`, `arr ** 2` - applies to each element
- `np.sqrt(arr)` - square root
- `np.mean(arr)` - average
- **Vectorization** - apply calculations/ functions directly to arrays without loops (`arr * 5` multiplies every element by 5, etc.)
- `np.vectorize(function)` - make a Python function vectorized to use with NumPy arrays (such as `len()` but without parentheses)
- **Broadcasting**:
-   compare array **shapes** from **right to left** - each dimension must be equal or 1 to be broadcasted
-   useful for applying operations to arrays of different shapes (dimensions must still be compatible), like when adding a vector to each row or column

## Statistics & Aggregation
- `arr.sum()`, `arr.min()`, `arr.max()`, `arr.mean()`, etc.
- `arr.mean(axis=0)` - column or `arr.mean(axis=1)` - by row
- `np.median(arr)` - middle value
- `np.std(arr)` - standard deviation (how spread out numbers are - low means data is close to mean, high means data is spread out)
- `np.prod(arr)` - product of all elements (multiplies all elements)
- `np.cumsum(arr)` - cumulative sum (returns an array where each element is the running sum until that point - np.cumsum([1,2,3]) -> array([1,3,6]))
- `keepdims=True` - **parameter** to keep original array's dimensions

## Filtering
- `mask = arr > 3` - condition for mask giving true/false values
- `arr[mask]` - only **elements** where condiiton is true (greater than 3)
- `np.where(arr > 3)` - only **indices** where condition is true (greater than 3)

## Reshape & Stack
- `arr.reshape((2,3))` - change shape without changing data
- `arr.flatten()` - make 1D
- `np.concatenate((arr1,arr2), axis=0)` - join arrays along an axis
- `np.vstack((a,b))` - stack vertically
- `np.hstack((a,b))` - stack horizontally

## Modifying
- `np.delete(arr, index, axis)` - Remove an element at given index, axis=0 to remove row or axis=1 to remove column in 2d array

## Image Analogy
- Image = 3D array (height, width, 3)
- Each pixel = (R,G,B) values from 0–255
- `gray = img.mean(axis=2).astype(np.uint8)`
- `inverted = 255 - img`
