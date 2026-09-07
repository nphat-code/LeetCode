# #0100 - Same Tree

- **LeetCode Link**: [100. Same Tree](https://leetcode.com/problems/same-tree/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Tree, Depth-First Search (DFS), Recursion

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho hai cây nhị phân `p` và `q`. Kiểm tra xem hai cây có giống hệt nhau về cả cấu trúc và giá trị của từng nút tương ứng hay không.
- **Trực giác cốt lõi**:
  - Hai cây `p` và `q` giống nhau khi và chỉ khi:
    1. Cả hai nút hiện tại cùng rỗng (đạt đến đáy nhánh cây) $\implies$ `True`.
    2. Một bên rỗng và một bên có nút $\implies$ `False` (lệch cấu trúc).
    3. Cả hai cùng có giá trị nhưng `p.val != q.val` $\implies$ `False`.
    4. Cả hai cùng giá trị $\implies$ kiểm tra tiếp hai nhánh con: `isSameTree(p.left, q.left)` VÀ `isSameTree(p.right, q.right)`.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: DFS Đệ quy (Optimal & Chuẩn mực nhất ⭐)
- **Ý tưởng**:
  - Dùng đệ quy duyệt đồng thời cả hai cây từ nút gốc (`root`).
  - Kiểm tra 3 điều kiện dừng (Base Cases) trước, sau đó đệ quy kiểm tra đồng thời cả 2 nhánh con trái và phải.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — với $N$ là số lượng nút của cây nhỏ hơn (thuật toán sẽ dừng sớm ngay khi phát hiện cặp nút không khớp đầu tiên).
  - **Space Complexity**: $O(H)$ — với $H$ là chiều cao cây cho bộ nhớ Call Stack ($O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây suy biến thành danh sách liên kết).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # Trường hợp 1: Cả hai cây đều rỗng
        if not p and not q:
            return True
            
        # Trường hợp 2: Một trong hai cây rỗng (cấu trúc không khớp)
        if not p or not q:
            return False
            
        # Trường hợp 3: Giá trị hai nút khác nhau
        if p.val != q.val:
            return False
            
        # Trường hợp 4: Nút hiện tại khớp, tiếp tục kiểm tra 2 cây con
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

---

### 🔹 Cách 2: BFS Duyệt theo tầng với Queue (Iterative)
- **Ý tưởng**:
  - Dùng một hàng đợi lưu các cặp nút cần so sánh: `queue = deque([(p, q)])`.
  - Mỗi vòng lặp rút cặp `(node1, node2)` ra kiểm tra:
    - Nếu cả hai đều `None` $\rightarrow$ hợp lệ, tiếp tục.
    - Nếu một trong hai `None` hoặc `node1.val != node2.val` $\rightarrow$ `return False`.
    - Đẩy các cặp con tương ứng vào queue: `(node1.left, node2.left)` và `(node1.right, node2.right)`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$.
  - **Space Complexity**: $O(W)$ — với $W$ là độ rộng lớn nhất của cây.

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Trả về `True` quá sớm khi `p.val == q.val`**: Chỉ vì nút hiện tại bằng nhau chưa đủ để kết luận hai cây giống nhau, bắt buộc phải đệ quy kiểm tra toàn bộ các nút con bên dưới.
2. **Kiểm tra thiếu một bên rỗng**: Cần phân biệt rõ giữa trường hợp cả hai cùng `None` (`True`) và chỉ một trong hai `None` (`False`).
3. **Mẹo Clean Code**: Sau khi đã kiểm tra `if not p and not q: return True`, ở điều kiện kế tiếp bạn chỉ cần viết ngắn gọn `if not p or not q: return False` thay vì `not p and q or p and not q`.
