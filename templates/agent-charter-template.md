# Bản mô tả agent (charter) — <tên agent>

Một trang, viết trước khi build, Lead duyệt trong task FDY. Quy trình: [03-build-agents.md](../ai-agent-adoption/03-build-agents.md) Phần C, giai đoạn 2. Mẫu từ FDY-0006.

| Trường | Giá trị |
|---|---|
| **Mã agent** | `agt-NNNN` (NNNN = ID trong catalog 130 agent, vd `agt-0100`); là `name` trên Foundry, không đổi được sau khi tạo |
| **Mô tả ngắn** | Một câu, đặt vào trường description của agent trên Foundry |
| **Tên hiển thị Teams** | CC1 · <tên nghiệp vụ> |
| **Task FDY** | FDY-NNNN |
| **Trưởng dự án** | |
| **Nhóm (bước 1 B1)** | A / B / C · điểm |
| **Đường build (03 B0)** | Declarative / Agent Framework + hosted agent |
| **Lớp dữ liệu cao nhất** | c0 / c1 / c2 / c3 (c3 cần DPIA) |

## 1. Agent làm gì, cho ai

- Việc agent giải quyết (1–2 câu):
- Người dùng (nhóm, số người ước tính):
- Kết quả mong đợi, KPI đo được:

## 2. Dữ liệu và tri thức

| Nguồn | Cách truy xuất | Quyền |
|---|---|---|
| vd. semantic model `sm_bi_models` | Fabric data agent, chỉ đọc | Kế thừa RLS/OLS của người hỏi |

## 3. Được làm và không được làm

- Được:
- Không được (ví dụ: không ghi dữ liệu, không trả lời ngoài phạm vi, không đoán khi không có nguồn):
- Hành động cần người duyệt (human-in-the-loop):

## 4. Tool

| Tool | Đọc / ghi | Ghi chú |
|---|---|---|

## 5. Kiểm thử và phát hành

- Bộ golden: số câu, ai soạn, ai xác nhận đáp án (DA):
- Bộ safety: dùng bộ chung / bổ sung riêng:
- Kênh: Teams / khác:

## 6. Duyệt

| Vai | Tên | Ngày |
|---|---|---|
| Trưởng dự án viết | | |
| Lead duyệt | | |
| DPO (nếu c3) | | |
