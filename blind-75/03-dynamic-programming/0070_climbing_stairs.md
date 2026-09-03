# #0070 - Climbing Stairs

- **LeetCode Link**: [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Dynamic Programming (DP), Fibonacci Sequence, Space Optimization

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Có một cầu thang $n$ bậc. Mỗi lần chỉ được bước `1` bậc hoặc `2` bậc. Tính **tổng số cách khác nhau** để leo đến bậc thứ $n$.
- **Trực giác cốt lõi**:
  - Để đứng ở bậc $i$, bước chân cuối cùng của bạn chỉ có thể xuất phát từ:
    - Bậc $i - 1$ (bước thêm 1 bậc).
    - Bậc $i - 2$ (bước thêm 2 bậc).
  - Do đó, số cách đến bậc $i$ chính bằng tổng số cách đến bậc $i - 1$ và bậc $i - 2$:
    $$\text{ways}[i] = \text{ways}[i - 1] + \text{ways}[i - 2]$$
  - Đây chính là dãy số **Fibonacci**: $1, 2, 3, 5, 8, 13, \dots$

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Quy hoạch động dùng Mảng (1D DP Array)
- **Ý tưởng**: Dùng một mảng `dp` kích thước $n + 1$ để lưu số cách leo đến từng bậc từ $0$ đến $n$.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — vòng lặp tính từ $3$ đến $n$.
  - **Space Complexity**: $O(N)$ — mảng lưu $n + 1$ giá trị.

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n]
```

---

### 🔹 Cách 2: Tối ưu bộ nhớ với 2 biến (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Để tính bậc hiện tại, ta chỉ cần kết quả của 2 bậc liền kề trước đó (`prev1` và `prev2`).
  - Dùng 2 con trỏ dịch chuyển dần sang phải để giảm bộ nhớ từ $O(N)$ về $O(1)$.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$.
  - **Space Complexity**: $O(1)$ — chỉ dùng 2 biến lưu trạng thái.

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
            
        prev1, prev2 = 1, 2
        for _ in range(3, n + 1):
            prev1, prev2 = prev2, prev1 + prev2
            
        return prev2
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Bản chất của DP**: Chia bài toán lớn thành các bài toán con không giao nhau. Khi thấy công thức trạng thái chỉ phụ thuộc vào $k$ trạng thái liền trước (ở đây $k = 2$), luôn có thể **tối ưu không gian về $O(1)$** mà không cần lưu cả mảng.
2. **Python Tuple Unpacking**: `prev1, prev2 = prev2, prev1 + prev2` giúp gán song song giá trị mới mà không cần tạo thêm biến tạm `tmp`.
