def log_and_print(msg):
    print(msg)
    with open('display.txt', 'a') as f:
        f.write(str(msg) + '\n')

'''1. Seven Shortened Sorting Algorithms

Pattern 1: Basic Iterative Sorts
1. Bubble Sort (Adjacent Swaps)
Bubble Sort compares adjacent element pairs and repeatedly swaps them if they are out of order.

How to use:
    arr = [64, 34, 25, 12, 22, 11, 90]
    sorted_arr = bubble_sort(arr)
'''

def bubble_sort(arr):
    n = len(arr)
    log_and_print(f"Bubble Sort start: {arr}")
    for i in range(n):
        for j in range(0, n - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
        log_and_print(f"Pass {i + 1}: {arr}")
    return arr

'''Shell Sort

How to use:
    arr = [12, 34, 54, 2, 3]
    sorted_arr = shell_sort(arr)
'''

def shell_sort(arr):
    n = len(arr)
    gap = n // 2 
    log_and_print(f"Shell Sort start: {arr}")
    step = 1
    while gap > 0:
        for i in range(gap, n):
            j = i
            while j >= gap and arr[j - gap] > arr[j]:
                arr[j - gap], arr[j] = arr[j], arr[j - gap]
                j -= gap
        log_and_print(f"Gap {gap} (step {step}): {arr}")
        gap //= 2
        step += 1
    return arr

'''2. Selection Sort (Target Finding)
Selection Sort identifies the minimum element from the unsorted region and swaps it to the current boundary index.

How to use:
    arr = [64, 25, 12, 22, 11]
    sorted_arr = selection_sort(arr)
'''

def selection_sort(arr):
    n = len(arr)
    log_and_print(f"Selection Sort start: {arr}")
    for i in range(n):
        min_i = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_i]:
                min_i = j
        arr[i], arr[min_i] = arr[min_i], arr[i]
        log_and_print(f"Pass {i + 1}: {arr}")
    return arr

'''3. Insertion Sort (Sublist Shifting)
Insertion Sort inserts each element into its proper position within a sorted sublist by bubbling smaller items to the left.

How to use:
    arr = [12, 11, 13, 5, 6]
    sorted_arr = insertion_sort(arr)
'''

def insertion_sort(arr):
    log_and_print(f"Insertion Sort start: {arr}")
    for i in range(1, len(arr)):
        j = i
        while j > 0 and arr[j - 1] > arr[j]:
            arr[j - 1], arr[j] = arr[j], arr[j - 1]
            j -= 1
        log_and_print(f"Pass {i}: {arr}")
    return arr

'''Pattern 2: Divide & Conquer Pair
4. Quick Sort (Pivot Partitioning)
Quick Sort chooses a pivot element and partitions smaller and larger elements using list comprehensions.

How to use:
    arr = [10, 7, 8, 9, 1, 5]
    sorted_arr = quick_sort(arr)
'''

def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[-1]
    left = [x for x in arr[:-1] if x <= pivot]
    right = [x for x in arr[:-1] if x > pivot]
    log_and_print(f"Pivot: {pivot}, Left: {left}, Right: {right}")
    res = quick_sort(left) + [pivot] + quick_sort(right)
    log_and_print(f"Combined: {res}")
    return res

'''5. Merge Sort (Divide & Conquer)
Merge Sort recursively divides the array down into single elements and merges the sorted sub-arrays back together.

How to use:
    arr = [12, 11, 13, 5, 6, 7]
    sorted_arr = merge_sort(arr)
'''

def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left, right = merge_sort(arr[:mid]), merge_sort(arr[mid:])
    
    res = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            res.append(left[i]); i += 1
        else:
            res.append(right[j]); j += 1
    res = res + left[i:] + right[j:]
    log_and_print(f"Merged: {res}")
    return res

'''Pattern 3: Specialized Data Structures & Place Values
6. Heap Sort (Max Heap Extraction)
Heap Sort builds a max heap starting from the last non-leaf parent node (n // 2 - 1),
then repeatedly swaps the root element at index 0 with the last active element and re-heapifies.

How to use:
    arr = [12, 11, 13, 5, 6, 7]
    sorted_arr = heap_sort(arr)
'''

