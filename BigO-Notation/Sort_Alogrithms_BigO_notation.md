# Sort algorithms with their Big O notations

1. `Bubble` Sort: **O(n^2)**

    Bubble sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted.

    ```python
    def bubble_sort(arr):
        n = len(arr)
        for i in range(n):
            for j in range(0, n-i-1):
                if arr[j] > arr[j+1]:
                    arr[j], arr[j+1] = arr[j+1], arr[j]
        return arr
    ```

2. `Selection` Sort: **O(n^2)**

    Selection sort is a simple sorting algorithm that repeatedly finds the minimum element from the unsorted part of the list and puts it at the beginning. The algorithm maintains two sublists in a given list.

    ```python
    def selection_sort(arr):
       n = len(arr)
       for i in range(n):
           min_idx = i
           for j in range(i+1, n):
               if arr[j] < arr[min_idx]:
                   min_idx = j
           arr[i], arr[min_idx] = arr[min_idx], arr[i]
       return arr
    ```

3. `Insertion` Sort: **O(n^2)**

    Insertion sort is a simple sorting algorithm that builds the final sorted list one item at a time. It is much less efficient on large lists than more advanced algorithms such as quicksort, heapsort, or merge sort.

    ```python
    def insertion_sort(arr):
       n = len(arr)
       for i in range(1, n):
           key = arr[i]
           j = i-1
           while j >= 0 and key < arr[j]:
               arr[j+1] = arr[j]
               j -= 1
           arr[j+1] = key
       return arr
    ```

4. `Merge` Sort: **O(n log n)**

    Merge sort is a divide-and-conquer algorithm that divides the input list into two halves, sorts each half recursively, and then merges the sorted halves back together.

    ```python
    def merge_sort(arr):
       if len(arr) > 1:
           mid = len(arr) // 2
           left_half = arr[:mid]
           right_half = arr[mid:]
           merge_sort(left_half)
           merge_sort(right_half)
           i = j = k = 0
           while i < len(left_half) and j < len(right_half):
               if left_half[i] < right_half[j]:
                   arr[k] = left_half[i]
                   i += 1
               else:
                   arr[k] = right_half[j]
                   j += 1
               k += 1
           while i < len(left_half):
               arr[k] = left_half[i]
               i += 1
               k += 1
           while j < len(right_half):
               arr[k] = right_half[j]
               j += 1
               k += 1
       return arr
    ```

5. `Quick` Sort: **O(n^2) `worst-case`, O(n log n) `average-case`**

    Quick sort is a divide-and-conquer algorithm that selects a pivot element and partitions the input list around the pivot, such that elements smaller than the pivot come before it and elements larger than the pivot come after it. The algorithm then recursively sorts the two partitions.

    ```python
    def quick_sort(arr):
       if len(arr) <= 1:
           return arr
       pivot = arr[len(arr)//2]
       left_half = [x for x in arr if x < pivot]
       middle = [x for x in arr if x == pivot]
       right_half = [x for x in arr if x > pivot]
       return quick_sort(left_half) + middle + quick_sort(right_half)
    ```
6. `Heap` Sort: **O(n log n)**

    Heap sort is a comparison-based sorting algorithm that uses a binary heap data structure to sort the input list. The algorithm first builds a heap from the input data, and then repeatedly extracts the maximum element from the heap and places it at the end of the sorted list.

    ```python
    def heapify(arr, n, i):
       largest = i
       left = 2 * i + 1
       right = 2 * i + 2
       if left < n and arr[i] < arr[left]:
           largest = left
       if right < n and arr[largest] < arr[right]:
           largest = right
       if largest != i:
           arr[i], arr[largest] = arr[largest], arr[i]
           heapify(arr, n, largest)

   def heap_sort(arr):
       n = len(arr)
       for i in range(n//2 - 1, -1, -1):
           heapify(arr, n, i)
       for i in range(n-1, 0, -1):
           arr[i], arr[0] = arr[0], arr[i]
           heapify(arr, i, 0)
       return arr
    ```


## Sort() and Sorted()

In Python, `sort()` and `sorted()` are both used to sort a list of elements, but they differ in how they work and what they return.

* `sort()` is a method that can be called on a list object. It list `in place`, meaning that it modifies the original list and `does not create a new one`. The `sort()` method returns None, so it cannot be assigned to a new variable. Here's an example:
    ```python
    arr = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
    arr.sort()
    print(arr)  # Output: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]
    ```
* `sorted()` is a built-in function that takes an iterable as input and returns `a new sorted list`. It does not modify the original list or any other iterable object. The sorted() function `can be assigned to a new variable`. Here's an example:

    ```python
    arr = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
    sorted_arr = sorted(arr)
    print(sorted_arr)  # Output: [1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]
    ```

>In summary, `sort()` sorts the list `in place` and returns None, while `sorted()` returns `a new sorted list` and `does not modify` the original list.

