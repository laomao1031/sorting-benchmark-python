# sorting-benchmark-python

import random
import time

# 1. 自實作 Counting Sort
def counting_sort(arr):
    if not arr: return arr
    max_val = max(arr)
    min_val = min(arr)
    range_k = max_val - min_val + 1
    # 分析工具：Counting Sort 的鍵值假設 (Key Range)
    counts = [0] * range_k
    for x in arr:
        counts[x - min_val] += 1
    
    sorted_arr = []
    for i, count in enumerate(counts):
        sorted_arr.extend([i + min_val] * count)
    return sorted_arr

# 2. 自實作 Radix Sort (LSD)
def radix_sort(arr):
    if not arr: return arr
    # 分析工具：Radix Sort 的 LSD 邏輯
    max_val = max(arr)
    exp = 1
    while max_val // exp > 0:
        # 每一輪使用 Counting Sort 作為穩定排序基礎
        buckets = [[] for _ in range(10)]
        for x in arr:
            index = (x // exp) % 10
            buckets[index].append(x)
        arr = [x for bucket in buckets for x in bucket]
        exp *= 10
    return arr

# 實驗設定
n = 1_000_000 # 1M 筆資料
# 16-bit unsigned integer 範圍為 [0, 65535]
data = [random.randint(0, 65535) for _ in range(n)]

# 量測開始
results = {}

for name, func in [("sorted()", sorted), ("Counting Sort", counting_sort), ("Radix Sort", radix_sort)]:
    test_data = data.copy()
    start = time.time()
    func(test_data)
    results[name] = time.time() - start
    print(f"{name:15}: {results[name]:.4f} seconds")
