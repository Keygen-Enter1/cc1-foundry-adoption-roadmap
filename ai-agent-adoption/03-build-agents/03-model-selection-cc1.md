# Chọn model theo loại tác vụ và Model Router

> Sản phẩm của FDY-0006 #4, #5. Khối 2 (Model) trong [03-build-agents.md](../03-build-agents.md).

| | |
|---|---|
| **Trạng thái** | Đề xuất 2026-10-29, chờ duyệt. Chưa stage nào có deployment |
| **Nguồn** | Microsoft Learn, kiểm 2026-10-29: [Region availability for Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure-region-availability) · [How to use model router](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/model-router) |

## 1. Ba model đã chốt có ở Southeast Asia với kiểu deployment nào

| Model | Global Standard | Data Zone Standard (APAC) | Ghi chú |
|---|---|---|---|
| `gpt-5` (2025-08-07) | **Có** | **Không** (bảng APAC không có dòng gpt-5) | Muốn gpt-5 thì phải Global Standard |
| `gpt-5.4-mini` (2026-03-17) | Có | Có | |
| `text-embedding-3-large` | Có | Có | |
| `model-router` (2025-11-18) | Không ở SEA (chỉ Australia East, South India) | Không ở SEA | Xem mục 3 |

Hệ quả cho FDY-0003 Q4: nếu muốn giữ dữ liệu xử lý trong APAC (Data Zone) thì chỉ có `gpt-5.4-mini` + embedding; `gpt-5` buộc Global Standard (xử lý ở bất kỳ datacenter Azure nào, dữ liệu lưu vẫn ở geography APAC theo trang Learn). Bicep hiện khai báo cả ba là Global Standard, khớp với thực tế vùng.

## 2. Tiêu chí chọn model theo loại tác vụ

Nguyên tắc Microsoft (03 A2 khối 2): model nhỏ cho việc thường, model cao cấp cho suy luận phức tạp; đa dạng model để tránh nghẽn quota; test nhỏ trước khi chốt.

| Loại tác vụ | Model mặc định | Khi nào nâng lên | Chi phí tương đối |
|---|---|---|---|
| Hỏi đáp số liệu qua Fabric data agent (data agent nhóm A) | `gpt-5.4-mini` | Câu hỏi nhiều bước, cần giải thích chênh lệch, so sánh nhiều kỳ → `gpt-5` | thấp |
| Tóm tắt, tra cứu tài liệu (RAG) | `gpt-5.4-mini` | Tài liệu dài, cần tổng hợp nhiều nguồn mâu thuẫn → `gpt-5` | thấp |
| Suy luận nhiều bước, workflow có tool ghi (nhóm B/C) | `gpt-5` | — | cao |
| Judge cho evaluator | `gpt-5` | — | cao, chỉ chạy batch |
| Embedding cho kb / vector store | `text-embedding-3-large` | — | rất thấp |
| Phân loại, trích trường đơn giản theo lịch (nhóm D, không phải agent) | `gpt-5.4-mini` | — | thấp |

Quy tắc:
- Mỗi agent khai báo model trong bản mô tả agent; đổi model là một PR, chạy lại bộ golden, so điểm với bản trước (tụt ≤3 điểm theo cổng chấm).
- Mỗi deployment gắn tag chi phí theo quy ước tag (Tổng quát, mục tag bắt buộc) để tách chi phí theo agent khi nhiều agent dùng chung deployment.
- Ứng viên model mới (`<model>-next`) chỉ deploy ở dev, so sánh bằng cùng bộ golden rồi mới đưa vào danh mục.

## 3. Model Router: không dùng ở giai đoạn này

- Model router là một deployment duy nhất tự chọn model trong pool (gpt-5.x, Claude, Llama...) theo chế độ Balanced / Quality / Cost; tiết kiệm chi phí, trả về tên model đã dùng trong trường `model`.
- **Không có ở Southeast Asia** cho cả Global Standard lẫn Data Zone (chỉ Australia East, South India trong APAC). Dùng được thì phải đặt resource ở vùng khác, lệch quyết định vùng `sea`.
- Pool mặc định gồm model ngoài OpenAI (Claude, Grok, DeepSeek, Llama); muốn kiểm soát phải cấu hình subset, và evaluator/judge phải chấm lại từng model trong subset.
- Kết luận: **chưa dùng**; xem lại khi model-router có ở Southeast Asia hoặc khi số agent đủ lớn để chi phí model thành vấn đề (ghi vào lịch review định kỳ).

## Việc cần làm

- [ ] Chốt FDY-0003 Q4 theo bảng mục 1: Global Standard cho cả ba, hay Data Zone cho `gpt-5.4-mini` + embedding và Global cho `gpt-5`.
- [ ] Sau khi có deployment: chạy bộ golden của thí điểm với `gpt-5.4-mini` và `gpt-5`, ghi điểm và chi phí/câu để xác nhận bảng mục 2.
