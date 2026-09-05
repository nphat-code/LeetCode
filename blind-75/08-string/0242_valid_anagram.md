# #0242 - Valid Anagram

- **LeetCode Link**: [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: String, Hash Table, Frequency Counting

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho 2 chuỗi `s` và `t`. Xác định xem `t` có phải là chuỗi đảo chữ (Anagram) của `s` hay không.
- **Trực giác cốt lõi**:
  - Hai chuỗi là Anagram khi và chỉ khi chúng chứa **chính xác các ký tự giống nhau** với **cùng số lần xuất hiện (tần suất)**.
  - Trường hợp biên: Nếu `len(s) != len(t)` $\rightarrow$ Chắc chắn không phải Anagram, trả về `False` ngay lập tức.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Đếm tần suất bằng 1 Bảng Băm (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  1. Kiểm tra nếu độ dài 2 chuỗi khác nhau $\rightarrow$ `return False`.
  2. Duyệt qua `s`: tăng số đếm của từng ký tự `count[char] += 1`.
  3. Duyệt qua `t`: nếu ký tự không có trong bảng hoặc số đếm đã về 0 $\rightarrow$ `return False`. Ngược lại giảm số đếm `count[char] -= 1`.
  4. Nếu duyệt hết `t` an toàn $\rightarrow$ `return True`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$ — với $N$ là độ dài chuỗi.
  - **Space Complexity**: $O(1)$ — bảng băm chỉ lưu tối đa 26 chữ cái tiếng Anh thường.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
            
        count = {}
        for char in s:
            count[char] = count.get(char, 0) + 1
            
        for char in t:
            if char not in count or count[char] == 0:
                return False
            count[char] -= 1
            
        return True
```

---

### 🔹 Cách 2: Mảng đếm tần suất 26 phần tử (Tối ưu bộ nhớ thuần túy)
- **Ý tưởng**: Dùng mảng `[0] * 26` thay cho bảng băm. Vị trí của mỗi chữ cái được tính bằng mã ASCII: `ord(char) - ord('a')`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N)$
  - **Space Complexity**: $O(1)$ — đúng 26 số nguyên.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
            
        freq = [0] * 26
        for i in range(len(s)):
            freq[ord(s[i]) - ord('a')] += 1
            freq[ord(t[i]) - ord('a')] -= 1
            
        return all(x == 0 for x in freq)
```

---

### 🔹 Cách 3: Sắp xếp (Sorting - Python One-liner)
- **Ý tưởng**: Nếu là Anagram thì sau khi sắp xếp lại, 2 chuỗi phải giống hệt nhau: `sorted(s) == sorted(t)`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(N \log N)$ — do thao tác sort.
  - **Space Complexity**: $O(N)$ — tạo chuỗi/danh sách mới sau khi sort.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **`dict.get(key, default)`**: Dùng `count.get(char, 0) + 1` là pattern kinh điển trong Python để đếm tần suất mà không cần viết `if char in count: ... else: ...`.
2. **Kỹ thuật Early Return khi trừ tần suất**: Ở vòng lặp thứ hai với chuỗi `t`, nếu gặp ký tự chưa có hoặc `count[char] == 0` thì trả về `False` ngay, không cần phải chạy thêm vòng lặp thứ 3 để kiểm tra lại `count`.
