# #0098 - Validate Binary Search Tree

- **LeetCode Link**: [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- **Độ khó (Difficulty)**: Medium
- **Dạng bài (Topic / Pattern)**: Tree, Binary Search Tree (BST), Depth-First Search (DFS), Inorder Traversal

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `root` của một cây nhị phân, kiểm tra xem cây này có phải là một Cây tìm kiếm nhị phân (BST) hợp lệ hay không.
- **Định nghĩa BST hợp lệ**:
  1. Mọi nút ở cây con bên **trái** phải có giá trị **nhỏ hơn nghiêm ngặt** nút hiện tại ($< \text{node.val}$).
  2. Mọi nút ở cây con bên **phải** phải có giá trị **lớn hơn nghiêm ngặt** nút hiện tại ($> \text{node.val}$).
  3. Cả 2 cây con trái và phải cũng phải là các BST hợp lệ.
- **Bẫy kinh điển**:
  - Không thể chỉ kiểm tra quan hệ cục bộ giữa cha và con trực tiếp (`left < root < right`), vì một nút ở nhánh phải có thể nhỏ hơn ông/bà tổ tiên ở phía trên.
  - Mỗi nút phải thỏa mãn một **khoảng giá trị hợp lệ toàn cục**:
    $$\text{low} < \text{node.val} < \text{high}$$
  - Khi rẽ sang **trái**: giới hạn trên bị thu hẹp $\implies (\text{low}, \text{node.val})$.
  - Khi rẽ sang **phải**: giới hạn dưới được nâng lên $\implies (\text{node.val}, \text{high})$.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: DFS Đệ quy với khoảng giá trị Min/Max (Optimal & Cực kỳ tinh gọn ⭐)
- **Ý tưởng**:
  - Đặt giá trị mặc định cho cận dưới $\text{low} = -\infty$ và cận trên $\text{high} = +\infty$ ngay trên tham số hàm.
  - **Base Case**: Nếu `not root` $\rightarrow$ cây rỗng luôn là BST hợp lệ $\rightarrow$ trả về `True`.
  - **Kiểm tra vi phạm**: Nếu `root.val <= low` hoặc `root.val >= high` $\rightarrow$ trả về `False`.
  - **Đệ quy**: Kiểm tra đồng thời cả 2 nhánh con:
    - Nhánh trái với khoảng `(low, root.val)`
    - Nhánh phải với khoảng `(root.val, high)`
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — mỗi nút được duyệt qua đúng 1 lần. Thuật toán có thể dừng sớm (Early Exit) ngay khi phát hiện nút đầu tiên vi phạm.
  - **Space Complexity**: $O(H)$ — chiều cao cây cho bộ nhớ ngăn xếp đệ quy (Call Stack). $O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây suy biến.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from typing import Optional

class Solution:
    def isValidBST(self, root: Optional[TreeNode], low=float('-inf'), high=float('inf')) -> bool:
        # Cây rỗng là BST hợp lệ
        if not root:
            return True
            
        # Giá trị nút hiện tại phải nằm nghiêm ngặt trong khoảng (low, high)
        if root.val <= low or root.val >= high:
            return False
            
        # Thu hẹp khoảng giá trị khi đi xuống 2 nhánh con
        return self.isValidBST(root.left, low, root.val) and self.isValidBST(root.right, root.val, high)
```

---

### 🔹 Cách 2: Duyệt Inorder Traversal (Trái -> Gốc -> Phải)
- **Ý tưởng**:
  - Một tính chất toán học cốt lõi của BST: **Thứ tự duyệt Inorder luôn tạo thành một dãy số tăng nghiêm ngặt (Strictly Increasing Array)**.
  - Ta chỉ cần duy trì một biến `prev` lưu giá trị của nút liền trước trong phép duyệt Inorder:
    - Nếu tại bất kỳ thời điểm nào mà `node.val <= prev` $\rightarrow$ trả về `False`.
  - Có thể thực hiện bằng đệ quy hoặc dùng Stack (Iterative).
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$
  - **Space Complexity**: $O(H)$

```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        prev = float('-inf')
        
        def inorder(node: Optional[TreeNode]) -> bool:
            nonlocal prev
            if not node:
                return True
                
            # Duyệt nhánh trái
            if not inorder(node.left):
                return False
                
            # Kiểm tra tính tăng nghiêm ngặt
            if node.val <= prev:
                return False
            prev = node.val
            
            # Duyệt nhánh phải
            return inorder(node.right)
            
        return inorder(root)
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Dùng dấu so sánh không nghiêm ngặt (`<` thay vì `<=`)**:
   - Theo định nghĩa chuẩn của BST trên LeetCode, các giá trị phải phân biệt hoàn toàn. Nếu cây có 2 nút trùng giá trị (ví dụ `[2, 2, 2]`) thì **không phải** là BST hợp lệ. Do đó điều kiện vi phạm phải là `root.val <= low or root.val >= high`.
2. **Khởi tạo biên bằng `INT_MIN` / `INT_MAX` thay vì `float('-inf')` / `float('inf')`**:
   - Nếu đề bài chứa giá trị nút đạt tới $-2^{31}$ hoặc $2^{31} - 1$, việc dùng hằng số nguyên có thể dẫn đến so sánh sai biên. Dùng `float('-inf')` và `float('inf')` là an toàn tuyệt đối trong Python.
