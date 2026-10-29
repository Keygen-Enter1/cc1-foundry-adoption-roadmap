# Guardrail 4 điểm và trace cho agent CC1

> Sản phẩm của FDY-0006 #9, #10. Dùng cho agent thí điểm đầu tiên; áp cho mọi agent sau. Đối chiếu khối 5 (Security) và khối 4 (Observability) trong [03-build-agents.md](../03-build-agents.md).

| | |
|---|---|
| **Trạng thái** | Đề xuất 2026-10-29, chờ duyệt. Chưa bật trên resource nào |
| **Nguồn** | Microsoft Learn, kiểm 2026-10-29: [Content filtering for Foundry Models](https://learn.microsoft.com/en-us/azure/foundry-classic/foundry-models/concepts/content-filter) · [Trace and observe AI agents](https://learn.microsoft.com/en-us/azure/foundry-classic/how-to/develop/trace-agents-sdk). Hai trang thuộc Foundry classic portal; cơ chế giống portal mới, giao diện có thể khác |

## 1. Guardrail: 4 điểm chặn

Microsoft khuyên chặn ở 4 điểm: câu hỏi vào → gọi tool → dữ liệu tool trả về → câu trả lời ra. Foundry cung cấp bộ lọc nội dung (content filter) cấu hình ở cấp resource, gắn vào từng model deployment; không áp cho model embedding.

| Điểm | Rủi ro | Foundry có gì | CC1 bật gì |
|---|---|---|---|
| 1. Câu hỏi vào | Nội dung độc hại; prompt attack (bắt agent bỏ luật, đổi vai) | Lọc 4 nhóm hate / sexual / violence / self-harm theo mức low–high; Prompt shields cho user prompt attack; blocklist | Bộ lọc mặc định Microsoft (đang khai báo trong Bicep, `raiPolicyName` trên deployment) · Prompt shields: **chặn** · blocklist tiếng Việt dùng chung (#11, chưa có) |
| 2. Gọi tool | Gọi tool sai ý định; ghi dữ liệu khi không được phép | Task adherence (kiểm tool call có khớp ý định người dùng); luật trong instructions | Tool chỉ đọc ở thí điểm · hành động ghi phải có người duyệt (#6) · instructions phần 3 "Quy tắc dùng tool" |
| 3. Dữ liệu tool trả về | Indirect prompt injection: lệnh xấu giấu trong tài liệu agent đọc | Prompt shields cho indirect attack (cần đánh dấu tài liệu bằng document delimiter, model OpenAI) | Bật cho agent đọc tài liệu (RAG); không cần cho agent chỉ hỏi semantic model |
| 4. Câu trả lời ra | Lộ dữ liệu cá nhân; bịa không có căn cứ; nội dung xấu; chép tài liệu có bản quyền | Lọc 4 nhóm; PII filter; groundedness (chỉ streaming, chỉ 4 vùng Mỹ/Pháp/Canada, **không có Southeast Asia**); protected material | Lọc 4 nhóm mặc định · PII: **chặn** · groundedness không dùng được ở vùng, thay bằng evaluator "có trích nguồn" trong bộ golden (#7) |

Cách làm: tạo một cấu hình bộ lọc dùng chung trên resource mỗi stage (portal: Guardrails + controls → Content filters, hoặc REST `rai-policies`), gắn vào 2 deployment chat; embedding không gắn. Cấu hình này sau sẽ đưa vào Bicep để ba stage giống nhau. Khi bị lọc: câu hỏi bị chặn trả HTTP 400, câu trả lời bị chặn có `finish_reason = content_filter`; agent phải trả lời người dùng bằng câu đã định sẵn thay vì lỗi.

## 2. Trace: Foundry → Application Insights của stage

- Mỗi stage đã có App Insights riêng (Bicep tạo `<foundry>-appinsights`). Việc còn lại là **nối App Insights vào project** (portal: Tracing → connect), khi đó portal có trang Tracing xem từng bước agent: run, tool call, model call; Azure Monitor xem end-to-end.
- Agent viết bằng Microsoft Agent Framework được trace sẵn; declarative agent chạy trong Foundry cũng ghi run và tool call.
- **Ghi nội dung hội thoại** (`AZURE_TRACING_GEN_AI_CONTENT_RECORDING_ENABLED`): mặc định tắt. CC1: bật ở dev để debug, **tắt ở uat/prd** vì nội dung có thể chứa dữ liệu cá nhân; Microsoft cũng khuyên che dữ liệu nhạy cảm và không ghi secret vào thuộc tính trace.
- Trường bắt buộc trên mỗi trace: tên dịch vụ = tên agent (`OTEL_SERVICE_NAME`, truy vấn bằng `cloud_RoleName`), phiên bản agent, stage. Không ghi tên người hỏi, chỉ mã ẩn danh.
- Evaluation tự động trong playground có tính phí và không có ở `japaneast` (vùng DR); Southeast Asia không nằm trong danh sách bị loại trừ.

## Việc cần làm

- [ ] Tạo cấu hình bộ lọc dùng chung ở dev, gắn vào deployment khi bật `deployModels` (FDY-0003 Q4).
- [ ] Nối App Insights vào project `cc1-ai-dev` sau khi chạy `deploy.ps1 -Env dev -Apply`.
- [ ] Kiểm Task adherence và Prompt shields indirect attack có trong portal mới và vùng Southeast Asia không; ghi kết quả kèm ngày.
