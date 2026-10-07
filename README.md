    '''1. Seven Shortened Sorting Algorithms

    Pattern 1: Basic Iterative Sorts
    1. Bubble Sort (Adjacent Swaps)
    Bubble Sort compares adjacent element pairs and repeatedly swaps them if they are out of order.'''

    def bubble_sort(arr):
        n = len(arr)
        for i in range(n):
            for j in range(0, n - 1 - i):
                if arr[j] > arr[j + 1]:
                    arr[j], arr[j + 1] = arr[j + 1], arr[j]
        return arr

    '''Shell Sort
    '''

    def shell_sort(arr):
        n = len(arr)
        gap = n // 2 
        while gap > 0:
            for i in range(gap, n):
                j = i
                while j >= gap and arr[j - gap] > arr[j]:
                    arr[j - gap], arr[j] = arr[j], arr[j - gap]
            gap //= 2 
        return arr

    '''2. Selection Sort (Target Finding)
    Selection Sort identifies the minimum element from the unsorted region and swaps it to the current boundary index.'''

    def selection_sort(arr):
        n = len(arr)
        for i in range(n):
            min_i = i
            for j in range(i + 1, n):
                if arr[j] < arr[min_i]:
                    min_i = j
            arr[i], arr[min_i] = arr[min_i], arr[i]
        return arr

    '''3. Insertion Sort (Sublist Shifting)
    Insertion Sort inserts each element into its proper position within a sorted sublist by bubbling smaller items to the left.'''

    def insertion_sort(arr):
        for i in range(1, len(arr)):
            j = i
            while j > 0 and arr[j - 1] > arr[j]:
                arr[j - 1], arr[j] = arr[j], arr[j - 1]
                j -= 1
        return arr

    '''Pattern 2: Divide & Conquer Pair
    4. Quick Sort (Pivot Partitioning)
    Quick Sort chooses a pivot element and partitions smaller and larger elements using list comprehensions.'''

    def quick_sort(arr):
        if len(arr) <= 1:
            return arr
        pivot = arr[-1]
        left = [x for x in arr[:-1] if x <= pivot]
        right = [x for x in arr[:-1] if x > pivot]
        return quick_sort(left) + [pivot] + quick_sort(right)

    '''5. Merge Sort (Divide & Conquer)
    Merge Sort recursively divides the array down into single elements and merges the sorted sub-arrays back together.'''

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
        return res + left[i:] + right[j:]

'''Pattern 3: Specialized Data Structures & Place Values
6. Heap Sort (Max Heap Extraction)
Heap Sort builds a max heap starting from the last non-leaf parent node (n // 2 - 1),
then repeatedly swaps the root element at index 0 with the last active element and re-heapifies.
'''
def heap_sort(arr):
    n = len(arr)

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

    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(i, 0)

    return arr

    '''7. Radix Sort (Place Value Buckets)
    Radix Sort processes numbers digit by digit across individual place values using 10 digit buckets.'''

    def radix_sort(arr):
        if not arr: return arr
        max_val = max(arr)
        exp = 1
        while max_val // exp > 0:
            buckets = [[] for _ in range(10)]
            for num in arr:
                buckets[(num // exp) % 10].append(num)
            arr = [num for bucket in buckets for num in bucket]
            exp *= 10
        return arr

    '''2. Binary Search Tree (BST)
    The Binary Search Tree architecture organizes keys hierarchically with node links, supporting recursive insertion, iterative lookup, node deletion across zero/one/two children cases, and depth-first traversals (inorder, preorder, postorder).
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
