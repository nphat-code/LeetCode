# #0125 - Valid Palindrome

- **LeetCode Link**: [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: String, Two Pointers

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Xác định xem chuỗi `s` có phải là chuỗi đối xứng (palindrome) hay không, sau khi đã loại bỏ toàn bộ ký tự không phải chữ cái/chữ số và chuyển hết thành chữ thường.
- **Trực giác cốt lõi**:
  - Đối xứng nghĩa là đọc từ trái qua phải hay từ phải qua trái đều giống hệt nhau.
  - Ta có thể đặt 2 con trỏ ở 2 đầu chuỗi (`left` ở đầu và `right` ở cuối), lần lượt di chuyển vào giữa để so sánh từng cặp ký tự hợp lệ.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Lọc ký tự hợp lệ vào mảng phụ rồi dùng Two Pointers
- **Ý tưởng**:
  1. Duyệt qua chuỗi `s`, nếu ký tự là chữ/số (`char.isalnum()`) thì thêm dạng viết thường (`char.lower()`) vào một mảng `cleaned_chars`.
  2. Dùng 2 con trỏ `left = 0`, `right = len(cleaned_chars) - 1` chạy vào giữa để so sánh: nếu gặp cặp ký tự khác nhau thì trả về `False`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — duyệt qua chuỗi 1 lần để lọc và duyệt mảng đã lọc 1 lần.
  - **Space Complexity**: $O(N)$ — tốn bộ nhớ lưu danh sách các ký tự đã lọc.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        cleaned_chars = []
        for char in s:
            if char.isalnum():
                cleaned_chars.append(char.lower())
                
        left = 0
        right = len(cleaned_chars) - 1
        while left < right:
            if cleaned_chars[left] != cleaned_chars[right]:
                return False
            left += 1
            right -= 1
            
        return True
```

---

### 🔹 Cách 2: Two Pointers In-place trực tiếp trên chuỗi gốc (Optimal ⭐)
- **Ý tưởng**:
  - Tối ưu bộ nhớ về $O(1)$ bằng cách không tạo bất kỳ chuỗi/mảng phụ nào.
  - Đặt `left = 0`, `right = len(s) - 1` ngay trên chuỗi `s`:
    - Nếu `s[left]` không phải chữ/số $\rightarrow$ bỏ qua: `left += 1`.
    - Nếu `s[right]` không phải chữ/số $\rightarrow$ bỏ qua: `right -= 1`.
    - Nếu cả hai đều là chữ/số $\rightarrow$ so sánh chữ thường `s[left].lower() != s[right].lower()`. Nếu khác nhau $\rightarrow$ `return False`. Nếu giống nhau $\rightarrow$ thu hẹp cả hai con trỏ: `left += 1`, `right -= 1`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — mỗi ký tự được 2 con trỏ ghé thăm tối đa 1 lần.
  - **Space Complexity**: $O(1)$ — chỉ sử dụng đúng 2 biến con trỏ.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1
        
        while left < right:
            if not s[left].isalnum():
                left += 1
            elif not s[right].isalnum():
                right -= 1
            elif s[left].lower() != s[right].lower():
                return False
            else:
                left += 1
                right -= 1
                
        return True
```

---

## 3. Các bẫy thường gặp (Common Pitfalls)
1. **Quên kiểm tra cả số lẫn chữ**: Nhiều bạn chỉ kiểm tra chữ cái (`isalpha()`) mà quên mất đề bài yêu cầu cả chữ số (`0-9`) $\rightarrow$ Dùng `isalnum()`.
2. **Không phân biệt hoa thường**: Phải đồng nhất về chữ thường bằng `.lower()` trước khi so sánh.
3. **Chuỗi chỉ toàn ký tự đặc biệt hoặc khoảng trắng**: Ví dụ `s = "   "` hoặc `s = ",,, "` $\rightarrow$ Hai con trỏ sẽ lướt qua nhau mà không báo lỗi, kết quả trả về `True` (chuỗi rỗng là một palindrome hợp lệ).
