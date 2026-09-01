# #0217 - Contains Duplicate

- **LeetCode Link**: [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Array, Hash Set, Sorting

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho mảng số nguyên `nums`. Trả về `True` nếu có **bất kỳ giá trị nào xuất hiện ít nhất 2 lần**, ngược lại trả về `False` nếu tất cả các phần tử đều phân biệt.
- **Trực giác**: Cần một cấu trúc dữ liệu cho phép kiểm tra sự tồn tại của một phần tử trong thời gian $O(1)$ $\rightarrow$ **Hash Set**.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: One-liner với `len(set(nums))` (Cách của bạn ⭐)
- **Ý tưởng**: Chuyển đổi mảng thành `set` (tập hợp các phần tử duy nhất). Nếu độ dài của `set` khác độ dài mảng ban đầu $\rightarrow$ có phần tử bị trùng.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — duyệt qua mảng để tạo set.
  - **Space Complexity**: $O(N)$ — lưu các phần tử vào set.

```python
from typing import List

class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        return len(nums) != len(set(nums))
```

---

### 🔹 Cách 2: Early Exit với Hash Set (Tối ưu thời gian chạy thực tế)
- **Ý tưởng**:
  - Vừa duyệt mảng vừa thêm vào `seen = set()`.
  - Nếu `num in seen` $\rightarrow$ trả về `True` ngay lập tức (**Early Return**).
  - Ưu điểm so với Cách 1: Không cần duyệt hết toàn bộ $N$ phần tử nếu phần tử trùng nằm ở đầu mảng (ví dụ: `[1, 1, 2, 3, 4, ...]`).
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ trường hợp xấu nhất, $O(1)$ trường hợp tốt nhất.
  - **Space Complexity**: $O(N)$.

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```

---

### 🔹 Cách 3: Sắp xếp (Sort) — Đánh đổi Time lấy Space $O(1)$
- **Ý tưởng**: Sort mảng tăng dần. Nếu có phần tử trùng, chúng sẽ nằm cạnh nhau: `nums[i] == nums[i - 1]`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \log N)$ — do thao tác sắp xếp.
  - **Space Complexity**: $O(1)$ hoặc $O(N)$ tùy thuật toán sort của ngôn ngữ (In-place sort).

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return True
        return False
```

---

## 3. So sánh các cách tiếp cận trong Phỏng vấn

| Cách tiếp cận | Time (Worst) | Time (Best) | Space | Ưu điểm | Nhược điểm |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **Set Length (One-liner)** | $O(N)$ | $O(N)$ | $O(N)$ | Ngắn gọn, chuẩn Python | Luôn duyệt hết mảng |
| **Set with Early Exit** | $O(N)$ | $O(1)$ | $O(N)$ | Dừng sớm khi tìm thấy | Tốn bộ nhớ |
| **Sorting** | $O(N \log N)$ | $O(N \log N)$ | $O(1)$ | Tiết kiệm bộ nhớ | Chậm hơn |

---

## 4. Bài học & Mẹo nhớ (Key Takeaways)
- Muốn kiểm tra sự tồn tại (Uniqueness / Membership test) $\rightarrow$ Nghĩ ngay đến **Hash Set** ($O(1)$ lookup).
- Khi phỏng vấn, hãy chủ động nhắc đến **Early Exit** để thể hiện sự quan tâm đến hiệu năng thực tế (Best/Average case).
