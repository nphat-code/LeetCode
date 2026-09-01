# #0001 - Two Sum

- **LeetCode Link**: [1. Two Sum](https://leetcode.com/problems/two-sum/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Array, Hash Table (One-pass Hash Map)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho mảng số nguyên `nums` và một số nguyên `target`. Tìm **chỉ số (indices)** của 2 số sao cho tổng của chúng bằng `target`.
- **Giả thiết quan trọng**: Mỗi đầu vào có đúng một nghiệm duy nhất và không được dùng cùng 1 phần tử 2 lần.
- **Quan hệ toán học**: Nếu phần tử hiện tại là `x`, ta cần tìm xem trong mảng đã có phần tử `complement = target - x` hay chưa.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Vét cạn (Brute Force)
- **Ý tưởng**: Dùng 2 vòng lặp lồng nhau duyệt qua mọi cặp số `(nums[i], nums[j])` với `i < j` để kiểm tra `nums[i] + nums[j] == target`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N^2)$ — duyệt mọi cặp số.
  - **Space Complexity**: $O(1)$ — không tốn thêm bộ nhớ.

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        n = len(nums)
        for i in range(n):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []
```

---

### 🔹 Cách 2: One-pass Hash Map (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Vừa duyệt qua mảng, vừa tính `complement = target - nums[i]`.
  - Kiểm tra `complement` đã xuất hiện trong bảng băm (`hashmap`) hay chưa.
  - Nếu có: trả về ngay cặp chỉ số `[hashmap[complement], i]`.
  - Nếu chưa: lưu `hashmap[nums[i]] = i` để các số phía sau có thể tra cứu.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — chỉ duyệt qua mảng đúng 1 lần, thao tác tìm kiếm trong Hash Map mất trung bình $O(1)$.
  - **Space Complexity**: $O(N)$ — lưu tối đa $N$ phần tử trong `hashmap`.

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hashmap:
                return [hashmap[complement], i]
            hashmap[nums[i]] = i
        return []
```

> **💡 Mẹo viết Pythonic hơn (Dùng `enumerate`):**
> ```python
> class Solution:
>     def twoSum(self, nums: List[int], target: int) -> List[int]:
>         hashmap = {}
>         for i, num in enumerate(nums):
>             complement = target - num
>             if complement in hashmap:
>                 return [hashmap[complement], i]
>             hashmap[num] = i
>         return []
> ```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Core Pattern**: Khi bài toán yêu cầu tìm cặp số thỏa mãn $A + B = Target$, chuyển về dạng **$B = Target - A$** và dùng `Hash Map` để tra cứu trong $O(1)$.
2. **Tránh bẫy Self-pairing**: Chỉ gán `hashmap[nums[i]] = i` **sau** bước kiểm tra `complement in hashmap`. Nếu gán trước, khi gặp `target = 6` và `nums[0] = 3`, code sẽ trả về `[0, 0]` (sai vì dùng 1 phần tử 2 lần).
