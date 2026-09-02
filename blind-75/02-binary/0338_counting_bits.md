# #0338 - Counting Bits

- **LeetCode Link**: [338. Counting Bits](https://leetcode.com/problems/counting-bits/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Binary, Bit Manipulation, Dynamic Programming (DP)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho một số nguyên `n`. Trả về mảng `ans` có độ dài `n + 1` sao cho `ans[i]` là số lượng bit 1 trong biểu diễn nhị phân của `i` ($0 \le i \le n$).
- **Ví dụ**: `n = 5` $\rightarrow$ `ans = [0, 1, 1, 2, 1, 2]`.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Đếm bit độc lập cho từng số (Brian Kernighan)
- **Ý tưởng**: Lặp từ `0` đến `n`, với mỗi số `i` dùng thuật toán Brian Kernighan (`num & (num - 1)`) để đếm số bit 1.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \log N)$ (chính xác là $O(N \times k)$ với $k \le 32$ là số bit 1).
  - **Space Complexity**: $O(1)$ auxiliary space.

```python
from typing import List

class Solution:
    def countBits(self, n: int) -> List[int]:
        ans = []
        for i in range(n + 1):
            ans.append(0)
            num = i
            while num > 0:
                ans[i] += 1
                num = num & (num - 1)
        return ans
```

---

### 🔹 Cách 2: Quy hoạch động + Thao tác bit (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Bản chất của phép toán `i & (i - 1)` là **tắt đi đúng 1 bit 1 cuối cùng** của số `i`, biến `i` thành một số $j < i$.
  - Vì $j < i$, số lượng bit 1 của $j$ đã được tính trước đó và lưu trong `ans[i & (i - 1)]`.
  - Công thức chuyển trạng thái DP:
    $$\text{ans}[i] = \text{ans}[i \ \& \ (i - 1)] + 1$$
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — chỉ 1 vòng lặp duy nhất từ 1 đến `n`, mỗi bước tính trong $O(1)$.
  - **Space Complexity**: $O(1)$ auxiliary space (không tính mảng kết quả trả về).

```python
from typing import List

class Solution:
    def countBits(self, n: int) -> List[int]:
        ans = [0] * (n + 1)
        for i in range(1, n + 1):
            ans[i] = ans[i & (i - 1)] + 1
        return ans
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Kết hợp DP và Bitwise**: Thay vì tính toán lại từ đầu cho mỗi số ($O(N \log N)$), ta tái sử dụng kết quả của bài toán con nhỏ hơn đã tính trước đó để đạt thời gian tuyến tính **$O(N)$**.
2. **Hệ thức `i & (i - 1)`**: Là công cụ cực mạnh vừa để đếm bit trong $O(1)$, vừa tạo quan hệ chuyển trạng thái trong Quy hoạch động.
