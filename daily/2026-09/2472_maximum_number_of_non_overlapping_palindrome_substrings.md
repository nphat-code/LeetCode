# #2472 - Maximum Number of Non-overlapping Palindrome Substrings

- **LeetCode Link**: [2472. Maximum Number of Non-overlapping Palindrome Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings/)
- **Độ khó (Difficulty)**: Hard 🔴
- **Dạng bài (Topic / Pattern)**: String, Two Pointers, Greedy, Interval Scheduling, Dynamic Programming

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho một chuỗi `s` và số nguyên dương `k`. Chọn ra **số lượng lớn nhất các chuỗi con đối xứng (Palindromes) không đè lên nhau** (non-overlapping), trong đó mỗi chuỗi con có độ dài **tối thiểu là `k`** ($\text{length} \ge k$).
- **Ràng buộc**: $1 \le k \le s.\text{length} \le 2000$.
- **Nhận xét mấu chốt (Key Insight - Thu gọn bài toán)**:
  1. **Tính chất đối xứng lồng nhau**:
     - Mọi chuỗi đối xứng có độ dài $L > k + 1$ đều chứa một chuỗi đối xứng con ngắn hơn có độ dài đúng bằng **$k$** hoặc **$k + 1$** ở phần lõi (bằng cách cắt bớt các ký tự đối xứng ở 2 đầu).
  2. **Chiến thuật Tham lam (Greedy / Interval Scheduling)**:
     - Để tối đa hóa số lượng chuỗi con không đè lên nhau, ta luôn ưu tiên chọn chuỗi đối xứng **kết thúc sớm nhất có thể**.
     - Do đó, ta **KHÔNG CẦN** tìm kiếm chuỗi đối xứng có độ dài lớn ($k+2, k+3, \dots$). Ta **CHỈ CẦN** kiểm tra độ dài **$k$** và **$k+1$**.
     - Khi phát hiện chuỗi đối xứng hợp lệ tại vị trí $i$, ta lập tức chọn nó và nhảy con trỏ qua chuỗi đó để tiếp tục tìm kiếm.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Quy hoạch động 1D + Bảng Palindrome 2D (Standard DP - $O(N^2)$)
- **Ý tưởng**:
  - Dùng bảng `is_pal[i][j]` để lưu trạng thái chuỗi con `s[i..j]` có đối xứng hay không ($O(N^2)$ Time, $O(N^2)$ Space).
  - Gọi `dp[i]` là số chuỗi đối xứng tối đa chọn được từ tiền tố `s[0..i]`.
  - Công thức chuyển trạng thái:
    $$dp[i] = \max(dp[i-1], \max_{j \le i - k, \text{is\_pal}[j][i]} (dp[j] + 1))$$
- **Độ phức tạp**:
  - **Time Complexity**: $O(N^2)$
  - **Space Complexity**: $O(N^2)$ — tốn bảng 2D $2000 \times 2000$ booleans.

---

### 🔹 Cách 2: Quét tham lam với kiểm tra độ dài $k$ và $k+1$ (Optimal Greedy ⭐)
- **Ý tưởng**:
  - Dùng con trỏ `i = 0` duyệt qua chuỗi:
    1. Kiểm tra chuỗi con độ dài $k$: `s[i : i + k]`. Nếu là palindrome $\implies$ `count += 1`, nhảy `i += k`.
    2. Nếu không, kiểm tra chuỗi con độ dài $k + 1$: `s[i : i + k + 1]`. Nếu là palindrome $\implies$ `count += 1`, nhảy `i += k + 1`.
    3. Nếu cả hai đều không phải $\implies$ tịnh tiến `i += 1`.
- **Tại sao tối ưu hơn**:
  - Không cần bảng DP $O(N^2)$, tiết kiệm tối đa bộ nhớ.
  - Mỗi bước kiểm tra đối xứng chỉ tốn $O(k)$.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \cdot k)$ — vòng lặp chạy tối đa $N$ lần, mỗi lần kiểm tra chuỗi độ dài $k$ hoặc $k+1$. Với $N \le 2000$, số phép tính tối đa $\approx 4 \times 10^6$ (chạy trong $< 0.05$s).
  - **Space Complexity**: $O(1)$ (hoặc $O(k)$ cho việc cắt chuỗi).

```python
class Solution:
    def maxPalindromes(self, s: str, k: int) -> int:
        def is_palindrome(sub: str) -> bool:
            left, right = 0, len(sub) - 1
            while left < right:
                if sub[left] != sub[right]:
                    return False
                left += 1
                right -= 1
            return True

        n = len(s)
        i = 0
        palindrome_count = 0
        
        while i <= n - k:
            # 1. Ưu tiên chuỗi đối xứng độ dài k (kết thúc sớm nhất)
            if is_palindrome(s[i : i + k]):
                palindrome_count += 1
                i += k
            # 2. Kiểm tra chuỗi đối xứng độ dài k + 1
            elif i + k + 1 <= n and is_palindrome(s[i : i + k + 1]):
                palindrome_count += 1
                i += k + 1
            # 3. Không có chuỗi đối xứng bắt đầu tại i
            else:
                i += 1
                
        return palindrome_count
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Rút gọn không gian tìm kiếm bằng tính chất toán học**:
   - Ở bài này, nếu tìm kiếm mọi độ dài $\ge k$ thì độ phức tạp sẽ rất lớn. Nhận xét *"mọi palindrome dài đều chứa palindrome con độ dài $k$ hoặc $k+1$"* là chìa khóa vàng biến bài toán Hard thành một bài toán quét tuyến tính cực kỳ ngắn gọn.
2. **Nguyên lý Interval Scheduling (Tham lam theo thời điểm kết thúc)**:
   - Khi cần chọn số khoảng không đè nhau nhiều nhất, khoảng nào kết thúc càng sớm thì càng có lợi.
3. **Cẩn trọng cấu trúc điều kiện `if / elif / else`**:
   - Khi kiểm tra điều kiện kèm logic xử lý ở `elif`, luôn đảm bảo các trường hợp không thỏa mãn phải rơi được vào nhánh `else` để tránh lỗi **vòng lặp vô tận (Infinite Loop)**.
