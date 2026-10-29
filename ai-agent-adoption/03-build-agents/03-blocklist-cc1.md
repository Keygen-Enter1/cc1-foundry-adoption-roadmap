# Blocklist tiếng Việt dùng chung

> Sản phẩm của FDY-0006 #11. Điểm chặn 1 và 4 trong [guardrail-trace-cc1.md](03-guardrail-trace-cc1.md).

| | |
|---|---|
| **Trạng thái** | Đề xuất 2026-10-29, chờ duyệt. Chưa tạo trên resource nào |
| **Nguồn** | Microsoft Learn, kiểm 2026-10-29: [Use blocklists](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/use-blocklists) |

## Foundry cung cấp gì

- Blocklist là tài nguyên con của Foundry resource (`raiBlocklists`), gắn vào cấu hình bộ lọc (`raiPolicies`) ở phía câu hỏi (`promptBlocklists`), câu trả lời (`completionBlocklists`) hoặc cả hai; tạo bằng portal (Guardrails → Blocklists, nhập tay hoặc tải CSV) hoặc REST ARM, nghĩa là **đưa được vào Bicep** cùng bộ lọc.
- Mỗi list tối đa 10.000 mục, mỗi mục tối đa 1.000 ký tự, khớp chính xác hoặc regex (phân biệt hoa thường); có sẵn list profanity của Microsoft; mục mới có hiệu lực sau khoảng 5 phút. Chỉ áp cho model Azure OpenAI.
- Khi khớp: câu hỏi bị chặn trả HTTP 400 `content_filter`; câu trả lời bị cắt với `finish_reason = content_filter` và chú thích tên list.

## CC1 dùng thế nào

| List | Nội dung | Phía | Ai giữ |
|---|---|---|---|
| Profanity có sẵn của Microsoft | Tục tĩu tiếng Anh | Cả hai | Microsoft |
| Tục tĩu, xúc phạm tiếng Việt | Từ và biến thể viết tắt, không dấu | Cả hai | Lead; thành viên dự án đề xuất |
| Dữ liệu nhạy cảm theo mẫu | Regex: số CCCD 12 số, số tài khoản ngân hàng, số điện thoại 10 số, email cá nhân | Câu trả lời | Lead; DPO duyệt |
| Chủ đề cấm theo nghiệp vụ | Từ khoá lương, hợp đồng lao động, kỷ luật, thông tin đối thủ (danh sách do nghiệp vụ đưa) | Câu hỏi | Trưởng dự án đề xuất, Lead duyệt |

Quy tắc:
- Một bộ list cho cả ba stage, nằm trong Git dưới dạng CSV, đưa lên bằng Bicep cùng bộ lọc; sửa list = PR, có hiệu lực sau deploy.
- Blocklist chỉ chặn từ khoá; không thay được bộ lọc nội dung và instructions. Dùng cho thứ bộ lọc Microsoft không biết (tiếng Việt, tên nội bộ).
- Mỗi lần có sự cố nội dung (runbook sự cố guardrail trong Sổ tay) xem xét thêm mục vào list.
- Kiểm chặn nhầm: bộ benign trong evaluator catalog phải đi qua blocklist không bị chặn; regex thử trước trên dữ liệu mẫu.

## Việc cần làm

- [ ] Soạn CSV ba list CC1 (tục tĩu tiếng Việt, regex dữ liệu nhạy cảm, chủ đề cấm) — thành viên dự án; DPO duyệt list regex.
- [ ] Thêm `raiBlocklists` + `raiPolicies` vào `infra/modules/stage.bicep`, kiểm API version hỗ trợ (what-if).
- [ ] Thử trên dev với 20 câu benign và 20 câu phải chặn; ghi tỷ lệ chặn nhầm.
