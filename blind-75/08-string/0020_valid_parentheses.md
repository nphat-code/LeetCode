# #0020 - Valid Parentheses

- **LeetCode Link**: [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: String, Stack, Hash Table

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho một chuỗi `s` gồm các ký tự `'('`, `')'`, `'{'`, `'}'`, `'['`, `']'`. Xác định xem chuỗi ngoặc có hợp lệ hay không (mỗi dấu mở phải đóng đúng loại và đúng thứ tự).
- **Trực giác cốt lõi**:
  - Dấu ngoặc mở **sau cùng** phải là dấu được **đóng đầu tiên** $\rightarrow$ Đây chính là cơ chế **LIFO (Last In, First Out)** của cấu trúc dữ liệu **Stack (Ngăn xếp)**.
  - Khi gặp dấu mở: Đẩy vào Stack.
  - Khi gặp dấu đóng: Lấy dấu mở ở đỉnh Stack ra để so sánh xem có khớp cặp hay không.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách: Dùng Stack & Hash Table (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  1. Dùng một bảng băm `matching = {')': '(', '}': '{', ']': '['}` ánh xạ dấu đóng sang dấu mở tương ứng.
  2. Duyệt qua từng ký tự trong chuỗi:
     - Nếu là dấu đóng (`char in matching`):
       - Nếu stack đang rỗng (không có dấu mở) hoặc dấu ở đỉnh stack không khớp (`stack[-1] != matching[char]`) $\rightarrow$ `return False`.
       - Nếu khớp $\rightarrow$ `stack.pop()`.
     - Nếu là dấu mở $\rightarrow$ `stack.append(char)`.
  3. Cuối cùng, nếu stack rỗng hoàn toàn (`not stack`) $\rightarrow$ `True`, nếu còn sót dấu mở $\rightarrow$ `False`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — duyệt qua chuỗi đúng 1 lần, mỗi thao tác `push`/`pop` mất $O(1)$.
  - **Space Complexity**: $O(N)$ — trong trường hợp xấu nhất (chuỗi toàn dấu mở như `"(((("`), stack lưu tối đa $N$ ký tự.

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        matching = {')': '(', '}': '{', ']': '['}
        
        for char in s:
            if char in matching:
                if not stack or stack[-1] != matching[char]:
                    return False
                stack.pop()
            else:
                stack.append(char)
                
        return not stack
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Quên kiểm tra Stack rỗng khi gặp dấu đóng**: Ví dụ `s = ")"` hoặc `s = "])"` $\rightarrow$ Cần check `if not stack: return False` trước khi truy cập `stack[-1]` để tránh lỗi `IndexError`.
2. **Quên kiểm tra Stack còn dư ở cuối**: Ví dụ `s = "(("` $\rightarrow$ Mặc dù không gặp dấu đóng nào sai, nhưng kết thúc chuỗi stack vẫn còn tồn đọng dấu mở $\rightarrow$ phải trả về `not stack` (tức là `False`).
3. **Mẹo Pythonic**:
   - Truy cập đỉnh stack: `stack[-1]` thay vì `stack[len(stack) - 1]`.
   - Kiểm tra rỗng: `if not stack:` thay vì `if len(stack) == 0:`.

---

## 4. Bài học & Mẹo nhớ (Key Takeaways)
- **Stack Pattern**: Bất kỳ bài toán nào liên quan đến **cặp ngoặc (parentheses)**, **thứ tự lồng nhau (nesting)**, hoặc **phần tử gần nhất thỏa mãn điều kiện (Monotonic Stack)** đều nên nghĩ ngay đến **Stack**.
