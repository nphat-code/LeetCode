# #0191 - Number of 1 Bits

- **LeetCode Link**: [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Binary, Bit Manipulation, Brian Kernighan's Algorithm

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho một số nguyên dương `n`. Đếm và trả về số lượng bit `1` (set bits / Hamming weight) trong biểu diễn nhị phân của `n`.
- **Ví dụ**: `n = 11` (nhị phân `...1011`) $\rightarrow$ Output: `3`.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Chuyển đổi sang chuỗi (String Conversion)
- **Ý tưởng**: Chuyển số `n` thành chuỗi nhị phân qua `bin(n)` rồi duyệt/đếm ký tự `'1'`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(k)$ với $k \le 32$ là số lượng bit $\rightarrow O(1)$.
  - **Space Complexity**: $O(k) \rightarrow O(1)$ do cần tạo chuỗi.

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        return bin(n).count("1")
```

---

### 🔹 Cách 2: Dịch bit & Kiểm tra bit cuối (Bit Shifting)
- **Ý tưởng**: Dùng `n & 1` để kiểm tra bit cuối (LSB), nếu là `1` thì tăng biến đếm, sau đó dịch phải `n = n >> 1`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(k)$ — luôn lặp đúng số bit của $n$ (tối đa 32 lần).
  - **Space Complexity**: $O(1)$ — không tốn thêm bộ nhớ.

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n > 0:
            if n & 1:
                count += 1
            n >>= 1
        return count
```

---

### 🔹 Cách 3: Thuật toán Brian Kernighan (Optimal - Tối ưu số vòng lặp ⭐)
- **Ý tưởng**:
  - Phép toán `n & (n - 1)` luôn **xóa bỏ bit 1 bên phải nhất** của `n`.
  - Vòng lặp `while n > 0` chỉ chạy đúng bằng **số lượng bit 1** thực tế trong `n` (bỏ qua toàn bộ các bit 0 mà không cần duyệt).
- **Độ phức tạp**:
  - **Time Complexity**: $O(\text{số bit 1}) \le 32 \rightarrow O(1)$ — trường hợp tốt nhất chỉ chạy 1 lần nếu $n = 2^k$.
  - **Space Complexity**: $O(1)$.

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n > 0:
            count += 1
            n = n & (n - 1)
        return count
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **`n & 1`**: Kiểm tra bit cuối cùng của `n` là 0 hay 1 (kiểm tra tính chẵn/lẻ).
2. **`n >> 1`**: Dịch bỏ bit cuối cùng (tương đương chia nguyên cho 2).
3. **`n & (n - 1)`**: "Vũ khí bí mật" để xóa bit 1 thấp nhất. Rất hay dùng trong các bài kiểm tra số mũ của 2 (`isPowerOfTwo`), đếm bit 1, hoặc giải bài tập quy hoạch động trên bit.
