# #0102 - Binary Tree Level Order Traversal

- **LeetCode Link**: [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- **Độ khó (Difficulty)**: Medium
- **Dạng bài (Topic / Pattern)**: Tree, Breadth-First Search (BFS), Queue

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho con trỏ `root` của một cây nhị phân, hãy trả về danh sách các giá trị nút được nhóm theo từng tầng (từ trái qua phải, từ tầng trên xuống tầng dưới).
- **Trực giác cốt lõi**:
  - Để duyệt theo từng tầng, cấu trúc dữ liệu tự nhiên nhất là **Hàng đợi (Queue - FIFO)**.
  - Điểm mấu chốt để phân tách các tầng: Tại mỗi vòng lặp chính của hàng đợi, số lượng phần tử `len(queue)` hiện có đại diện cho **chính xác toàn bộ các nút của tầng hiện tại**.
  - Bằng cách dùng vòng lặp `for _ in range(len(queue))`, ta xử lý cạn kiệt tất cả các nút của tầng đó, gom giá trị của chúng vào `current_level = []`, đồng thời đẩy các con thế hệ tiếp theo vào đuôi hàng đợi cho tầng kế tiếp.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: BFS Duyệt theo tầng với Queue (Optimal & Chuẩn mực nhất ⭐)
- **Ý tưởng**:
  - Dùng `collections.deque` để đảm bảo thao tác `popleft()` đạt độ phức tạp $O(1)$.
  - Xử lý biên (Edge case): nếu `not root` thì trả về danh sách rỗng `[]`.
  - Vòng lặp `while queue:` tiếp tục cho đến khi không còn nút nào.
  - Tại mỗi tầng, khởi tạo `current_level = []`, lấy lần lượt từng nút ra khỏi queue, đưa `node.val` vào `current_level`, sau đó thêm con trái và con phải vào queue nếu tồn tại.
  - Sau khi kết thúc vòng `for`, đẩy `current_level` vào `result`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — mỗi nút trong cây được đưa vào và lấy ra khỏi queue đúng 1 lần.
  - **Space Complexity**: $O(N)$ — trong trường hợp xấu nhất (cây nhị phân hoàn chỉnh), tầng đáy chứa khoảng $N/2$ nút, queue chiếm tối đa $O(N)$ bộ nhớ. Danh sách kết quả `result` lưu đủ $N$ phần tử.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque
from typing import Optional, List

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # Xử lý trường hợp cây rỗng
        if not root:
            return []
            
        result = []
        queue = deque([root])
        
        while queue:
            current_level = []
            level_size = len(queue)
            
            # Duyệt qua toàn bộ các nút thuộc riêng tầng này
            for _ in range(level_size):
                node = queue.popleft()
                current_level.append(node.val)
                
                # Đẩy các nút con vào hàng đợi cho tầng tiếp theo
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            # Thêm danh sách giá trị của tầng vừa hoàn thành vào kết quả chung
            result.append(current_level)
            
        return result
```

---

### 🔹 Cách 2: DFS Đệ quy kèm theo tham số độ sâu (Recursive DFS)
- **Ý tưởng**:
  - Ta vẫn có thể dùng đệ quy DFS (Preorder) bằng cách truyền thêm biến `depth` (hoặc `level`, bắt đầu từ 0).
  - Khi đệ quy đến một nút ở độ sâu `depth`:
    - Nếu `len(result) == depth`: Nghĩa là ta lần đầu tiên đặt chân đến tầng này $\implies$ tạo thêm một mảng rỗng `result.append([])`.
    - Sau đó: `result[depth].append(node.val)`.
    - Gọi đệ quy cho con trái `dfs(node.left, depth + 1)` rồi con phải `dfs(node.right, depth + 1)`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$
  - **Space Complexity**: $O(H)$ cho Call Stack ($O(\log N)$ nếu cây cân bằng, $O(N)$ nếu cây suy biến).

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        result = []
        
        def dfs(node: Optional[TreeNode], depth: int) -> None:
            if not node:
                return
            # Nếu chưa có danh sách con cho tầng này, tạo mới
            if len(result) == depth:
                result.append([])
            result[depth].append(node.val)
            
            dfs(node.left, depth + 1)
            dfs(node.right, depth + 1)
            
        dfs(root, 0)
        return result
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Dùng `list` thông thường làm queue thay vì `collections.deque`**:
   - Trong Python, thao tác `list.pop(0)` tốn thời gian $O(K)$ do phải dịch chuyển tất cả phần tử còn lại, dẫn đến toàn bộ thuật toán bị đội lên $O(N^2)$ (dễ bị Time Limit Exceeded). Bắt buộc dùng `deque` với `popleft()` đạt $O(1)$.
2. **Khởi tạo mảng tĩnh cố định**:
   - Không nên dùng `[[] * 2000]` vì trong Python cú pháp này không tạo ra các mảng con lồng nhau, và ta cũng không thể đoán trước chiều cao thực tế của cây. Hãy dùng dynamic list `result.append(current_level)`.
3. **Quên lưu `len(queue)` hoặc tính toán sai trong vòng lặp**:
   - Nếu trong vòng `for` không cố định số lượng phần tử của tầng đó mà vừa pop vừa append làm thay đổi điều kiện dừng không kiểm soát, thuật toán sẽ bị sai lệch cấu trúc tầng.
