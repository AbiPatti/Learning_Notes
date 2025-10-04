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

## Math & Operations
- Works elementwise (applied to each corresponding element)
- `arr + 5`, `arr * 2`, `arr ** 2` - applies to each element
- `np.sqrt(arr)` - square root
- `np.mean(arr)` - average
- Broadcasting lets small arrays expand to match bigger ones

## Statistics
- `arr.sum()`, `arr.min()`, `arr.max()`
- `arr.mean(axis=0)` - mean by column
- `arr.mean(axis=1)` - mean by row

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

## Image Analogy
- Image = 3D array (height, width, 3)
- Each pixel = (R,G,B) values from 0–255
- `gray = img.mean(axis=2).astype(np.uint8)`
- `inverted = 255 - img`