# #0836 - Rectangle Overlap

- **LeetCode Link**: [836. Rectangle Overlap](https://leetcode.com/problems/rectangle-overlap/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Math, Geometry, 1D Interval Overlap

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho hai hình chữ nhật song song với các trục tọa độ `rec1 = [x1, y1, x2, y2]` và `rec2 = [x1, y1, x2, y2]`. Kiểm tra xem hai hình chữ nhật có giao nhau với diện tích $> 0$ hay không.
- **Quy ước**:
  - `(x1, y1)` là góc dưới - trái.
  - `(x2, y2)` là góc trên - phải.
  - Chạm nhau ở mép cạnh hoặc đỉnh có diện tích $= 0$, không tính là giao nhau.
- **Ý tưởng cốt lõi**:
  - Hai hình chữ nhật trong không gian 2D giao nhau khi và chỉ khi phần chiếu của chúng giao nhau trên **cả hai trục $Ox$ và $Oy$**.
  - Để giải bài này, có 2 cách tiếp cận:
    1. **Tư duy loại trừ (Phủ định)**: Xác định 4 trường hợp chắc chắn KHÔNG giao nhau (nằm hoàn toàn về bên trái, phải, trên, hoặc dưới).
    2. **Tư duy giao đoạn thẳng 1D**: Kiểm tra điều kiện $\max(\text{start}_1, \text{start}_2) < \min(\text{end}_1, \text{end}_2)$ trên cả hai trục $x$ và $y$.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Kiểm tra vị trí tương đối (Tư duy loại trừ - Optimal $O(1)$ ⭐)
- **Ý tưởng**:
  - `rec1` và `rec2` hoàn toàn tách rời nhau (không giao nhau) nếu xảy ra 1 trong 4 điều kiện sau:
    1. `rec1` nằm hoàn toàn bên trái `rec2`: `rec1[2] <= rec2[0]` ($x_{2} \le x'_{1}$).
    2. `rec1` nằm hoàn toàn bên phải `rec2`: `rec1[0] >= rec2[2]` ($x_{1} \ge x'_{2}$).
    3. `rec1` nằm hoàn toàn bên dưới `rec2`: `rec1[3] <= rec2[1]` ($y_{2} \le y'_{1}$).
    4. `rec1` nằm hoàn toàn bên trên `rec2`: `rec1[1] >= rec2[3]` ($y_{1} \ge y'_{2}$).
  - Nếu bất kỳ điều kiện nào đúng $\implies$ trả về `False`.
  - Ngược lại $\implies$ trả về `True`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(1)$ — chỉ thực hiện 4 phép so sánh số học.
  - **Space Complexity**: $O(1)$ — không sử dụng bộ nhớ phụ.

```python
from typing import List

class Solution:
    def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:
        # Unpack tọa độ để code rõ ràng, tránh magic index [0], [1], [2], [3]
        x1, y1, x2, y2 = rec1
        x1_p, y1_p, x2_p, y2_p = rec2
        
        # Kiểm tra 4 trường hợp không giao nhau
        is_separated = (
            x2 <= x1_p or  # rec1 nằm bên trái rec2
            x1 >= x2_p or  # rec1 nằm bên phải rec2
            y2 <= y1_p or  # rec1 nằm bên dưới rec2
            y1 >= y2_p     # rec1 nằm bên trên rec2
        )
        
        return not is_separated
```

---

### 🔹 Cách 2: Giao của hai đoạn thẳng 1D (Intersection of 1D Intervals)
- **Ý tưởng**:
  - Đoạn thẳng $[A, B]$ và $[C, D]$ giao nhau có độ dài $> 0$ khi và chỉ khi:
    $$\max(A, C) < \min(B, D)$$
  - Áp dụng độc lập cho trục $Ox$ và $Oy$:
    - Trục $Ox$: $\max(x_1, x'_1) < \min(x_2, x'_2)$
    - Trục $Oy$: $\max(y_1, y'_1) < \min(y_2, y'_2)$
  - Hai hình chữ nhật giao nhau khi cả 2 điều kiện trên đồng thời thỏa mãn.
- **Độ phức tạp**:
  - **Time Complexity**: $O(1)$
  - **Space Complexity**: $O(1)$

```python
from typing import List

class Solution:
    def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:
        overlap_x = max(rec1[0], rec2[0]) < min(rec1[2], rec2[2])
        overlap_y = max(rec1[1], rec2[1]) < min(rec1[3], rec2[3])
        return overlap_x and overlap_y
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Bài toán 2D $\rightarrow$ Tách thành 2 bài toán 1D độc lập**:
   - Khi gặp các bài hình học về hình chữ nhật có cạnh song song trục tọa độ (Axis-aligned rectangles), luôn luôn tìm cách tách riêng việc xử lý trục $Ox$ và trục $Oy$.
2. **Kỹ thuật Unpacking biến trong Phỏng vấn**:
   - Thay vì dùng chỉ số `rec1[0], rec1[2]...` dễ gây nhầm lẫn và khó đọc, việc unpack ra các biến mang tên rõ ràng như `x1, y1, x2, y2` giúp code dễ đọc, dễ giải thích với interviewer và giảm thiểu tối đa bug sai sót chỉ số.
