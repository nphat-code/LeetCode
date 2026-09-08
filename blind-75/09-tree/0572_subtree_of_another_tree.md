# #0572 - Subtree of Another Tree

- **LeetCode Link**: [572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Tree, Depth-First Search (DFS), Recursion, Same Tree Pattern

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho hai cây nhị phân `root` và `subRoot`. Kiểm tra xem có tồn tại một cây con (subtree) nào trong `root` có cấu trúc và giá trị giống hệt với `subRoot` hay không.
- **Định nghĩa Subtree**: Cây con của một node bao gồm chính node đó và **toàn bộ con cháu (descendants)** của nó.
- **Trực giác cốt lõi**:
  - `subRoot` là cây con của `root` khi và chỉ khi thỏa mãn một trong các điều kiện:
    1. Cây bắt đầu ngay tại nút `root` hiện tại **giống hệt** `subRoot` (`isSameTree(root, subRoot) == True`).
    2. HOẶC `subRoot` là cây con của nhánh bên trái (`isSubtree(root.left, subRoot)`).
    3. HOẶC `subRoot` là cây con của nhánh bên phải (`isSubtree(root.right, subRoot)`).

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: DFS Đệ quy kết hợp Helper `isSameTree` (Optimal & Chuẩn mực ⭐)
- **Ý tưởng**:
  - Tách bài toán thành 2 hàm độc lập:
    - `isSameTree(p, q)`: Kiểm tra hai cây có giống hệt nhau từ cấu trúc đến giá trị từng node hay không (kế thừa từ bài #0100).
    - `isSubtree(root, subRoot)`: Duyệt qua từng node của `root`. Tại mỗi node, thử khớp với `subRoot` bằng `isSameTree`. Nếu không khớp, tiếp tục đệ quy tìm kiếm ở hai cây con trái và phải bằng toán tử `or`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \times M)$ — với $N$ là số lượng nút trong `root` và $M$ là số lượng nút trong `subRoot`. Trong trường hợp xấu nhất (cây toàn các giá trị giống nhau), tại mỗi nút của `root` ta phải duyệt tối đa $M$ nút của `subRoot`. Trong thực tế, thuật toán chạy rất nhanh nhờ ngắt sớm khi `p.val != q.val`.
  - **Space Complexity**: $O(H_{root} + H_{subRoot})$ — chiều cao của cây cho bộ nhớ Call Stack đệ quy ($O(N + M)$ trong trường hợp cây suy biến lệch 1 phía, $O(\log N + \log M)$ nếu cây cân bằng).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        # Nếu cây root rỗng mà subRoot có nút thì không thể chứa subRoot
        if not root:
            return False
            
        # Nếu cây bắt đầu từ root khớp hoàn toàn với subRoot
        if self.isSameTree(root, subRoot):
            return True
            
        # Nếu không khớp tại root, tiếp tục tìm ở nhánh trái HOẶC nhánh phải
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # Base cases
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val != q.val:
            return False
            
        # Đệ quy kiểm tra đồng thời cả hai nhánh con
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Nhầm lẫn giữa `and` và `or` trong `isSubtree`**:
   - `subRoot` chỉ cần xuất hiện ở nhánh trái **hoặc** nhánh phải, không cần và không thể xuất hiện ở cả hai cùng lúc $\implies$ bắt buộc dùng `or`.
2. **Gộp chung logic `isSameTree` vào `isSubtree`**:
   - Khi `root.val == subRoot.val`, hai cây bên dưới chưa chắc đã giống hệt nhau. Nếu gộp chung và trả về kết quả kiểm tra con ngay lập tức, ta sẽ bỏ qua trường hợp `subRoot` thực chất nằm ở một tầng sâu hơn bên dưới có cùng giá trị root.
3. **Thứ tự đặt Base Case**:
   - Đặt `if not root: return False` lên trước `if self.isSameTree(...)` giúp tiết kiệm một lần gọi hàm `isSameTree(None, subRoot)` không cần thiết khi chạm nút lá.
