def log_and_print(msg):

    print(msg)

    with open('display.txt', 'a') as f:

        f.write(str(msg) + '\n')


def bubble_sort(arr):

    # To sort descending: change 'arr[j] > arr[j + 1]' to 'arr[j] < arr[j + 1]'

    n = len(arr)

    log_and_print(f"Bubble Sort start: {arr}")

    step = 1

    for i in range(n):

        swapped = False

        for j in range(0, n - 1 - i):

            if arr[j] > arr[j + 1]:

                arr[j], arr[j + 1] = arr[j + 1], arr[j]

                log_and_print(f"  Step {step} (Swapped {arr[j + 1]} and {arr[j]}): {arr}")

                step += 1

                swapped = True

        log_and_print(f"Pass {i + 1}: {arr}")

        if not swapped:

            break

    return arr


def shell_sort(arr):

    # To sort descending: change 'arr[j - gap] > arr[j]' to 'arr[j - gap] < arr[j]'

    n = len(arr)

    gap = n // 2

    log_and_print(f"Shell Sort start: {arr}")

    step = 1

    gap_pass = 1

    while gap > 0:

        for i in range(gap, n):

            j = i

            while j >= gap and arr[j - gap] > arr[j]:

                arr[j - gap], arr[j] = arr[j], arr[j - gap]

                log_and_print(f"  Step {step} (Shifted {arr[j]} with {arr[j - gap]}, gap={gap}): {arr}")

                step += 1

                j -= gap

        log_and_print(f"Gap {gap} (Pass {gap_pass}) completed: {arr}")

        gap //= 2

        gap_pass += 1

    return arr


def selection_sort(arr):

    # To sort descending: change 'arr[j] < arr[min_i]' to 'arr[j] > arr[min_i]'

    n = len(arr)

    log_and_print(f"Selection Sort start: {arr}")

    step = 1

    for i in range(n):

        min_i = i

        for j in range(i + 1, n):

            if arr[j] < arr[min_i]:

                min_i = j

        if min_i != i:

            arr[i], arr[min_i] = arr[min_i], arr[i]

            log_and_print(f"  Step {step} (Swapped min {arr[i]} at index {min_i} to index {i}): {arr}")

        else:

            log_and_print(f"  Step {step} (Index {i} already contains min {arr[i]}): {arr}")

        step += 1

        log_and_print(f"Pass {i + 1}: {arr}")

    return arr


def insertion_sort(arr):

    # To sort descending: change 'arr[j - 1] > arr[j]' to 'arr[j - 1] < arr[j]'

    log_and_print(f"Insertion Sort start: {arr}")

    step = 1

    for i in range(1, len(arr)):

        j = i

        while j > 0 and arr[j - 1] > arr[j]:

            arr[j - 1], arr[j] = arr[j], arr[j - 1]

            log_and_print(f"  Step {step} (Shifted {arr[j]} and {arr[j - 1]}): {arr}")

            step += 1

            j -= 1

        log_and_print(f"Pass {i}: {arr}")

    return arr


def quick_sort(arr):

    # To sort descending: combine as 'quick_sort(right) + [pivot] + quick_sort(left)'

    if len(arr) <= 1:

        return arr

    pivot = arr[-1]

    left = [x for x in arr[:-1] if x <= pivot]

    right = [x for x in arr[:-1] if x > pivot]

    log_and_print(f"  Step (Partition on pivot {pivot}): Left={left}, Right={right}")

    res = quick_sort(left) + [pivot] + quick_sort(right)

    log_and_print(f"  Step (Combined with pivot {pivot}): {res}")

    return res


def merge_sort(arr):

    # To sort descending: change 'left[i] <= right[j]' to 'left[i] >= right[j]'

    if len(arr) <= 1:

        return arr

    mid = len(arr) // 2

    log_and_print(f"  Step (Split): Left={arr[:mid]}, Right={arr[mid:]}")

    left = merge_sort(arr[:mid])

    right = merge_sort(arr[mid:])

    res = []

    i = 0

    j = 0

    while i < len(left) and j < len(right):

        if left[i] <= right[j]:

            res.append(left[i])

            i += 1

        else:

            res.append(right[j])

            j += 1

        log_and_print(f"  Step (Merge append): Current={res}")

    res = res + left[i:] + right[j:]

    log_and_print(f"Merged segment: {res}")

    return res


def heap_sort(arr):

    # To sort descending: change '>' to '<' in heapify comparisons to build a min-heap instead of a max-heap

    n = len(arr)

    log_and_print(f"Heap Sort start: {arr}")

    step = [1]

    def heapify(n, i):

        largest = i

        l = 2 * i + 1

        r = 2 * i + 2

        if l < n and arr[l] > arr[largest]:

            largest = l

        if r < n and arr[r] > arr[largest]:

            largest = r

        if largest != i:

            arr[i], arr[largest] = arr[largest], arr[i]

            log_and_print(f"  Step {step[0]} (Heapify swap index {i} and {largest}): {arr}")

            step[0] += 1

            heapify(n, largest)

    for i in range(n // 2 - 1, -1, -1):

        heapify(n, i)

    log_and_print(f"Built max heap: {arr}")

    for i in range(n - 1, 0, -1):

        arr[0], arr[i] = arr[i], arr[0]

        log_and_print(f"  Step {step[0]} (Swapped root {arr[i]} to index {i}): {arr}")

        step[0] += 1

        heapify(i, 0)

        log_and_print(f"Extracted to index {i}: {arr}")

    return arr


def radix_sort(arr):

    # To sort descending: collect buckets in reverse order using 'reversed(buckets)'

    if not arr:

        return arr

    log_and_print(f"Radix Sort start: {arr}")

    max_val = max(arr)

    exp = 1

    step = 1

    while max_val // exp > 0:

        buckets = [[] for _ in range(10)]

        for num in arr:

            bucket_idx = (num // exp) % 10

            buckets[bucket_idx].append(num)

        log_and_print(f"  Step {step} (Distribute exp {exp} into buckets): {buckets}")

        arr = [num for bucket in buckets for num in bucket]

        log_and_print(f"  Step {step} (Collected for exp {exp}): {arr}")

        step += 1

        exp *= 10

    return arr


class Node:

    def __init__(self, value):

        self.value = value

        self.left = None

        self.right = None


class BST:

    def __init__(self):

        self.root = None

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

    def search(self, value):

        current = self.root

        while current:

            if value == current.value:

                return True

            if value < current.value:

                current = current.left

            else:

                current = current.right

        return False

    def delete(self, value):

        def _delete(node, value):

            if not node:

                return None

            if value < node.value:

                node.left = _delete(node.left, value)

            elif value > node.value:

                node.right = _delete(node.right, value)

            else:

                if not node.left:

                    return node.right

                if not node.right:

                    return node.left

                successor = node.right

                while successor.left:

                    successor = successor.left

                node.value = successor.value

                node.right = _delete(node.right, successor.value)

            return node

        self.root = _delete(self.root, value)

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

    def levelorder(self):

        if not self.root:

            return []

        result = []

        queue = [self.root]

        while queue:

            current = queue.pop(0)

            result.append(current.value)

            if current.left:

                queue.append(current.left)

            if current.right:

                queue.append(current.right)

        return result
