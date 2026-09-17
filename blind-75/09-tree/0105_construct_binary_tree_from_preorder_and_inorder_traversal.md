# #0105 - Construct Binary Tree from Preorder and Inorder Traversal

- **LeetCode Link**: [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- **Độ khó (Difficulty)**: Medium
- **Dạng bài (Topic / Pattern)**: Tree, Binary Tree, Divide and Conquer, Hash Map

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho hai mảng số nguyên `preorder` và `inorder` của cùng một cây nhị phân (các giá trị là duy nhất). Hãy tái dựng lại cây nhị phân ban đầu và trả về nút `root`.
- **Đặc trưng của hai phép duyệt cây**:
  1. **Preorder Traversal (`Root` $\rightarrow$ `Left` $\rightarrow$ `Right`)**:
     - Phần tử đầu tiên luôn luôn là **nút Gốc (Root)** của cây / cây con hiện tại.
  2. **Inorder Traversal (`Left` $\rightarrow$ `Root` $\rightarrow$ `Right`)**:
     - Nút Gốc đóng vai trò là **vạch phân cách**:
       - Mọi phần tử nằm bên trái vị trí của `Root` thuộc về **cây con bên trái**.
       - Mọi phần tử nằm bên phải vị trí của `Root` thuộc về **cây con bên phải**.
- **Ý tưởng Chia để trị (Divide and Conquer)**:
  - Xác định nút `root` từ `preorder`.
  - Tìm vị trí của `root` trong `inorder` để biết kích thước của cây con trái và phải.
  - Phân chia các mảng thành các phần tương ứng và đệ quy giải tiếp cho cây con trái và phải.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Đệ quy cơ bản với cắt mảng Slicing (Baseline $O(N^2)$)
- **Ý tưởng**:
  - Dùng vòng lặp hoặc `inorder.index(root.val)` để tìm vị trí của `root.val` trong `inorder`.
  - Dùng kỹ thuật cắt lát mảng `[:]` của Python để tạo các mảng con cho nhánh trái và nhánh phải.
  - Đệ quy:
    - `root.left = self.buildTree(preorder_left, inorder_left)`
    - `root.right = self.buildTree(preorder_right, inorder_right)`
- **Độ phức tạp**:
  - **Time Complexity**: $O(N^2)$ — ở mỗi nút tốn $O(N)$ để tìm vị trí trong `inorder` và $O(N)$ để sao chép mảng khi cắt lát `[:]`.
  - **Space Complexity**: $O(N^2)$ — tốn bộ nhớ để lưu các mảng con mới được tạo ra ở mỗi tầng đệ quy.

```python
from typing import List, Optional

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None
            
        root = TreeNode(preorder[0])
        
        # Tìm vị trí gốc trong inorder
        mid = inorder.index(root.val)
        
        # Cắt mảng tương ứng
        inorder_left = inorder[:mid]
        inorder_right = inorder[mid + 1:]
        
        preorder_left = preorder[1 : 1 + len(inorder_left)]
        preorder_right = preorder[1 + len(inorder_left):]
        
        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right, inorder_right)
        
        return root
```

---

### 🔹 Cách 2: Đệ quy tối ưu với Hash Map & Con trỏ biên (Optimal $O(N)$ ⭐)
- **Ý tưởng giải quyết 2 điểm nghẽn của Cách 1**:
  1. **Tối ưu tra cứu**: Tạo sẵn một Dictionary `inorder_index_map = {val: idx for idx, val in enumerate(inorder)}` giúp tìm vị trí nút gốc trong `inorder` với thời gian **$O(1)$**.
  2. **Tối ưu bộ nhớ**: Không cắt mảng! Chỉ truyền 2 chỉ số biên `(in_left, in_right)` để xác định phạm vi cây con trên mảng `inorder`.
  3. **Con trỏ `preorder_idx`**: Vì `preorder` duyệt theo thứ tự `Root -> Left -> Right`, mỗi lần tạo một nút ta chỉ cần tăng `preorder_idx += 1` và **luôn dựng cây con bên trái trước rồi mới đến bên phải**.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — xây dựng Hash Map tốn $O(N)$, mỗi nút trong cây được tạo đúng một lần với chi phí $O(1)$.
  - **Space Complexity**: $O(N)$ — gồm $O(N)$ cho Hash Map và $O(H)$ cho ngăn xếp đệ quy (Call Stack).

```python
from typing import List, Optional

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # 1. Bảng băm tra cứu vị trí trong inorder với thời gian O(1)
        inorder_index_map = {val: idx for idx, val in enumerate(inorder)}
        preorder_idx = 0
        
        def build_subtree(in_left: int, in_right: int) -> Optional[TreeNode]:
            nonlocal preorder_idx
            
            # Base Case: Phạm vi không còn nút nào
            if in_left > in_right:
                return None
                
            # Lấy giá trị nút gốc tiếp theo từ preorder
            root_val = preorder[preorder_idx]
            preorder_idx += 1
            root = TreeNode(root_val)
            
            # Tìm vị trí nút gốc trong inorder
            root_inorder_idx = inorder_index_map[root_val]
            
            # Đệ quy dựng nhánh trái trước, nhánh phải sau
            root.left = build_subtree(in_left, root_inorder_idx - 1)
            root.right = build_subtree(root_inorder_idx + 1, in_right)
            
            return root
            
        return build_subtree(0, len(inorder) - 1)
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Preorder cho biết Gốc, Inorder cho biết Kích thước & Ranh giới**:
   - `preorder[0]` luôn là Root.
   - Vị trí của Root trong `inorder` chia cây thành 2 nửa trái và phải.
2. **Kỹ thuật tối ưu kinh điển: Hash Map + Pointer**:
   - Mỗi khi gặp bài toán đệ quy cắt mảng trên dữ liệu không trùng lặp, hãy nghĩ ngay đến việc:
     - Dùng Hash Map để tra cứu vị trí $O(1)$.
     - Dùng các con trỏ chỉ số biên `(left, right)` thay vì copy mảng bằng slicing `[:]`.
