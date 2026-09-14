# #0230 - Kth Smallest Element in a BST

- **LeetCode Link**: [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
- **Độ khó (Difficulty)**: Medium
- **Dạng bài (Topic / Pattern)**: Tree, Binary Search Tree (BST), Depth-First Search (DFS), Inorder Traversal, Early Exit

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `root` của một Cây tìm kiếm nhị phân (BST) và một số nguyên `k`, tìm và trả về giá trị nhỏ thứ `k` (1-indexed) trong cây.
- **Tính chất cốt lõi của BST**:
  - Khi duyệt theo thứ tự giữa (**Inorder Traversal**: `Left` $\rightarrow$ `Root` $\rightarrow$ `Right`), các giá trị của BST luôn xuất hiện theo **thứ tự tăng dần (sorted order)**.
  - Phần tử nhỏ nhất luôn nằm ở nút tận cùng bên trái.
- **Phân tích yêu cầu**:
  - $1 \le k \le n \le 10^4$.
  - Mọi giá trị nút là số nguyên không âm.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Duyệt Inorder lưu toàn bộ vào mảng (Brute Force / Baseline)
- **Ý tưởng**:
  - Duyệt Inorder toàn bộ cây nhị phân và lưu giá trị từng nút vào một mảng `sorted_values`.
  - Do tính chất BST, mảng thu được đã được sắp xếp tăng dần.
  - Trả về phần tử tại vị trí thứ `k - 1` (do mảng 0-indexed, $k$ 1-indexed).
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — phải thăm hết toàn bộ $N$ nút trên cây.
  - **Space Complexity**: $O(N)$ — cần mảng để lưu $N$ phần tử + $O(H)$ cho ngăn xếp đệ quy (Call Stack).
- **Hạn chế**: Khi cây rất lớn ($N = 10^6$) mà $k$ rất nhỏ (ví dụ $k = 1$ hoặc $k = 2$), việc duyệt toàn bộ cây và cấp phát mảng lớn là lãng phí bộ nhớ và thời gian.

```python
from typing import Optional

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        sorted_values = []
        
        def inorder(node: Optional[TreeNode]) -> None:
            if not node:
                return
            inorder(node.left)
            sorted_values.append(node.val)
            inorder(node.right)
            
        inorder(root)
        return sorted_values[k - 1]
```

---

### 🔹 Cách 2: Đệ quy Inorder với biến đếm & Dừng sớm (Optimal DFS Early Exit ⭐)
- **Ý tưởng**:
  - Thay vì lưu mảng, ta duy trì biến đếm `k` dùng chung giữa các tầng đệ quy (thông qua `nonlocal`).
  - Mỗi khi kết thúc nhánh trái và quay về nút hiện tại (thời điểm nút được xử lý theo thứ tự tăng dần), ta giảm `k -= 1`.
  - Khi `k == 0`, nút hiện tại chính là đáp án cần tìm. Ta gán `result = node.val`.
  - **Dừng sớm (Early Exit)**: Nếu `result is not None`, lập tức `return` để không duyệt tiếp các nút còn lại.
- **Độ phức tạp**:
  - **Time Complexity**: $O(H + k)$ — trong trường hợp tốt nhất hoặc $k$ nhỏ, ta chỉ cần đi xuống nhánh trái nhất ($O(H)$) và đếm thêm $k$ bước rồi dừng ngay, không cần duyệt hết $N$ nút.
  - **Space Complexity**: $O(H)$ — không gian ngăn xếp đệ quy với $H$ là chiều cao của cây ($O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây lệch). Không tốn mảng $O(N)$.

```python
from typing import Optional

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        result = None
        
        def inorder(node: Optional[TreeNode]) -> None:
            nonlocal k, result
            # Dừng nếu gặp nút rỗng hoặc đã tìm thấy kết quả
            if not node or result is not None:
                return
                
            # 1. Duyệt nhánh trái
            inorder(node.left)
            
            # 2. Xử lý nút hiện tại (khoảnh khắc quay ngược về)
            if result is not None:
                return
                
            k -= 1
            if k == 0:
                result = node.val
                return
                
            # 3. Duyệt nhánh phải
            inorder(node.right)
            
        inorder(root)
        return result
```

---

### 🔹 Cách 3: Dùng Stack khử đệ quy (Iterative Inorder - Phù hợp phỏng vấn sâu)
- **Ý tưởng**:
  - Dùng một ngăn xếp `stack` tường minh để mô phỏng lại quá trình Inorder Traversal.
  - Vòng lặp liên tục đẩy con trỏ sang trái `curr = curr.left` vào `stack`.
  - Khi chạm `None`, `pop` phần tử từ đỉnh `stack` ra (đây chính là phần tử nhỏ tiếp theo).
  - Giảm `k -= 1`. Khi `k == 0`, trả về giá trị ngay lập tức.
  - Chuyển sang nhánh phải `curr = node.right`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(H + k)$
  - **Space Complexity**: $O(H)$

```python
from typing import Optional

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        curr = root
        
        while curr or stack:
            # Đi hết về phía bên trái
            while curr:
                stack.append(curr)
                curr = curr.left
                
            # Lấy phần tử nhỏ nhất hiện tại
            curr = stack.pop()
            k -= 1
            if k == 0:
                return curr.val
                
            # Duyệt sang nhánh phải
            curr = curr.right
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Inorder Traversal của BST = Dãy số có thứ tự**:
   - Bất cứ bài toán nào liên quan đến thứ tự (nhỏ thứ $k$, lớn thứ $k$, kiểm tra hợp lệ, tìm cặp số...) trên BST đều nên nghĩ ngay đến **Inorder Traversal**.
2. **Cơ chế Call Stack trong Đệ quy**:
   - Thời điểm hàm chạy sau dòng `inorder(node.left)` chính là khoảnh khắc đệ quy quay về nút cha. Không cần đặt cờ (flag) để kiểm tra.
3. **Phạm vi biến (Scope) trong Python**:
   - Cần phân biệt biến cục bộ và biến ngoài phạm vi lồng nhau. Sử dụng `nonlocal` giúp chia sẻ trạng thái giữa các tầng đệ quy mà không cần dùng biến toàn cục.
