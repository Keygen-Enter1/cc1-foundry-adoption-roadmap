# Quy tắc người duyệt (human-in-the-loop) cho tool ghi

> Sản phẩm của FDY-0006 #6. Áp dụng từ agent thí điểm đầu tiên vì thí điểm có tool ghi (user xác nhận 2026-10-29). Khối 3 (knowledge, tools, memory) trong [03-build-agents.md](../03-build-agents.md).

| | |
|---|---|
| **Trạng thái** | Đề xuất 2026-10-29, chờ duyệt |
| **Nguyên tắc Microsoft** | Hành động tác động lớn (ghi DB, giao dịch) phải có người duyệt; tool đi qua API hẹp, không truy cập trực tiếp; mọi tool call có log gắn agent và hội thoại (03 A2 khối 3) |

## 1. Phân loại hành động của tool

| Loại | Ví dụ | Agent được làm |
|---|---|---|
| Đọc | Truy vấn semantic model, đọc đơn hàng Odoo | Tự làm, không hỏi |
| Ghi thuận nghịch, phạm vi nhỏ | Tạo bản nháp, thêm ghi chú, cập nhật trạng thái của chính người hỏi | Làm sau khi người hỏi xác nhận "đồng ý" trong cùng hội thoại |
| Ghi khó đảo, ảnh hưởng người khác hoặc tiền | Duyệt đề nghị, tạo đơn mua, đổi số lượng/giá, xoá | Chỉ **đề xuất**; người có thẩm quyền duyệt ở ngoài agent (Teams approval hoặc trong Odoo), agent không tự gọi tool |
| Cấm | Phân quyền, dữ liệu lớp c3+, lương, hợp đồng | Không có tool; instructions phần 2 ghi "Never" |

Mỗi tool ghi khai báo loại của mình trong bản mô tả agent (mục 4 Tool) trước khi build.

## 2. Cách xác nhận trong hội thoại (loại "ghi thuận nghịch")

1. Agent hiện đúng nội dung sẽ ghi: bản ghi nào, trường nào, giá trị cũ → mới.
2. Người hỏi trả lời "đồng ý". Câu khác, không trả lời, hoặc hội thoại khác → không ghi.
3. Agent gọi tool, rồi báo kết quả kèm mã bản ghi để người dùng kiểm.
4. Một lần xác nhận chỉ cho một hành động; không xác nhận trước cho nhiều lần.

Quy tắc này nằm trong instructions phần 2 và 3 (mẫu đã có câu sẵn).

## 3. Cách duyệt ngoài agent (loại "ghi khó đảo")

- Agent tạo đề xuất kèm lý do và dữ liệu nguồn; gửi cho người duyệt qua kênh có sẵn (Teams, hoặc luồng duyệt trong Odoo).
- Người duyệt là người có thẩm quyền theo quy trình nghiệp vụ hiện tại, không phải trưởng dự án agent.
- Agent không giữ quyền gọi tool ghi loại này: identity của agent chỉ được cấp quyền đọc và quyền tạo đề xuất.

## 4. Log và kiểm tra

- Mọi tool call ghi vào trace (xem [guardrail-trace-cc1.md](03-guardrail-trace-cc1.md)): tên agent, phiên bản, mã hội thoại, tool, tham số, kết quả, có xác nhận hay không. Không ghi nội dung dữ liệu cá nhân.
- Bộ golden có ít nhất 5 câu thử tool ghi: 2 câu phải hỏi xác nhận, 2 câu phải từ chối (ngoài phạm vi), 1 câu có xác nhận thì ghi đúng.
- Khi ghi sai: rollback bằng tay theo quy trình nghiệp vụ; agent không tự sửa.

## 5. Áp cho thí điểm

| Việc | Ai |
|---|---|
| Liệt kê tool ghi của thí điểm và xếp loại theo bảng 1 | Trưởng dự án, Lead duyệt |
| Cấp quyền cho identity agent đúng theo loại | Lead |
| Viết câu xác nhận vào instructions, thêm 5 câu golden | Thành viên dự án |

## Việc cần làm

- [ ] Liệt kê tool ghi của agent thí điểm (MCP Odoo: hành động nào) và xếp loại.
- [ ] Kiểm Foundry có cơ chế duyệt tool call có sẵn cho declarative agent không (kèm link Learn, ngày kiểm); nếu có thì dùng thay cách xác nhận trong hội thoại.
