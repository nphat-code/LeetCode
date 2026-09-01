# LeetCode Workspace Rules & Workflow

Quy chuẩn và hành vi tự động cho trợ lý AI khi làm việc trong workspace này:

## 1. Tự động đồng bộ tiến độ vào README.md & Git
Mỗi khi người dùng hoàn thành một bài tập LeetCode hoặc cùng AI giải/tối ưu xong một bài:
1. **Tạo / Cập nhật file lời giải**:
   - Lưu vào đúng thư mục chủ đề tương ứng (ví dụ: `blind-75/01-array/0001_two_sum.md`).
   - Cấu trúc đầy đủ: Tóm tắt đề $\rightarrow$ Brute Force ($O(N^2)$) $\rightarrow$ Optimal ($O(N)$) $\rightarrow$ Time/Space Complexity $\rightarrow$ Key Takeaways.
2. **Cập nhật Checklist trong `blind-75/README.md` (hoặc track tương ứng)**:
   - Đánh dấu `[x]` vào ô checkbox của bài đó.
   - Cập nhật thông tin Pattern / Kỹ thuật chính và ghi chú ngắn gọn nếu cần.
   - Tự động tính toán lại và cập nhật:
     - Số lượng bài đã giải: `Tổng số bài (X / 75)`, `Easy (x/20)`, `Medium (y/48)`, `Hard (z/7)`.
     - Thanh Progress bar: `[████░░░░░░] P% Completed`.
3. **Cập nhật tổng quan ở root `README.md`** và **tự động Git commit & push** lên GitHub.

## 2. Phương pháp Hướng dẫn & Coaching (Socratic Mentoring)
- **KHÔNG đưa code lời giải ngay khi người dùng làm sai**:
  - Nếu code sai logic hoặc miss trường hợp biên (edge cases), hãy **đưa ra test case phản ví dụ (counter-example)** để người dùng tự trace.
  - Đặt các câu hỏi gợi mở, gợi ý pattern/cấu trúc dữ liệu phù hợp để người học tự tư duy và tìm ra giải pháp.
  - Chỉ cung cấp lời giải đầy đủ khi người dùng yêu cầu hoặc đã tự sửa thành công.

## 3. Tối ưu Clean Code & Đặt tên biến chuẩn Phỏng vấn (Variable Naming)
- Khi người dùng gửi code, AI sẽ **chủ động review và chuẩn hóa tên biến**:
  - Đổi tên biến chung chung (`temp`, `a`, `b`, `x`, `dict1`...) thành tên biến mang ý nghĩa thuật toán rõ ràng (ví dụ: `complement`, `seen`, `visited`, `window_start`, `prev_node`, `max_profit`...).
  - Giải thích ngắn gọn lý do tại sao tên biến mới giúp code dễ đọc và tạo ấn tượng tốt hơn với interviewer.
