# #0121 - Best Time to Buy and Sell Stock

- **LeetCode Link**: [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Array, Greedy, Sliding Window / One-pass

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho mảng `prices` trong đó `prices[i]` là giá cổ phiếu vào ngày thứ `i`. Chọn **một ngày** để mua và **một ngày khác trong tương lai** để bán sao cho **lợi nhuận (profit) lớn nhất**.
- **Điều kiện**: Ngày bán phải diễn ra **sau** ngày mua (`i_sell > i_buy`). Nếu không thể có lãi, trả về `0`.
- **Trực giác cốt lõi**: Để lợi nhuận lớn nhất khi bán ở ngày hiện tại, ta phải mua ở ngày có **giá thấp nhất từng xuất hiện trước đó**.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Vét cạn (Brute Force)
- **Ý tưởng**: Thử tất cả các cặp ngày mua `i` và bán `j` với `i < j`, tính lợi nhuận `prices[j] - prices[i]` và lấy giá trị lớn nhất.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N^2)$ — kiểm tra tất cả $N(N-1)/2$ cặp ngày. Gặp lỗi **Time Limit Exceeded (TLE)** khi $N \le 10^5$.
  - **Space Complexity**: $O(1)$.

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        max_profit = 0
        n = len(prices)
        for i in range(n):
            for j in range(i + 1, n):
                profit = prices[j] - prices[i]
                if profit > max_profit:
                    max_profit = profit
        return max_profit
```

---

### 🔹 Cách 2: Duyệt 1 lần - Greedy (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Duyệt qua mảng giá cổ phiếu từ trái sang phải.
  - Luôn duy trì giá mua thấp nhất đã thấy từ trước đến nay (`min_price`).
  - Tại mỗi ngày, nếu bán ở giá hiện tại thì lãi là `price - min_price`. Ta cập nhật lợi nhuận tối đa (`max_profit`).
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — chỉ duyệt qua mảng đúng 1 lần.
  - **Space Complexity**: $O(1)$ — chỉ dùng 2 biến phụ lưu giá thấp nhất và lãi lớn nhất.

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = float('inf')
        max_profit = 0
        
        for price in prices:
            if price < min_price:
                min_price = price
            else:
                profit = price - min_price
                if profit > max_profit:
                    max_profit = profit
                    
        return max_profit
```

> **💡 Mẹo viết Pythonic & Tối giản (Dùng hàm `min` / `max`):**
> ```python
> class Solution:
>     def maxProfit(self, prices: List[int]) -> int:
>         min_price = float('inf')
>         max_profit = 0
>         
>         for price in prices:
>             min_price = min(min_price, price)
>             max_profit = max(max_profit, price - min_price)
>             
>         return max_profit
> ```

---

## 3. Review & Chuẩn hóa Code
- **Tên biến ban đầu**: `max_price` dùng để lưu lợi nhuận.
- **Chuẩn hóa**: Đổi `max_price` $\rightarrow$ `max_profit` vì biến này chứa **lợi nhuận** cao nhất (profit) chứ không phải giá cổ phiếu (price). Việc đặt đúng tên giúp interviewer đọc code không bị hiểu nhầm mục đích của biến.

---

## 4. Bài học & Mẹo nhớ (Key Takeaways)
1. **Greedy State Tracking**: Khi cần tối ưu hiệu số $A[j] - A[i]$ với $j > i$, chỉ cần duyệt qua $j$ và liên tục theo dõi $\min(A[0 \dots j-1])$.
2. **Khởi tạo giá trị vô cùng**: Khởi tạo `min_price = float('inf')` là pattern chuẩn mực để xử lý phần tử đầu tiên một cách tự nhiên mà không cần check điều kiện biên rỗng mảng.
