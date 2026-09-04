# #0206 - Reverse Linked List

- **LeetCode Link**: [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Linked List, In-place Pointer Reversal (Three Pointers)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `head` của một danh sách liên kết đơn. Hãy đảo ngược danh sách và trả về `head` mới.
- **Ví dụ**: `1 -> 2 -> 3 -> 4 -> 5 -> None` $\rightarrow$ `5 -> 4 -> 3 -> 2 -> 1 -> None`.
- **Trực giác cốt lõi**:
  - Với mỗi node `curr`, ta muốn bẻ mũi tên trỏ ngược về node phía trước: `curr.next = prev`.
  - Nhưng nếu bẻ ngay lập tức, ta sẽ mất liên kết tới các node phía sau. Do đó, cần một con trỏ tạm `next_node = curr.next` để "giữ chân" danh sách còn lại trước khi bẻ mũi tên.
  - Khởi tạo `prev = None` để node đầu tiên sau khi bị đảo ngược sẽ trở thành node đuôi (trỏ vào `None`), đồng thời xử lý tự nhiên trường hợp danh sách rỗng.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách: Lặp 3 con trỏ (Iterative - Optimal ⭐)
- **Ý tưởng**:
  - Dùng 3 con trỏ: `prev`, `curr`, `next_node`.
  - Mỗi bước lặp gồm 4 thao tác nhịp nhàng:
    1. `next_node = curr.next` (Lưu node sau)
    2. `curr.next = prev` (Bẻ ngược mũi tên)
    3. `prev = curr` (Dịch prev lên)
    4. `curr = next_node` (Dịch curr lên)
  - Khi `curr` chạy đến `None`, `prev` chính là node cuối cùng ban đầu $\rightarrow$ trở thành `head` mới.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — duyệt qua danh sách 1 lần.
  - **Space Complexity**: $O(1)$ — in-place, không tốn thêm bộ nhớ.

```python
from typing import Optional

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        curr = head
        
        while curr:
            next_node = curr.next  # 1. Lưu lại node phía sau
            curr.next = prev       # 2. Đảo chiều con trỏ về trước
            prev = curr            # 3. Dịch prev tiến lên
            curr = next_node       # 4. Dịch curr tiến lên
            
        return prev
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Quên tạo biến tạm `next_node`**: Bẻ `curr.next = prev` trước khi lưu `curr.next` sẽ làm đứt xích và mất toàn bộ phần còn lại của danh sách.
2. **Khởi tạo `prev = head` thay vì `prev = None`**: Sẽ khiến node đầu tiên trỏ ngược lại node thứ hai ban đầu, tạo ra vòng lặp vô tận (Cycle `1 <-> 2`).
3. **Quên xử lý `head = None`**: Khởi tạo `prev = None` và `curr = head` giúp code tự động trả về `None` khi danh sách rỗng mà không cần `if not head: return None`.

---

## 4. Bài học & Mẹo nhớ (Key Takeaways)
- **Kỹ thuật 3 con trỏ trong Linked List**: Luôn nhớ trình tự 4 bước: **Lưu sau $\rightarrow$ Bẻ ngược $\rightarrow$ Tiến prev $\rightarrow$ Tiến curr**.
