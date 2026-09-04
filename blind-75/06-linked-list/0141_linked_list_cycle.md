# #0141 - Linked List Cycle

- **LeetCode Link**: [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Linked List, Two Pointers (Floyd's Tortoise and Hare / Cycle Detection)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `head` của một danh sách liên kết. Xác định xem danh sách có chứa chu trình (cycle) hay không.
- **Trực giác**:
  - Nếu danh sách có chu trình, ta sẽ bị cuốn vào một vòng lặp vô tận và không bao giờ chạm tới `None`.
  - Có 2 cách tiếp cận:
    1. Dùng bộ nhớ ngoài (Hash Set) để ghi nhớ các node đã đi qua.
    2. Dùng 2 con trỏ chạy với 2 vận tốc khác nhau (Rùa & Thỏ) để đạt $O(1)$ Space.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Dùng Hash Set (Ghi nhớ node đã thăm)
- **Ý tưởng**: Duyệt qua từng node, nếu node đã có trong `seen` $\rightarrow$ có chu trình. Nếu gặp `None` $\rightarrow$ không có chu trình.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$
  - **Space Complexity**: $O(N)$ — lưu các node vào `set`.

```python
from typing import Optional

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        seen = set()
        curr = head
        while curr:
            if curr in seen:
                return True
            seen.add(curr)
            curr = curr.next
        return False
```

---

### 🔹 Cách 2: Thuật toán Rùa & Thỏ (Floyd's Cycle Detection - Optimal ⭐)
- **Ý tưởng**:
  - Thả 2 con trỏ cùng xuất phát từ `head`:
    - `slow`: Mỗi bước đi **1 node** (`slow = slow.next`).
    - `fast`: Mỗi bước đi **2 node** (`fast = fast.next.next`).
  - Nếu danh sách **không có chu trình**: `fast` (hoặc `fast.next`) sẽ chạm tới `None` trước $\rightarrow$ Trả về `False`.
  - Nếu danh sách **có chu trình**: Vì `fast` chạy nhanh hơn `slow` 1 node mỗi vòng lặp, nên khoảng cách giữa `fast` và `slow` trong vòng tròn sẽ rút ngắn dần 1 node sau mỗi bước, chắc chắn `fast` sẽ đuổi kịp và trùng vị trí với `slow` (`slow == fast`) $\rightarrow$ Trả về `True`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — tối đa $N$ bước lặp.
  - **Space Complexity**: $O(1)$ — chỉ dùng đúng 2 con trỏ, không phụ thuộc vào độ dài danh sách.

```python
from typing import Optional

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
                
        return False
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
- **Thiếu điều kiện `fast.next`**: Vòng lặp phải kiểm tra `while fast and fast.next:` để đảm bảo lệnh `fast.next.next` không bao giờ bị lỗi `NoneType` khi danh sách có độ dài lẻ hoặc chỉ có 1 phần tử.
- **Nhầm lẫn `set` và `dict`**: Khi chỉ cần kiểm tra xem phần tử đã tồn tại hay chưa, dùng `set` tiết kiệm một nửa bộ nhớ so với `dict` (vì không cần lưu thêm con trỏ value).

---

## 4. Bài học & Mẹo nhớ (Key Takeaways)
- **Pattern Fast & Slow Pointers (Floyd's Algorithm)**:
  - Dùng để: Phát hiện chu trình trong Linked List, tìm điểm bắt đầu của chu trình (#142), tìm trung điểm của danh sách (Middle of Linked List), kiểm tra danh sách Palindrome (#234).
