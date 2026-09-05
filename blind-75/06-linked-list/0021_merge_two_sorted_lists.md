# #0021 - Merge Two Sorted Lists

- **LeetCode Link**: [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Linked List, Two Pointers, Dummy Node Pattern

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho 2 danh sách liên kết đơn `list1` và `list2` đã được sắp xếp tăng dần. Hãy gộp chúng thành 1 danh sách duy nhất cũng có thứ tự tăng dần.
- **Trực giác cốt lõi**:
  - Tại mỗi bước, ta so sánh giá trị ở đầu 2 danh sách: node nào nhỏ hơn thì móc vào danh sách mới.
  - **Kỹ thuật Dummy Node (Node giả)**: Dùng `dummy = ListNode(0)` làm điểm tựa ban đầu và con trỏ `tail` để nối đuôi. Nhờ đó, ta không cần viết code lằng nhằng để xác định xem ai sẽ là `head` đầu tiên, đồng thời xử lý tự nhiên trường hợp một trong hai danh sách rỗng.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách: Dùng Dummy Node & Con trỏ Tail (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  1. Tạo node mốc `dummy = ListNode(0)` và con trỏ nối đuôi `tail = dummy`.
  2. Khi cả 2 danh sách còn phần tử (`while list1 and list2:`):
     - Nếu `list1.val <= list2.val`: `tail.next = list1`, dịch `list1 = list1.next`.
     - Ngược lại: `tail.next = list2`, dịch `list2 = list2.next`.
     - Luôn dịch `tail = tail.next`.
  3. Khi 1 danh sách hết trước, móc phần còn lại của danh sách kia vào đuôi: `tail.next = list1 if list1 else list2`.
  4. Trả về `dummy.next`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N + M)$ — với $N, M$ là độ dài của 2 danh sách.
  - **Space Complexity**: $O(1)$ — chỉ tạo 1 node giả duy nhất, việc gộp diễn ra in-place bằng cách nối lại con trỏ.

```python
from typing import Optional

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        tail = dummy
        
        while list1 and list2:
            if list1.val <= list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                tail.next = list2
                list2 = list2.next
            tail = tail.next
            
        tail.next = list1 if list1 else list2
        return dummy.next
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Sức mạnh của Dummy Node**: Bất cứ khi nào bài toán yêu cầu **tạo danh sách mới** hoặc **thay đổi node đầu tiên** (như xóa node đầu, gộp danh sách, chia nhánh), luôn khởi tạo một `dummy = ListNode(0)` để loại bỏ mọi trường hợp biên phức tạp.
2. **Nối đuôi nguyên cụm**: Không cần chạy vòng lặp để chép từng phần tử còn lại; chỉ cần 1 câu lệnh `tail.next = list1 if list1 else list2` là xong vì phần còn lại đã được sắp xếp sẵn.
