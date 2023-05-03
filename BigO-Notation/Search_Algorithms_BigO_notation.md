# Search algorithms with their Big O notations

1. `Linear Search` - **O(n)**

    Linear search is a simple search algorithm that iterates through a list of elements to find a target value. Its time complexity is O(n), where n is the number of elements in the list.

    ```python
    def linear_search(arr, target):
        for i, value in enumerate(arr):
            if value == target:
                return i
        return -1

    arr = [3, 5, 1, 7, 9]
    target = 7
    result = linear_search(arr, target)
    print(result)  # Output: 3
    ```
2. `Binary` Search - **O(log n)**

    Binary search is an efficient search algorithm that works on `sorted lists`. It repeatedly divides the search interval in half until the target value is found or the interval is empty. Its time complexity is O(log n), where n is the number of elements in the list.
    
    >Attention: Works on Sorted list

    ```python
    def binary_search(arr, target):
        left, right = 0, len(arr) - 1

        while left <= right:
            mid = (left + right) // 2
            mid_value = arr[mid]

            if mid_value == target:
                return mid
            elif mid_value < target:
                left = mid + 1
            else:
                right = mid - 1

        return -1

    arr = [1, 3, 5, 7, 9]
    target = 7
    result = binary_search(arr, target)
    print(result)  # Output: 3
    ```

3. `Jump` Search - **O(√n)**

    Jump search is a search algorithm that works on `sorted lists`. It checks fewer elements by jumping ahead a fixed number of steps, then performs a linear search within the smaller subarray. Its time complexity is O(√n), where n is the number of elements in the list.

    >Attention: Works on Sorted list

    ```python
    import math

    def jump_search(arr, target):
        n = len(arr)
        jump = int(math.sqrt(n))

        left, right = 0, 0
        while right < n and arr[right] < target:
            left = right
            right += jump

        for i in range(left, min(right, n)):
            if arr[i] == target:
                return i

        return -1

    arr = [1, 3, 5, 7, 9]
    target = 7
    result = jump_search(arr, target)
    print(result)  # Output: 3
    ```
4. `Exponential` Search - **O(log n)**

     Exponential search is a search algorithm that works by repeatedly doubling the search interval until the desired element is either found or passed, and then performing a binary search in the smaller interval.

     ```python
    def exponential_search(arr, x):
        n = len(arr)
        if arr[0] == x:
            return 0
        i = 1
        while i < n and arr[i] <= x:
            i *= 2
        return binary_search(arr, x, i // 2, min(i, n - 1))

    def binary_search(arr, x, low, high):
        while low <= high:
            mid = (low + high) // 2
            if arr[mid] == x:
                return mid
            elif arr[mid] < x:
                low = mid + 1
            else:
                high = mid - 1
        return -1

    arr = [1, 2, 3, 4, 5]
    x = 4
    result = exponential_search(arr, x)
    print(result) # Output: 3
    ```

5. `Ternary` Search - O(log3 n)

     Ternary search is a search algorithm that works by dividing the search interval into three parts and discarding one part based on the value of the desired element relative to the values at the two dividing points.

    ```python
    def ternary_search(arr, x):
        left = 0
        right = len(arr) - 1
        while left <= right:
            mid1 = left + (right - left) // 3
            mid2 = right - (right - left) // 3
            if arr[mid1] == x:
                return mid1
            elif arr[mid2] == x:
                return mid2
            elif x < arr[mid1]:
                right = mid1 - 1
            elif x > arr[mid2]:
                left = mid2 + 1
            else:
                left = mid1 + 1
                right = mid2 - 1
        return -1

    arr = [1, 2, 3, 4, 5]
    x = 4
    result = ternary_search(arr, x)
    print(result) # Output: 3
    ```

6. `Fibonacci` Search - O(log n)

    Fibonacci search is a search algorithm that works by dividing the search interval into two parts using Fibonacci numbers, and discarding one part based on the value of the desired element relative to the value at the dividing point.

    ```python
    def fibonacci_search(arr, x):
        n = len(arr)
        fib2 = 0
        fib1 = 1
        fib = fib1 + fib2
        while fib < n:
            fib2 = fib1
            fib1 = fib
            fib = fib1 + fib2
        offset = -1
        while fib > 1:
            i = min(offset + fib2, n - 1)
            if arr[i] < x:
                fib = fib1
                fib1 = fib2
                fib2 = fib - fib1
                offset = i
            elif arr[i] > x:
                fib = fib2
                fib1 = fib1 - fib2
                fib2 = fib - fib1
            else:
                return i
        if fib1 and arr[offset + 1] == x:
            return offset + 1
        return -1

    arr = [1, 2, 3, 4, 5]
    x = 4
    result = fibonacci_search(arr, x)
    print(result) # Output: 3
    ```

7. Hash Search - O(1) (average case)

    Hash search is a search algorithm that works by storing the elements in a hash table and then looking up the desired element in constant time.

    ```python
    def hash_search(arr, x):
        hash_table = {}
        for i in range(len(arr)):
            hash_table[arr[i]] = i
        if x in hash_table:
            return hash_table[x]
        return -1

    arr = [1, 2, 3, 4, 5]
    x = 4
    result = hash_search(arr, x)
    print(result) # Output: 3
    ```

    >Note that hash search has an average case time complexity of O(1), but a worst case time complexity of O(n) if there are many hash collisions.