def heap_sort(arr):
    n = len(arr)
    log_and_print(f"Heap Sort start: {arr}")

    def heapify(n, i):
        largest = i
        l, r = 2 * i + 1, 2 * i + 2
        if l < n and arr[l] > arr[largest]:
            largest = l
        if r < n and arr[r] > arr[largest]:
            largest = r
        if largest != i:
            arr[i], arr[largest] = arr[largest], arr[i]
            heapify(n, largest)

    for i in range(n // 2 - 1, -1, -1):
        heapify(n, i)
    log_and_print(f"Built max heap: {arr}")

    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(i, 0)
        log_and_print(f"Extracted to index {i}: {arr}")

    return arr

'''7. Radix Sort (Place Value Buckets)
Radix Sort processes numbers digit by digit across individual place values using 10 digit buckets.

How to use:
    arr = [170, 45, 75, 90, 802, 24, 2, 66]
    sorted_arr = radix_sort(arr)
'''

def radix_sort(arr):
    if not arr: return arr
    log_and_print(f"Radix Sort start: {arr}")
    max_val = max(arr)
    exp = 1
    while max_val // exp > 0:
        buckets = [[] for _ in range(10)]
        for num in arr:
            buckets[(num // exp) % 10].append(num)
        arr = [num for bucket in buckets for num in bucket]
        log_and_print(f"Exp {exp}: {arr}")
        exp *= 10
    return arr

'''2. Binary Search Tree (BST)
The Binary Search Tree architecture organizes keys hierarchically with node links, supporting recursive insertion, iterative lookup, node deletion across zero/one/two children cases, and depth-first traversals (inorder, preorder, postorder).

How to use:
    tree = BST()
    
    # Insert values
    tree.insert(50)
    tree.insert(30)
    tree.insert(70)
    tree.insert(20)
    
    # Search for a value (returns True/False)
    tree.search(30)
    tree.search(99)
    
    # Delete a value
    tree.delete(20)
    
    # Traversals (return list of values)
    tree.inorder()
    tree.preorder()
    tree.postorder()
'''

class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None

    # --- INSERT ---
    def insert(self, value):
        def _insert(node):
            if not node:
                return Node(value)
            if value < node.value:
                node.left = _insert(node.left)
            elif value > node.value:
                node.right = _insert(node.right)
            return node

        self.root = _insert(self.root)

    # --- SEARCH (Iterative is easiest to recall) ---
    def search(self, value):
        current = self.root
        while current:
            if value == current.value:
                return True
            current = current.left if value < current.value else current.right
        return False

    # --- DELETE ---
    def delete(self, value):
        def _delete(node, value):
            if not node:
                return None

            # 1. Traverse to the node
            if value < node.value:
                node.left = _delete(node.left, value)
            elif value > node.value:
                node.right = _delete(node.right, value)
            else:
                # 2. Case A & B: Zero or one child
                if not node.left:
                    return node.right
                if not node.right:
                    return node.left

                # 3. Case C: Two children (find smallest in right subtree)
                successor = node.right
                while successor.left:
                    successor = successor.left

                node.value = successor.value
                node.right = _delete(node.right, successor.value)

            return node

        self.root = _delete(self.root, value)

    # --- TRAVERSALS (Recursive concatenation) ---
    def inorder(self, node=None, start=True):
        if start:
            node = self.root
        if not node:
            return []
        return (
            self.inorder(node.left, False)
            + [node.value]
            + self.inorder(node.right, False)
        )

    def preorder(self, node=None, start=True):
        if start:
            node = self.root
        if not node:
            return []
        return (
            [node.value]
            + self.preorder(node.left, False)
            + self.preorder(node.right, False)
        )

    def postorder(self, node=None, start=True):
        if start:
            node = self.root
        if not node:
            return []
        return (
            self.postorder(node.left, False)
            + self.postorder(node.right, False)
            + [node.value]
        )

