# #0190 - Reverse Bits

- **LeetCode Link**: [190. Reverse Bits](https://leetcode.com/problems/reverse-bits/)
- **Độ khó (Difficulty)**: Easy
- **Dạng bài (Topic / Pattern)**: Binary, Bit Manipulation, Bit Shifting

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho một số nguyên không dấu 32-bit `n`. Hãy **đảo ngược thứ tự 32 bit** của nó và trả về giá trị số nguyên mới tương ứng.
- **Trực giác "Băng chuyền"**:
  - Tương tự như việc đảo ngược số thập phân (`res * 10 + digit`), trong hệ nhị phân ta liên tục **dịch trái `res` sang 1 vị trí (`res << 1`)** và **ghép bit cuối cùng của `n` (`n & 1`)** vào đuôi.
  - Lặp lại đúng **32 lần** để đảm bảo đảo ngược trọn vẹn cả các bit `0` dẫn đầu (leading zeros).

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách: Dịch bit 32 lần (Optimal - Lời giải của bạn ⭐)
- **Ý tưởng**:
  - Khởi tạo `res = 0`.
  - Chạy vòng lặp đúng 32 lần:
    1. Dịch trái `res` 1 bit để chừa ô trống ở đuôi: `res <<= 1`.
    2. Lấy bit cuối của `n`: `bit = n & 1`.
    3. Thêm bit đó vào đuôi của `res`: `res += bit` (hoặc `res |= bit`).
    4. Dịch phải `n` để chuẩn bị xét bit tiếp theo: `n >>= 1`.
- **Độ phức tạp**:
  - **Time Complexity**: $O(1)$ — vòng lặp luôn chạy đúng 32 lần.
  - **Space Complexity**: $O(1)$ — chỉ dùng biến `res` và `bit`.

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = 0
        for _ in range(32):
            res <<= 1
            bit = n & 1
            res += bit
            n >>= 1
        return res
```

> **💡 Mẹo viết gọn trong 1 dòng vòng lặp:**
> ```python
> class Solution:
>     def reverseBits(self, n: int) -> int:
>         res = 0
>         for _ in range(32):
>             res = (res << 1) | (n & 1)
>             n >>= 1
>         return res
> ```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Ràng buộc độ dài cố định**: Khi đề bài chỉ định rõ kiểu dữ liệu có số bit cố định (như 32-bit integer), **tuyệt đối không dùng `while n > 0`** vì sẽ bỏ sót các bit 0 ở đầu (leading zeros). Luôn dùng vòng lặp cố định `for _ in range(32)`.
2. **Cơ chế đảo bit**:
   - Lấy bit cuối: `n & 1`.
   - Đẩy bit vào kết quả: `(res << 1) | bit`.
   - Bỏ bit cuối: `n >>= 1`.
