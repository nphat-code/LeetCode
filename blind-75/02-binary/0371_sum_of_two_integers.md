# #0371 - Sum of Two Integers

- **LeetCode Link**: [371. Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
- **Độ khó (Difficulty)**: Medium
- **Dạng bài (Topic / Pattern)**: Binary, Bit Manipulation, Half-Adder Simulation

---

## 1. Tóm tắt đề bài & Ý tưởng trực giác (Intuition)
- **Mục tiêu**: Cho hai số nguyên `a` và `b`. Tính và trả về tổng của `a` và `b` **không được sử dụng các toán tử `+` và `-`**.
- **Ràng buộc**: $-1000 \le a, b \le 1000$.
- **Mô phỏng mạch cộng phần cứng (Half-Adder)**:
  Phép cộng 2 số nhị phân được phân rã thành 2 phần:
  1. **Tổng không nhớ (Sum without carry)**: Được tính bởi phép **XOR (`^`)**.
     - `0 ^ 0 = 0`, `0 ^ 1 = 1`, `1 ^ 0 = 1`, `1 ^ 1 = 0`.
  2. **Phần nhớ (Carry)**: Được tính bởi phép **AND (`&`)** kèm theo **dịch trái 1 bit (`<< 1`)**.
     - Chỉ có nhớ khi cả 2 bit đều bằng `1` $\implies$ `a & b`.
     - Phải dịch trái 1 bit `(a & b) << 1` để dồn giá trị nhớ sang cột bên trái kế tiếp (hàng có trọng số lớn gấp đôi).
  3. Lặp lại quá trình cộng giữa tổng không nhớ và phần nhớ cho đến khi **hết phần nhớ (`b == 0`)**.

---

## 2. Các hướng giải quyết (Approaches)

### 🔹 Cách 1: Mô phỏng mạch cộng 32-bit (Optimal $O(1)$ Time & Space ⭐)
- **Ý tưởng**:
  - Trong Python, số nguyên có độ dài vô hạn (Arbitrary Precision). Khi gặp số âm, Python biểu diễn bằng vô số bit 1 ở bên trái. Nếu dịch trái `<< 1`, bit 1 sẽ bành trướng mãi khiến vòng lặp `while b:` không bao giờ dừng.
  - **Mặt nạ 32-bit (`mask = 0xFFFFFFFF`)**:
    - Dùng `mask` gồm 32 bit 1 liên tiếp để "cắt gọt" số về chuẩn 32-bit.
    - Trong vòng lặp:
      ```python
      carry = ((a & b) << 1) & mask
      a = (a ^ b) & mask
      b = carry
      ```
  - **Khôi phục số âm trong Python**:
    - Bit thứ 31 là bit dấu.
    - Nếu `a <= 0x7FFFFFFF`: bit dấu là 0 $\implies$ là số dương, trả về `a`.
    - Nếu `a > 0x7FFFFFFF`: bit dấu là 1 $\implies$ là số âm trong chuẩn bù 2 32-bit. Ta dùng công thức `~(a ^ mask)` để chuyển nó về số âm hợp lệ trong Python.
- **Độ phức tạp**:
  - **Time Complexity**: $O(1)$ — vì tối đa chỉ có 32 bit, vòng lặp chạy tối đa 32 lần.
  - **Space Complexity**: $O(1)$ — không sử dụng cấu trúc dữ liệu phụ.

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        # Mặt nạ 32 bit 1 để giới hạn số trong phạm vi 32-bit
        mask = 0xFFFFFFFF
        
        while b:
            # 1. Tính phần nhớ và dồn sang cột bên trái
            carry = ((a & b) << 1) & mask
            
            # 2. Tính tổng không nhớ
            a = (a ^ b) & mask
            
            # 3. Cập nhật b thành phần nhớ để cộng tiếp
            b = carry
            
        # Nếu bit dấu = 0 (<= 0x7FFFFFFF) -> số dương
        # Nếu bit dấu = 1 (> 0x7FFFFFFF) -> số âm, chuyển về số âm của Python
        return a if a <= 0x7FFFFFFF else ~(a ^ mask)
```

---

## 3. Bài học & Mẹo nhớ (Key Takeaways)
1. **Bản chất phép toán Bit**:
   - `XOR (^)` là phép cộng không nhớ.
   - `AND (&)` là phép tìm các vị trí có nhớ.
   - `Shift left (<< 1)` là phép đẩy nhớ sang hàng tiếp theo có trọng số gấp 2.
2. **Xử lý số nguyên 32-bit trong Python**:
   - Python không giới hạn số bit (không bị tràn số) nên số âm có thể gây lặp vô tận khi shift bit sang trái.
   - Luôn nhớ kỹ thuật dùng mặt nạ `mask = 0xFFFFFFFF` để cắt 32 bit và công thức `~(val ^ mask)` để khôi phục số âm khi làm các bài toán Bit Manipulation mức độ thấp.
