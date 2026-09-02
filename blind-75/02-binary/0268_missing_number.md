# #0268 - Missing Number

- **LeetCode Link**: [268. Missing Number](https://leetcode.com/problems/missing-number/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Binary, Bit Manipulation (XOR), Math (Gauss Sum), Sorting

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho mảng `nums` chứa $n$ số nguyên phân biệt trong khoảng $[0, n]$. Tìm và trả về **con số duy nhất bị thiếu**.
- **Ví dụ**: `nums = [3, 0, 1]` ($n = 3$, dải số $[0, 3]$) $\rightarrow$ Output: `2`.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Sắp xếp (Sorting)
- **Ý tưởng**: Sort mảng tăng dần rồi duyệt kiểm tra xem `nums[i] + 1 != nums[i + 1]`. Xử lý thêm biên thiếu số `0` hoặc số `n`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \log N)$ — do thao tác sort.
  - **Space Complexity**: $O(1)$ hoặc $O(N)$ tùy thuật toán sort.

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        nums.sort()
        for i in range(len(nums) - 1):
            if nums[i] + 1 != nums[i + 1]:
                return nums[i] + 1
        if nums[0] != 0:
            return 0
        return nums[-1] + 1
```

---

### 🔹 Cách 2: Tổng Gauss - Toán học (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Tổng lý thuyết của các số từ $0$ đến $n$ là: $\text{expected\_sum} = \frac{n(n + 1)}{2}$.
  - Con số bị thiếu chính là hiệu: $\text{expected\_sum} - \sum(\text{nums})$.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — tính tổng mảng trong 1 lần duyệt.
  - **Space Complexity**: $O(1)$ — chỉ dùng 1 biến số nguyên.

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        expected_sum = n * (n + 1) // 2
        return expected_sum - sum(nums)
```

---

### 🔹 Cách 3: Thao tác Bit - XOR (Optimal & Tránh tràn số)
- **Ý tưởng**:
  - Tính chất của phép XOR: $a \oplus a = 0$ và $a \oplus 0 = a$.
  - Nếu XOR tất cả các chỉ số $0 \dots n$ với tất cả các giá trị trong `nums`:
    - Các số xuất hiện ở cả 2 bên sẽ tự triệt tiêu thành 0.
    - Con số duy nhất còn lại chính là con số bị thiếu.
  - **Ưu điểm so với Cách 2**: Trong các ngôn ngữ như C++/Java, nếu $n$ rất lớn thì $n(n+1)/2$ có thể bị **tràn số nguyên (Integer Overflow)**, trong khi phép XOR không bao giờ bị tràn số.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$.
  - **Space Complexity**: $O(1)$.

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Tìm phần tử duy nhất/bị thiếu** $\rightarrow$ Hai công cụ đắc lực nhất là **Tổng đại số ($\sum$)** và **Toán tử XOR ($\oplus$)**.
2. **Sức mạnh của XOR**:
   - $x \oplus x = 0$ (tự hủy)
   - $x \oplus 0 = x$ (giữ nguyên)
   - Có tính giao hoán và kết hợp: thứ tự XOR không quan trọng.
