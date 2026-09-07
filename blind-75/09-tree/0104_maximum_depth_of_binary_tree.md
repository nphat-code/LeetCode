# #0104 - Maximum Depth of Binary Tree

- **LeetCode Link**: [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Tree, Depth-First Search (DFS), Breadth-First Search (BFS)

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Tìm chiều sâu tối đa (số lượng nút trên đường đi dài nhất từ `root` đến một nút lá bất kỳ) của một cây nhị phân.
- **Trực giác**:
  - **DFS (Đệ quy - Postorder)**: Chiều sâu tại nút hiện tại chính là $1 + \max(\text{chiều sâu con trái}, \text{chiều sâu con phải})$.
  - **BFS (Hàng đợi - Level Order)**: Duyệt từng tầng của cây bằng một `Queue`. Mỗi khi duyệt hết tất cả các nút ở tầng hiện tại, tăng `depth` lên $1$.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: DFS Đệ quy (Optimal & Ngắn gọn nhất ⭐)
- **Ý tưởng**:
  1. **Base Case**: Nếu nút hiện tại là rỗng (`not root`) $\rightarrow$ chiều sâu là `0`.
  2. **Recursive Step**: Gọi đệ quy tính chiều sâu cây con trái và phải, lấy giá trị lớn hơn cộng thêm `1` cho nút hiện tại.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — ghé thăm mỗi nút trong cây đúng 1 lần.
  - **Space Complexity**:
    - Cây cân bằng (Balanced Tree): $O(\log N)$ do chiều cao Call Stack là $\log_2(N)$.
    - Cây lệch (Skewed Tree): $O(N)$ trong trường hợp xấu nhất cây suy biến thành Linked List.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

---

### 🔹 Cách 2: BFS Duyệt theo tầng với Queue (Level Order Traversal ⭐)
- **Ý tưởng**:
  - Dùng `collections.deque` để lưu trữ các nút theo cơ chế FIFO.
  - Ban đầu đẩy `root` vào queue (nếu `root` tồn tại).
  - Khi queue còn phần tử, mỗi vòng `while` tương ứng với 1 tầng:
    - Tăng `depth += 1`.
    - Dùng `for _ in range(len(queue)):` để snapshot số lượng nút của tầng hiện tại, lần lượt rút ra bằng `popleft()` và đẩy các con hợp lệ (`node.left`, `node.right`) vào đuôi queue cho tầng kế tiếp.
  - Trả về `depth` khi duyệt xong toàn bộ cây.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — mỗi nút được `append` và `popleft` đúng 1 lần.
  - **Space Complexity**: $O(W)$ — với $W$ là độ rộng tối đa của cây (tầng chứa nhiều nút nhất). Trong cây nhị phân đầy đủ, tầng lá cuối cùng có thể chứa tới $\approx N / 2$ nút $\rightarrow O(N)$.

```python
from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
            
        queue = deque([root])
        depth = 0
        
        while queue:
            depth += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
        return depth
```

---

## 3. So sánh DFS vs BFS trong Phỏng vấn
| Tiêu chí | DFS (Đệ quy) | BFS (Level Order với Queue) |
| :--- | :--- | :--- |
| **Độ dài code** | Cực kỳ ngắn gọn (3 dòng) | Dài hơn, cần import `deque` |
| **Bộ nhớ (Space)** | $O(H)$ — tốt khi cây bẹt/rộng ($H \ll W$) | $O(W)$ — tốt khi cây sâu/lệch ($W \ll H$) |
| **Rủi ro thực tế** | Có thể bị **Stack Overflow** nếu cây quá sâu ($> 1000$ tầng trong Python) | An toàn, không sợ tràn stack do dùng vùng nhớ Heap |

---

## 4. Các bẫy thường gặp (Common Pitfalls)
1. **Quên kiểm tra cây rỗng (`root is None`)**: Trong BFS, nếu cho `None` vào queue thì `while queue` sẽ chạy và ném lỗi `AttributeError` khi truy cập `.left`.
2. **Dùng `pop()` thay vì `popleft()`**: Trong Python, `queue.pop()` rút ở đuôi (LIFO - ngăn xếp). Queue bắt buộc phải dùng `popleft()` để rút ở đầu (FIFO).
3. **Hiểu lầm về `len(queue)` trong vòng `for`**: Trong Python, `range(len(queue))` đánh giá kích thước queue một lần duy nhất tại thời điểm bắt đầu vòng lặp, giúp tách biệt chính xác từng tầng dù ta có `append` thêm con vào queue.
