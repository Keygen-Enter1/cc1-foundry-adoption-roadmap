# Lộ trình triển khai AI agent tại CC1 · theo Microsoft Cloud Adoption Framework

> Khung 4 bước của Microsoft, áp vào CC1. Mỗi bước là một tài liệu riêng: phần đầu tóm tắt khuyến nghị của Microsoft, phần sau là cách làm cụ thể ở CC1 (đang nghiên cứu, bổ sung dần).

| | |
|---|---|
| **Phiên bản** | v0.1 — khung, chưa có nội dung CC1 |
| **Ngày** | 2026-10-28 |
| **Tác giả** | Trần Trọng Nghĩa — Data Team CC1 |
| **Trạng thái** | Draft — khung 4 bước; từng bước sẽ nghiên cứu thêm |
| **Nguồn gốc** | [AI Agent Adoption Guidance — Microsoft CAF](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/) (kiểm ngày 2026-10-28; trang gốc cập nhật 2026-02 → 2026-10) |
| **Phạm vi công nghệ** | **Chỉ Microsoft Foundry** (đã chốt 2026-10-28). Copilot Studio, M365 Copilot agent, GPU/container của khung Microsoft được lược bỏ; Fabric chỉ là nguồn dữ liệu |
| **Danh mục use case** | 130 agent — cc1-agent-list.pdf · đánh giá lại và trọng số: [cc1-agent-list-danh-gia.md](01-plan-for-agents/01-cc1-agent-list-danh-gia.md) |

## Bốn bước

```
(1) Plan ──► (2) Govern & secure ──► (3) Build ──► (4) Manage
 Lập kế hoạch     Quản trị & bảo mật      Xây dựng      Vận hành
```

| Bước | Microsoft gọi là | Bước con (theo Microsoft) | Tài liệu CC1 |
|---|---|---|---|
| 1 | Plan for agents | Business plan · Technology plan · Organizational readiness · Data architecture | [01-plan-for-agents.md](01-plan-for-agents.md) |
| 2 | Govern and secure agents | Responsible AI · Governance & security · Prepare environment | [02-govern-and-secure-agents.md](02-govern-and-secure-agents.md) |
| 3 | Build agents | Single or multiple agents · Process to build agents | [03-build-agents.md](03-build-agents.md) |
| 4 | Manage agents | Integrate agents · Operate agents | [04-manage-agents.md](04-manage-agents.md) |

## Khái niệm nền (Microsoft)

- **AI agent** = chương trình dùng mô hình sinh để đọc đầu vào (sự kiện, tin nhắn người dùng, tin nhắn agent khác), suy luận và chọn hành động. Khác ứng dụng truyền thống ở chỗ luồng xử lý không cố định.
- **5 thành phần**: mô hình (bộ suy luận) · instructions (phạm vi, ranh giới) · retrieval (dữ liệu nền) · actions/tools (hàm, API) · memory (lịch sử, trạng thái).
- **Khác RAG**: RAG truy xuất theo pipeline cố định; agent tự quyết dùng tri thức/tool nào ở từng bước, nên phi tất định và cần test + quản trị chặt hơn.
- **3 loại agent** theo mức tự chủ tăng dần: productivity (truy xuất, tổng hợp) → action (làm tác vụ cụ thể trong workflow) → automation (quy trình nhiều bước, có trigger, ít giám sát).
- **Lý do đầu tư**: hiệu quả (tự động việc lặp), tốc độ, mở rộng theo tải.

## Hiện trạng CC1 liên quan (để đối chiếu khi điền từng bước)

| Đã có | Ở đâu |
|---|---|
| Kiến trúc nền tảng Foundry: 3 stage, 1 subscription, CI/CD, đặt tên, RACI, luật F1–F10 | Bức tranh tổng quát |
| Runbook, giám sát, registry, phân quyền, model/chi phí, dữ liệu/tuân thủ chi tiết | Sổ tay vận hành |
| Luồng dữ liệu Fabric → Foundry, workspace tài liệu, ràng buộc tenant | Tích hợp Fabric |
| Câu hỏi mở, tuân thủ dữ liệu cá nhân & Luật AI | FDY-0001 · FDY-0002 |
| Pilot `agt-catalog`, PoC kết quả 11/2026, ngân sách 5.000 USD | Tổng quát §07, §25 |

Nhận xét ban đầu: tài liệu CC1 hiện tập trung mạnh ở bước 2 (môi trường, phân quyền, tuân thủ) và bước 3 (quy trình build, CI/CD). Bước 1 (business plan, chấm điểm use case, chỉ số thành công, sẵn sàng tổ chức) và bước 4 (tích hợp vào quy trình nghiệp vụ, vòng đời, quota) là chỗ trống lớn nhất.

## Cách dùng bộ tài liệu

1. Đọc phần "Khung Microsoft" ở mỗi bước để có ngôn ngữ chung.
2. Điền phần "Áp dụng tại CC1" theo thứ tự ưu tiên: bước 1 trước (chưa có), rồi bước 4, sau đó rà lại bước 2 và 3 bằng cách trỏ sang tài liệu đã có thay vì viết lại.
3. Mỗi mục CC1 ghi rõ: quyết định / người quyết / trạng thái (đề xuất · đã chốt · đang làm) · nguồn.
4. Fact về Microsoft ghi kèm link và ngày kiểm tra, theo quy ước index.md.
5. Tài liệu chi tiết của một bước đặt trong thư mục riêng cùng tên bước và mang tiền tố số bước, ví dụ [03-build-agents/](03-build-agents/) chứa `03-model-selection-cc1.md`; bước chưa có tài liệu chi tiết thì chưa tạo thư mục.

## Nguồn (Microsoft Learn, kiểm 2026-10-28)

| Trang | URL |
|---|---|
| Tổng quan | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/ |
| Business plan | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/business-strategy-plan |
| Technology plan | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/technology-solutions-plan-strategy |
| Organizational readiness | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/organization-people-readiness-plan |
| Data architecture | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/data-architecture-plan |
| Govern & secure | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/governance-security-across-organization |
| Single or multiple agents | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/single-agent-multiple-agents |
| Process to build | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/build-secure-process |
| Manage | https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/integrate-manage-operate |
