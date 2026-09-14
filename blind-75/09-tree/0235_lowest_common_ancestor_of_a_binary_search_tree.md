# #0235 - Lowest Common Ancestor of a Binary Search Tree

- **LeetCode Link**: [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- **Độ khó (Difficulty)**: Medium (trên LeetCode đôi khi được xếp Easy/Medium)
- **Dạng bài (Topic / Pattern)**: Tree, Binary Search Tree (BST), Divide and Conquer, BST Properties

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `root` của một Cây tìm kiếm nhị phân (BST) và hai nút `p`, `q` thuộc cây. Tìm và trả về **Tổ tiên chung thấp nhất (LCA)** của `p` và `q`.
- **Định nghĩa LCA**: Nút $T$ thấp nhất trên cây sao cho cả $p$ và $q$ đều là hậu duệ của $T$ (một nút có thể là hậu duệ của chính nó).
- **Ràng buộc**:
  - Số lượng nút: $[2, 10^5]$.
  - $p \ne q$.
  - Cả $p$ và $q$ đảm bảo luôn tồn tại trên cây.
- **Tính chất cốt lõi của BST**:
  - Mọi nút ở cây con bên trái đều $< \text{node.val}$.
  - Mọi nút ở cây con bên phải đều $> \text{node.val}$.
  - Do đó, từ một nút gốc:
    1. Nếu cả $p$ và $q$ đều nhỏ hơn nút hiện tại $\implies$ cả hai nằm hoàn toàn ở **nhánh trái** $\implies$ LCA phải nằm ở nhánh trái.
    2. Nếu cả $p$ và $q$ đều lớn hơn nút hiện tại $\implies$ cả hai nằm hoàn toàn ở **nhánh phải** $\implies$ LCA phải nằm ở nhánh phải.
    3. Nếu xảy ra hiện tượng **chia nhánh (Split Point)** — tức là một nút nhỏ hơn và một nút lớn hơn, HOẶC nút hiện tại bằng chính $p$ hoặc $q$ $\implies$ **nút hiện tại chính là LCA**!

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Đệ quy theo thuộc tính BST (Recursive)
- **Ý tưởng**:
  - So sánh `p.val` và `q.val` với `root.val`:
    - Nếu cả hai $< root.val$: đệ quy sang `root.left`.
    - Nếu cả hai $> root.val$: đệ quy sang `root.right`.
    - Ngược lại: trả về `root`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(H)$ — với $H$ là chiều cao cây ($O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây suy biến). Mỗi tầng ta chỉ đi xuống đúng 1 nút.
  - **Space Complexity**: $O(H)$ — bộ nhớ ngăn xếp đệ quy (Call Stack).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        return root
```

---

### 🔹 Cách 2: Vòng lặp khử đệ quy (Iterative - Optimal $O(1)$ Space ⭐)
- **Ý tưởng**:
  - Do thuật toán là Đệ quy đuôi (Tail Recursion) chỉ đi xuống 1 nhánh duy nhất, ta có thể dùng vòng lặp `while` để duyệt.
  - Duy trì con trỏ `curr` bắt đầu từ `root`.
  - Di chuyển `curr = curr.left` hoặc `curr = curr.right` tùy thuộc vào giá trị so sánh.
  - Khi gặp điểm chia rẽ hoặc chạm vào $p$/$q$, trả về `curr` ngay lập tức.
- **Tại sao tối ưu hơn**: Loại bỏ hoàn toàn $O(H)$ Call Stack, đạt mức tiêu thụ bộ nhớ phụ lý tưởng $O(1)$.
- **Độ phức tạp**:
  - **Time Complexity**: $O(H)$ — tương tự đệ quy, chỉ duyệt dọc theo 1 đường đi từ gốc đến điểm chia rẽ.
  - **Space Complexity**: $O(1)$ — không dùng thêm bộ nhớ phụ nào.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        curr = root
        
        while curr:
            # Cả hai nằm bên trái
            if p.val < curr.val and q.val < curr.val:
                curr = curr.left
            # Cả hai nằm bên phải
            elif p.val > curr.val and q.val > curr.val:
                curr = curr.right
            # Điểm chia rẽ (Split Point) hoặc trùng p/q -> chính là LCA
            else:
                return curr
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Khác biệt cốt lõi giữa LCA trên BST vs. LCA trên Cây nhị phân thường (BT)**:
   - Trên **BST** (#0235): Nhờ có thứ tự giá trị ($Left < Root < Right$), ta chỉ cần chọn **đúng 1 nhánh** để đi xuống ở mỗi tầng $\implies$ Thời gian $O(H)$, Bộ nhớ $O(1)$.
   - Trên **Binary Tree thông thường** (#0236): Không có tính chất thứ tự, bắt buộc phải duyệt cả 2 nhánh trái và phải bằng Post-order DFS $\implies$ Phức tạp hơn.
2. **Kỹ thuật Clean Code khi Phỏng vấn**:
   - Khởi tạo con trỏ duyệt `curr = root` thay vì ghi đè trực tiếp tham số `root` giúp code rõ ràng về mặt ngữ nghĩa và giữ nguyên tham chiếu ban đầu.
