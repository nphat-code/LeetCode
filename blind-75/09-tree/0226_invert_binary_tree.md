# #0226 - Invert Binary Tree

- **LeetCode Link**: [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Tree, Depth-First Search (DFS), Breadth-First Search (BFS)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `root` của một cây nhị phân. Hãy đảo ngược cấu trúc cây (hoán đổi cây con bên trái và bên phải ở mọi nút) và trả về `root`.
- **Trực giác cốt lõi**:
  - Tại mỗi nút, ta cần tráo đổi liên kết con trỏ `left` và `right` cho nhau:
    $$\text{left}, \text{right} = \text{right}, \text{left}$$
  - Tiếp tục áp dụng quy tắc đảo này đệ quy xuống các cây con bên dưới cho đến khi chạm nút lá (`None`).
  - Có thể duyệt theo chiều sâu (**DFS**) hoặc duyệt theo tầng (**BFS**) đều giải quyết triệt để bài toán.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: DFS Đệ quy (Optimal & Pythonic ⭐)
- **Ý tưởng**:
  - **Base Case**: Nếu nút hiện tại là `None` $\rightarrow$ trả về `None`.
  - **Recursive Step**: Gán `root.left` bằng kết quả sau khi invert nhánh phải `self.invertTree(root.right)`, và `root.right` bằng kết quả sau khi invert nhánh trái `self.invertTree(root.left)`.
  - Nhờ cơ chế unpack tuple của Python, vế phải `(self.invertTree(root.right), self.invertTree(root.left))` được tính toán đầy đủ trước khi gán vào vế trái, không bị ghi đè dữ liệu.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — mỗi nút trong cây được duyệt đúng 1 lần.
  - **Space Complexity**: $O(H)$ — với $H$ là chiều cao của cây cho bộ nhớ Call Stack ($O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây suy biến lệch 1 phía).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        
        # Đệ quy đảo nhánh phải gán cho trái, và đảo nhánh trái gán cho phải
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```

---

### 🔹 Cách 2: BFS Duyệt theo tầng với Queue (Iterative)
- **Ý tưởng**:
  - Sử dụng `collections.deque` để duyệt cây theo thứ tự tầng (Level-order).
  - Với mỗi nút lấy ra từ đầu hàng đợi (`queue.popleft()`):
    1. Hoán đổi 2 con trỏ: `node.left, node.right = node.right, node.left`.
    2. Đẩy các nút con hợp lệ (khác `None`) vào queue để xử lý tiếp ở các vòng lặp sau.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — duyệt qua $N$ nút.
  - **Space Complexity**: $O(W)$ — với $W$ là độ rộng lớn nhất của cây (ở tầng đáy của cây nhị phân hoàn chỉnh, $W \approx N/2 \implies O(N)$).

```python
from collections import deque

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
            
        queue = deque([root])
        while queue:
            node = queue.popleft()
            
            # Hoán đổi hai cây con
            node.left, node.right = node.right, node.left
            
            # Đẩy các node con tồn tại vào hàng đợi
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
                
        return root
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Lỗi ghi đè con trỏ trong các ngôn ngữ khác**:
   - Trong C++/Java hoặc cách viết gán tuần tự, nếu gán `root.left = invert(root.right)` trước thì con trỏ `root.left` gốc sẽ bị mất. Cần lưu lại vào biến tạm: `temp = root.left; root.left = invert(root.right); root.right = invert(temp);`.
   - Trong Python, cú pháp unpack `a, b = b, a` giải quyết điều này rất tự nhiên.
2. **Dùng `break` thay vì `continue` khi duyệt vòng lặp**:
   - Khi lặp trong tầng, nếu gặp node lá, không được dùng `break` vì sẽ ngắt vòng lặp và bỏ sót các node còn lại ở cùng tầng. Tốt nhất là không cần điều kiện phụ, chỉ đổi chỗ trực tiếp và đẩy các con khác `None` vào queue.
