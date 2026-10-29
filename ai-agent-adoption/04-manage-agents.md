# Bước 4 · Manage agents — Tích hợp và vận hành AI agent

> Trả lời: đưa agent vào công việc hằng ngày thế nào, và duy trì cả "đội agent" an toàn, đúng chi phí, đúng tuân thủ từ deploy đến nghỉ hưu.
> Lộ trình: [microsoft-foundry-adoption-roadmap.md](microsoft-foundry-adoption-roadmap.md) · Trước: [03-build-agents.md](03-build-agents.md)

| | |
|---|---|
| **Trạng thái** | Draft — phần Microsoft đã tóm tắt; phần CC1 chưa có |
| **Nguồn** | Trang Manage của Microsoft CAF (link ở cuối), kiểm 2026-10-28 |

## Phần A · Khung Microsoft

### A1 · Integrate — đưa agent vào nghiệp vụ

**Business integration**
1. Nhúng agent vào nơi người dùng đang làm việc (Teams, portal nội bộ); đổi ngữ cảnh làm giảm adoption.
2. Change management: đào tạo prompting, hiểu giới hạn agent, cách dùng output trong quyết định.
3. Mở rộng theo giai đoạn: 1 đơn vị / use case tác động cao trước, đo giá trị thật, rồi mới rộng.

**Standardization**
1. Template kiến trúc tái dùng (prompt, pattern tích hợp, RAG) tự kế thừa security, logging, monitoring. Foundry: Bicep/Terraform.
2. Vòng cải tiến liên tục: thu feedback + dữ liệu vận hành, backlog ưu tiên câu hỏi chưa trả lời được.

### A2 · Operate — vận hành đội agent

**Asset lifecycle**
1. Kiểm kê định kỳ (hàng quý) từ registry ở bước 2; nghỉ hưu agent không dùng.
2. Quản trị tập trung một control plane: policy, xoay credential, access control.
3. Tối ưu chi phí có hệ thống: token cap, rate limit, rút ngắn system prompt, tóm tắt lịch sử, cache, chuyển việc tất định sang rule.
4. HA theo mức quan trọng: agent nội bộ không trọng yếu không cần redundancy như mission-critical.

**Compliance & security liên tục**
1. Scan tự động phát hiện drift cấu hình, vi phạm policy (Azure Policy, Defender for Cloud).
2. Alert vào SOC; jailbreak, rò rỉ, bất thường xử lý như sự cố an ninh thường.
3. Red teaming định kỳ, nhất là sau đổi model hoặc kiến trúc.

**Quota**
1. Rà xu hướng tiêu thụ hàng tháng.
2. Chỉnh quota theo ưu tiên nghiệp vụ (workload quan trọng cao hơn, thử nghiệm thấp hơn).
3. Benchmark lại lựa chọn model: model nhỏ cho việc thường.

**Administration**
1. Inventory toàn bộ project/deployment luôn cập nhật.
2. **AI Gateway** (Azure API Management) làm điểm kiểm soát chung: log, throttle, access.
3. Kiểm soát chi tiết: token limit theo project/agent, pause/resume agent và tool ngoài khi có sự cố.

## Phần B · Áp dụng tại CC1

_Chưa nghiên cứu. Đây là bước trống nhất cùng với bước 1._

### B1 · Tích hợp vào nghiệp vụ CC1
- [ ] Kênh tiếp cận agent: Teams? Power BI? portal? (liên hệ project teams-management).
- [ ] Kế hoạch rollout theo giai đoạn cho `agt-catalog` sau PoC 11/2026.
- [ ] Kế hoạch đào tạo người dùng cuối và thu feedback.
- [ ] Template agent chuẩn CC1 (Bicep đã có ở Tổng quát §09 plan).

### B2 · Vận hành CC1 đối chiếu

| Microsoft | CC1 đã có | Cần bổ sung |
|---|---|---|
| Lifecycle, kiểm kê | Tổng quát §21 (registry), §22 (lịch định kỳ); Sổ tay §C, §D | [ ] tiêu chí nghỉ hưu agent · [ ] rà quý |
| Chi phí, quota | Tổng quát §07; Sổ tay §G | [ ] token cap / rate limit · [ ] AI Gateway có dùng không · [ ] rà tháng |
| Compliance & security liên tục | Sổ tay §B, §I | [ ] Azure Policy scan · [ ] SOC · [ ] red teaming định kỳ |
| Administration | Tổng quát §04, §20; Sổ tay §A (runbook) | [ ] runbook pause/resume agent · [ ] inventory tự động |

## Nguồn

- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/integrate-manage-operate
- Foundry Control Plane: https://learn.microsoft.com/en-us/azure/ai-foundry/control-plane/overview
- AI Gateway: https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools/governance
