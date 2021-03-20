Individual Project
==================

In [1]:

    from sys import stdout
    from copy import deepcopy
    from random import sample, randint
    %load_ext memory_profiler
    %load_ext watermark
    %watermark -v -iv

    Python implementation: CPython
    Python version       : 3.8.8
    IPython version      : 7.21.0

### K & Array Generation

In [2]:

    # Returning arrays of length n containing unique integers (0 - n-1), which are randomly ordered
    def randomize(hundred, thous, hunderedThous, mill, tenMill):
        hundred = sample(range(0, 100), 100)
        thous = sample(range(0, 1000), 1000)
        hunderedThous = sample(range(0, 100000), 100000)
        mill = sample(range(0, 1000000), 1000000)
        tenMill = sample(range(0, 10000000), 10000000)
        return hundred, thous, hunderedThous, mill, tenMill


    hundred, thous, hunderedThous, mill, tenMill = [], [], [], [], []
    # Random arrays saved so every solution uses same set of arrays
    arrays = [hundred, thous, hunderedThous, mill, tenMill]
    randArrays = randomize(*arrays)
    hundred, thous, hunderedThous, mill, tenMill = deepcopy(randArrays)
    # Arbitrary k value within all arrays (0 <= k <= 99)
    k = randint(0, 99)

### Solutions

In [3]:

    def hoarsPartition(array, lower, upper):
        pivot = array[lower]
        i = lower - 1
        j = upper + 1

        while True:
            j -= 1
            while array[j] > pivot:
                j -= 1

            i += 1
            while array[i] < pivot:
                i += 1

            if i < j:
                array[i], array[j] = array[j], array[i]
            else:
                return j


    def quickSort(array, lower, upper):
        if lower < upper:
            pivot = hoarsPartition(array, lower, upper)

            quickSort(array, lower, pivot)
            quickSort(array, pivot + 1, upper)


    def naiveSolution(array, k):
        quickSort(array, 0, len(array) - 1)
        stdout.write("%dth smallest element in array is %d\n" % (k, array[k - 1]))

In [4]:

    %%timeit -r 1 -n 1
    naiveSolution(hundred, k)

    58th smallest element in array is 57
    389 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [5]:

    %%timeit -r 1 -n 1
    naiveSolution(thous, k)

    58th smallest element in array is 57
    1.36 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [6]:

    %%timeit -r 1 -n 1
    naiveSolution(hunderedThous, k)

    58th smallest element in array is 57
    174 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [7]:

    %%timeit -r 1 -n 1
    naiveSolution(mill, k)

    58th smallest element in array is 57
    2.26 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [8]:

    %%timeit -r 1 -n 1
    naiveSolution(tenMill, k)

    58th smallest element in array is 57
    28.9 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [9]:

    hundred, thous, hunderedThous, mill, tenMill = deepcopy(randArrays)

In [10]:

    def quickSelect(array, lower, upper, k):
        if lower == upper:
            return array[lower]

        pivotIndex = randint(lower, upper)
        pivotValue = array[pivotIndex]
        array[upper], array[pivotIndex] = array[pivotIndex], array[upper]
        pivotIndex = lower

        for index in range(lower, upper):
            if array[index] <= pivotValue:
                array[index], array[pivotIndex] = array[pivotIndex], array[index]
                pivotIndex = pivotIndex + 1

        array[upper], array[pivotIndex] = array[pivotIndex], array[upper]

        if k < pivotIndex:
            return quickSelect(array, lower, pivotIndex - 1, k)
        elif k > pivotIndex:
            return quickSelect(array, pivotIndex + 1, upper, k)
        else:
            return array[k]


    def smarterSolution(array, k):
        stdout.write("%dth smallest element in array is %d\n" % (k, quickSelect(array, 0, len(array) - 1, k) - 1))

In [11]:

    %%timeit -r 1 -n 1
    smarterSolution(hundred, k)

    58th smallest element in array is 57
    144 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [12]:

    %%timeit -r 1 -n 1
    smarterSolution(thous, k)

    58th smallest element in array is 57
    216 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [13]:

    %%timeit -r 1 -n 1
    smarterSolution(hunderedThous, k)

    58th smallest element in array is 57
    11.1 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [14]:

    %%timeit -r 1 -n 1
    smarterSolution(mill, k)

    58th smallest element in array is 57
    242 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [15]:

    %%timeit -r 1 -n 1
    smarterSolution(tenMill, k)

    58th smallest element in array is 57
    4.89 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [16]:

    hundred, thous, hunderedThous, mill, tenMill = deepcopy(randArrays)

### Algorithms for Runtime Comparison

In [17]:

    def bubbleSort(array):
        for x in range(len(array)):
            for y in range(len(array) - 1):
                if array[y] > array[y + 1]:
                    array[y], array[y + 1] = array[y + 1], array[y]

    def upperBound(array, k):
        bubbleSort(array)
        stdout.write("%dth smallest element in array is %d\n" %(k, array[k - 1]))

In [18]:

    %%timeit -r 1 -n 1
    upperBound(hundred, k)

    58th smallest element in array is 57
    706 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [19]:

    %%timeit -r 1 -n 1
    upperBound(thous, k)

    58th smallest element in array is 57
    72.4 ms ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [20]:

    %%timeit -r 1 -n 1
    upperBound(hunderedThous, k)

    58th smallest element in array is 57
    13min 57s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [21]:

    def binarySearch(array, lower, upper, k):
        if lower <= upper:
            middle = (lower + upper) // 2
            if array[middle] == k:
                return middle
            if array[middle] > k:
                return binarySearch(array, lower, middle - 1, k)
            if array[middle] < k:
                return binarySearch(array, middle + 1, upper, k)


    def lowerBound(array, k):
        stdout.write("%dth smallest element in array is %d\n" % (k, binarySearch(array, 0, len(array) - 1, k)-1))

In [22]:

    %%timeit -r 1 -n 1
    lowerBound(hundred, k)

    58th smallest element in array is 57
    28.6 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [23]:

    %%timeit -r 1 -n 1
    lowerBound(thous, k)

    58th smallest element in array is 57
    99.5 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

In [24]:

    %%timeit -r 1 -n 1
    lowerBound(hunderedThous, k)

    58th smallest element in array is 57
    106 µs ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)

### Examples

In [25]:

    # Array content
    ten = sample(range(0, 10), 10)
    stdout.write(" ".join(str(x) for x in ten))
    bubbleSort(ten)
    stdout.write("\n" + " ".join(str(x) for x in ten))

    7 8 4 1 5 3 9 0 2 6
    0 1 2 3 4 5 6 7 8 9
