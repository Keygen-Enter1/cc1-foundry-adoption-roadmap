# Bước 2 · Govern and secure agents — Quản trị và bảo mật AI agent

> Trả lời: chuẩn tối thiểu nào mọi agent phải đạt trước khi được chạy, và môi trường nào cưỡng chế chuẩn đó.
> Lộ trình: [microsoft-foundry-adoption-roadmap.md](microsoft-foundry-adoption-roadmap.md) · Trước: [01-plan-for-agents.md](01-plan-for-agents.md) · Tiếp: [03-build-agents.md](03-build-agents.md)

| | |
|---|---|
| **Trạng thái** | Draft — phần Microsoft đã tóm tắt; phần CC1 chưa có (nhưng nhiều nội dung đã nằm ở tài liệu nền) |
| **Nguồn** | Trang Govern & secure của Microsoft CAF (link ở cuối), kiểm 2026-10-28 |

## Phần A · Khung Microsoft

Khuyến nghị cốt lõi: **một baseline quản trị + bảo mật tập trung, cưỡng chế được, cho mọi agent**, gắn vào identity, data governance và security hiện có, không dựng mô hình quản trị song song.

### A1 · Responsible AI policies
Chính sách RAI (công bằng, tin cậy & an toàn, riêng tư & bảo mật, bao trùm, minh bạch, trách nhiệm) phải có trước; governance và security xây trên đó, không trùng lặp hay mâu thuẫn.

### A2 · Governance & security — 4 miền

**2.1 Control plane** — mọi agent phải quan sát được, quản trị được, bảo mật được.
1. Gán trách nhiệm quản trị agent cho đúng người đang chịu trách nhiệm cloud governance/security/compliance.
2. **Agent registry**: mọi agent ghi vào một inventory duy nhất (owner, mục đích, platform, phạm vi truy cập). Có Agent 365 thì dùng registry sẵn; chưa có thì Microsoft Entra Agent ID là nguồn sự thật về danh tính và owner.
3. **Mỗi agent một identity riêng** (Entra Agent ID), gắn với chính sách identity của tổ chức.
4. Chính sách áp nhất quán mọi nền tảng (first-party, custom, third-party).
5. Quan sát hoạt động và tool liên tục; dùng dữ liệu quan sát để chỉnh guardrail.
6. **Theo dõi & phân bổ chi phí**: tag theo agent/use case/cost center, cảnh báo ngưỡng, giới hạn ai được tạo/deploy/scale.

Không có Agent 365 thì ghép: Entra (identity) + Purview (data governance) + Defender (security) + Azure Monitor (giám sát). Có accelerator tự động hoá: Data and Agent Governance and Security Accelerator (GitHub Microsoft).

**2.2 Data governance & compliance**
- *Regulatory*: riêng tư dữ liệu (tối thiểu hoá, rà dataset RAG/fine-tune, ẩn danh, quyền xoá) · data residency (vị trí nguồn, runtime, output; log không giữ nội dung nhạy cảm) · retention (log, memory, training data; tự động purge).
- *Corporate*: cô lập dữ liệu mật khỏi agent công khai (landing zone "corp" vs "online") · least-privilege data access, agent kế thừa quyền người dùng, Purview DLP + sensitivity label · chuẩn hoá tích hợp knowledge/tool qua API/connector chính thức · minh bạch (công bố có AI, thông báo khi dùng dữ liệu) · công bằng & trách nhiệm (owner kết quả, người duyệt deploy).
- Công cụ: Purview Compliance Manager, Purview API, Azure Policy cho Foundry (cấu hình hạ tầng, model deployment policy).

**2.3 Security** — 10 yêu cầu baseline
1. Đào tạo security cho đội dev/security (jailbreak, data poisoning, credential theft).
2. Bật AI threat protection (Defender for Cloud).
3. Hạ tầng agent theo baseline như hệ thống khách hàng.
4. Red teaming trước prod và sau thay đổi lớn (AI Red Teaming Agent).
5. Lọc input/output; coi mọi đầu vào là thù địch.
6. Managed identity cho xác thực, không secret.
7. Least privilege cho tool; DLP giới hạn đầu ra.
8. Alert chảy vào SOC (Azure Monitor → Log Analytics → Sentinel).
9. Quản trị tích hợp ngoài: chỉ MCP server tin cậy, A2A với entity được duyệt.
10. Kế hoạch sự cố: tắt agent nhanh, giữ log, truyền thông, DR, diễn tập.

**2.4 Development standards**: chuẩn hoá framework/SDK (Microsoft Agent Framework, Foundry SDK) và giao thức (MCP cho tool/data, A2A cho agent-agent).

### A3 · Prepare environment
1. Thoả thuận chiến lược cưỡng chế với workload team: bắt đầu bằng audit/giám sát, siết dần.
2. Quản trị môi trường: Azure platform → workload landing zone có governance nhất quán (không cần landing zone riêng cho AI); SaaS → cấu hình identity, access, data governance.
3. Mẫu tham chiếu: Foundry chat baseline architecture, AI Landing Zones (GitHub Azure), Well-Architected AI workloads.

## Phần B · Áp dụng tại CC1

_Phần lớn đã có ở tài liệu nền; việc ở đây là đối chiếu với 4 miền của Microsoft, chỉ ra khoảng trống, không viết lại._

### B1 · Đối chiếu với tài liệu nền

| Miền Microsoft | CC1 đã có | Khoảng trống cần nghiên cứu |
|---|---|---|
| Responsible AI policies | FDY-0002 (bảo vệ dữ liệu cá nhân, Luật AI) — Planning | [ ] Chính sách RAI thành văn của CC1 |
| 2.1 Control plane | Tổng quát §15 (danh tính), §16 (tag), §21 (registry vận hành); Sổ tay §C | [ ] Entra Agent ID cho từng agent · [ ] Agent 365 có dùng không · [ ] cảnh báo chi phí theo tag |
| 2.2 Data governance | Tổng quát §08, §04; Sổ tay §H, §F; Tích hợp Fabric §7 | [ ] residency Southeast Asia / Data Zone APAC đã đủ chưa · [ ] retention log/memory · [ ] Purview DLP |
| 2.3 Security | Tổng quát §11, §26 (F1–F10); Sổ tay §I | [ ] red teaming · [ ] Defender AI threat protection · [ ] SOC/Sentinel · [ ] kế hoạch sự cố tắt agent |
| 2.4 Dev standards | Tổng quát §18–§19; CI/CD §05 | [ ] chuẩn framework (Agent Framework / Foundry SDK) · [ ] chuẩn MCP/A2A |
| Prepare environment | Tổng quát §02 topology, §09 provisioning (plan) | [ ] landing zone / Azure Policy · [ ] audit trước, siết sau |

### B2 · Việc cần quyết (ghi quyết định · người quyết · trạng thái)
- [ ] Ai chịu trách nhiệm governance agent tại CC1 (gắn vào RACI §20).
- [ ] Registry agent: dùng gì, ở đâu, trường bắt buộc.
- [ ] Mức security tối thiểu cho PoC vs prd.

## Nguồn

- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/governance-security-across-organization
- Responsible AI policies (CAF AI scenario): https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/ai/responsible-ai-policies
- Accelerator: https://github.com/microsoft/Data-and-Agent-Governance-and-Security-Accelerator
