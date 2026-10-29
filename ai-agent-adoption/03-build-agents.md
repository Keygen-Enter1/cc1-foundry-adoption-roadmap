# Bước 3 · Build agents — Xây dựng AI agent

> Trả lời: một agent hay nhiều agent, và quy trình chuẩn nào mọi đội phải theo khi build.
> Lộ trình: [microsoft-foundry-adoption-roadmap.md](microsoft-foundry-adoption-roadmap.md) · Trước: [02-govern-and-secure-agents.md](02-govern-and-secure-agents.md) · Tiếp: [04-manage-agents.md](04-manage-agents.md)

| | |
|---|---|
| **Trạng thái** | Draft — phần Microsoft đã tóm tắt; B0 đã chốt; Phần C (quy trình phát triển agent) đề xuất chờ duyệt, lý do ở FDY-0005 |
| **Nguồn** | 2 trang Build của Microsoft CAF (link ở cuối), kiểm 2026-10-28 |

## Phần A · Khung Microsoft

### A1 · Single agent hay multi-agent?

| Cách tiếp cận | Dùng khi | Bỏ qua prototype? | Bước đầu |
|---|---|---|---|
| **Single agent** | Miền hẹp, cần ngữ cảnh thống nhất, ưu tiên tốc độ, ràng buộc chi phí | Có | Deploy 1 agent với tool cần thiết, lặp theo usage |
| **Multi-agent** | Ranh giới cứng về security/compliance/tổ chức; chắc chắn mở rộng đa miền | Có | Agent cô lập, quyền giới hạn, hợp đồng giao diện rõ |

**Bắt đầu multi-agent** chỉ khi: vượt ranh giới security/compliance (cần cô lập dữ liệu, tách trách nhiệm) · nhiều đội sở hữu miền tri thức khác nhau · lộ trình chắc chắn mở rộng (trên 3–5 chức năng riêng biệt).

**Test single agent trước** khi: có nhiều vai (planner/reviewer/executor) nhưng chưa chứng minh cần tách · cần ra thị trường nhanh · chi phí ưu tiên · dữ liệu lớn (thường lỗi ở retrieval chứ không phải kiến trúc) · tải cao · đa phương thức.

**Trade-off multi-agent**: mỗi handoff thêm độ trễ, trạng thái phải đồng bộ, prompt/giám sát/debug nhân lên, bề mặt bảo mật rộng hơn, chi phí tăng vì context lặp.

**Workflow** (Foundry workflows, Agent Framework workflows): cần cho multi-agent (điều phối, trạng thái, rẽ nhánh, minh bạch) và cũng hữu ích cho single agent (lặp lại theo lịch, tích hợp hệ thống, cổng duyệt, human review).

### A2 · Quy trình build — 5 khối

**1. Orchestration**
- *Agent charter*: tài liệu ranh giới — trách nhiệm hệ thống gắn mục tiêu nghiệp vụ, vai từng agent, hành động bị cấm.
- *Chiến lược*: managed orchestration (Foundry Agent Service) vs code-first (Agent Framework, LangGraph); pattern tuần tự vs song song; **workflow tất định cho logic nghiệp vụ quan trọng**.
- *Instructions là code*: cấu trúc chuẩn (identity & tone · scope & boundaries · tool mandates · citation rules), output có cấu trúc (JSON/XML), version control + validation tự động trước deploy.

**2. Model selection**: model nhỏ cho việc thường, model cao cấp cho suy luận phức tạp; đa dạng model tránh nghẽn quota; tag chi phí; test nhỏ trước deploy. Công cụ: model catalog, leaderboard, Model Router, quotas & limits.

**3. Knowledge, tools, memory**
- *Knowledge*: phân vùng theo vai, chỉ lấy từ kho đã quản trị (Foundry IQ). **CC1:** kho tri thức không đặt trong Foundry; dự kiến đặt ở nơi khác (Fabric, xem [tài liệu bước 1, mục B4 · Kiến trúc dữ liệu CC1](01-plan-for-agents.md#b4--kiến-trúc-dữ-liệu-cc1)), Foundry gọi đến khi trả lời.
- *Tools*: ranh giới tường minh trong system message, human-in-the-loop cho hành động tác động lớn (ghi DB, giao dịch); API hẹp thay vì truy cập trực tiếp; test trong sandbox; log gắn agent instance + conversation ID. AI Gateway để quản trị tool.
- *Memory*: ephemeral cho agent giao dịch, persistent chỉ cho agent tư vấn; lưu ngoài inference engine (Cosmos DB, BYO storage); cô lập user/tenant/domain; retention và xoá tự động. **CC1:** chưa kích hoạt được memory ở vùng Asia (kiểm 2026-10-29); để dùng sau.

**4. Observability**: tracing (đường thực thi, điểm quyết định), baseline latency/chi phí/success rate, cảnh báo lệch; bộ evaluator chung lưu catalog, chạy trong CI/CD (GitHub Actions, Azure DevOps), continuous evaluation; red teaming định kỳ. Foundry Control Plane, Agent Monitoring Dashboard.

**5. Security**: guardrail ở nhiều điểm (input → tool call → tool response → output); blocklist tập trung, cập nhật theo sự cố; Defender AI protection, AI Gateway.

## Phần B · Áp dụng tại CC1

_Chỉ ghi trạng thái hiện tại; lý do và lịch sử ở FDY-0004._

### B0 · Chuẩn công nghệ build (đã chốt 2026-10-28)

| Loại agent | Cách làm | Dùng cho |
|---|---|---|
| Không cần code | **Declarative agent** trên Foundry: instructions + model + tool/knowledge khai báo; Foundry chạy vòng suy luận | Nhóm A, data agent, RAG |
| Cần code | Viết bằng **Microsoft Agent Framework** (SDK duy nhất được chuẩn hoá), deploy thành **hosted agent** trong project Foundry | Nhóm B/C: workflow tất định, nhiều nhánh, human-in-the-loop, ghi Odoo |
| Việc chạy ngắn theo lịch | Không làm agent; Azure Functions hoặc pipeline Fabric gọi model Foundry | Nhóm D (code/ML) |

Nguyên tắc: không tự host agent ngoài Foundry (Functions/ACA/AKS) cho agent có người dùng, để mọi agent cùng endpoint, identity, trace, evaluation, guardrail và registry của Foundry. Agent Framework chỉ là thư viện, không có runtime; hosted agent là runtime được quản lý, không thay thế thư viện.


### B1 · Quy trình build CC1 đối chiếu 5 khối

Cột "Cần bổ sung" làm chi tiết ở FDY-0006.

| Khối | CC1 đã có | Cần bổ sung |
|---|---|---|
| Orchestration | Tổng quát: quy ước asset, cấu trúc thư mục, quy trình đưa use case mới | có: [mẫu bản mô tả agent](../templates/agent-charter-template.md) · có: [mẫu instructions 4 phần](../templates/agent-instructions-template.md) · có: script kiểm instructions (chạy tay trước PR, CI sau) |
| Model | Tổng quát: model, quota & chi phí; Sổ tay: model & chi phí chi tiết | có: [chọn model theo tác vụ](03-build-agents/03-model-selection-cc1.md) (đề xuất) · Model Router: không dùng, chưa có ở Southeast Asia |
| Knowledge/tools/memory | Tổng quát: dữ liệu, knowledge & tuân thủ; Tích hợp Fabric: đưa tài liệu và dữ liệu có cấu trúc cho agent; Sổ tay: dữ liệu & tuân thủ chi tiết | Knowledge đặt ngoài Foundry (Fabric), Foundry gọi đến · memory: chưa dùng (vùng Asia chưa kích hoạt được) · có: [quy tắc người duyệt cho tool ghi](03-build-agents/03-human-in-the-loop-cc1.md) (đề xuất) |
| Observability | Tổng quát: giám sát & cảnh báo; Sổ tay: giám sát/SLO, CI/CD chi tiết (golden test) | có: [evaluator catalog](03-build-agents/03-evaluator-catalog-cc1.md) (đề xuất) · có: [đánh giá liên tục](03-build-agents/03-continuous-eval-redteam-cc1.md) (đề xuất) · có: [trace → App Insights](03-build-agents/03-guardrail-trace-cc1.md) (đề xuất) |
| Security | Tổng quát: luật cứng F1–F10 | có: [guardrail 4 điểm](03-build-agents/03-guardrail-trace-cc1.md) (đề xuất) · có: [blocklist tiếng Việt](03-build-agents/03-blocklist-cc1.md) (đề xuất) · có: [red teaming](03-build-agents/03-continuous-eval-redteam-cc1.md) (thủ công; agent tự động chưa có ở SEA) |

## Phần C · Quy trình phát triển agent tại CC1 (đề xuất 2026-10-29, chờ duyệt · FDY-0005)

Mọi agent đi qua một quy trình chung: đề xuất thành task → viết bản mô tả agent (charter) → build và kiểm thử ở dev → UAT → phát hành prd → vận hành. Ba cổng duyệt G1, G2, G3 và các ngưỡng bên dưới lấy từ tài liệu Tổng quát, mục "Quy trình đưa use case mới lên platform" và mục "Luồng CI/CD". Vai (Lead, trưởng dự án, thành viên dự án, DA, DPO) theo mục "Vai trò & RACI" của cùng tài liệu; 3 vai Platform / Workload / AI CoE ở [tài liệu bước 1, mục B3](01-plan-for-agents.md#b3--sẵn-sàng-tổ-chức-cc1) là nhóm nhiệm vụ, chưa cần ánh xạ với vai RACI (chốt 2026-10-29).

| # | Giai đoạn | Việc chính | Ai | Đầu ra | Cổng | Bước |
|---|---|---|---|---|---|---|
| 1 | Đề xuất, chấm điểm | Phiếu đề xuất (intake): vấn đề, người dùng, nguồn và lớp dữ liệu, KPI. Chấm impact / feasibility / data readiness, xếp nhóm A–D như [tài liệu bước 1, mục B1](01-plan-for-agents.md#b1--business-plan-cc1) | Người đề xuất; Lead sàng lọc (nhận hay từ chối, giao trưởng dự án) | Task FDY; dòng trong danh sách agent đã chấm | **G1** (Lead): có trưởng dự án, lớp dữ liệu chấp nhận được (c4 từ chối, c3 cần đánh giá tác động dữ liệu cá nhân (DPIA)), model trong danh mục, chi phí vừa budget, KPI đo được. 130 agent trong catalog đã được lãnh đạo cao nhất yêu cầu, nên G1 chỉ còn kiểm điều kiện, không xét có làm hay không (chốt 2026-10-29) | 1 |
| 2 | Bản mô tả agent, thiết kế | Bản mô tả agent (charter): agent làm gì, cho ai, được và không được làm gì (theo A2 khối 1); lớp dữ liệu c0–c4; nguồn dữ liệu theo [tài liệu bước 1, mục B4](01-plan-for-agents.md#b4--kiến-trúc-dữ-liệu-cc1); chọn declarative hay Agent Framework + hosted agent (B0); single hay multi-agent (A1) | Trưởng dự án viết; Lead duyệt; DPO nếu c3 | Bản mô tả agent ghi trong task FDY (mẫu chưa có) | Lead duyệt bản mô tả | 2 · 3 |
| 3 | Build ở dev | Instructions theo cấu trúc A2; tool, knowledge; bộ golden tiếng Việt (bộ câu hỏi mẫu kèm đáp án đúng, dùng để chấm agent), đáp án do DA xác nhận; chạy evaluator (chấm tự động) ở dev; mở PR | Thành viên dự án; DA | Định nghĩa agent trong Git (thư mục `agents/`, plan); dataset golden. **Bắt buộc sinh agent từ Git bằng script, kể cả ở dev; không tạo tay trên portal** (chốt 2026-10-29) | PR 1 approval + CODEOWNERS; check tĩnh CI (plan) | 3 |
| 4 | UAT | Promote theo pipeline ở [tài liệu bước 1, mục B2](01-plan-for-agents.md#b2--technology-plan-cc1): merge → dev → tag → uat. Cổng chấm tự động: trả lời đúng ≥85% bộ golden và không tụt quá 3 điểm so với bản trước; bộ safety (câu hỏi xấu phải từ chối) đạt ngưỡng. ≥5 người dùng xác nhận | Lead tạo tag; trưởng dự án xác nhận UAT, dán link vào task | Tag release | **G2** (trưởng dự án): định nghĩa agent nằm trong Git, ≥50 câu golden, đúng ≥85%, safety đạt · **G3** (trưởng dự án + Lead): ≥5 người dùng xác nhận, có mục sổ đăng ký AI, DPO duyệt nếu c3 | 1 · 2 |
| 5 | Phát hành prd | Deploy từ tag đã qua uat, có duyệt; smoke test (vài câu kiểm nhanh sau deploy); publish Teams; ghi registry (sổ theo dõi agent đang chạy) | Lead | Mục registry (plan) | Duyệt release prd | 4 |
| 6 | Vận hành | Theo dõi sát 2 tuần đầu, review ngày 30; giám sát, chấm lại định kỳ, red teaming (cố tình tấn công agent để tìm lỗ hổng) định kỳ; rollback bằng cách trỏ lại bản trước; xem xét dừng khi người dùng hoạt động <20% trong 2 tháng | Người trực, trưởng dự án, Lead | Runbook, task | — | 4 |

Luật chung: review xong task chỉ chuyển `Pending`, Lead hoặc user mới chuyển `Done`; AI coding assistant không tạo tag, không promote.

**Hai đường build** (theo B0)

| Bước | Declarative (nhóm A, data agent, RAG) | Agent Framework + hosted agent (nhóm B/C) |
|---|---|---|
| Bản mô tả agent | Như nhau | Thêm: thiết kế workflow, tool, điểm cần người duyệt (human-in-the-loop) |
| Build | Định nghĩa (instructions, model, tool, knowledge) trong Git, đẩy lên bằng script | Code Python trong repo; code review; test tool trong sandbox (A2); build rồi deploy thành hosted agent |
| Kiểm thử | Golden + safety | Golden + safety + test tool và chuyển giao giữa các agent |
| UAT, phát hành, vận hành | Như nhau | Như nhau |

**Trạng thái 2026-10-29**: mới có mẫu task FDY, quy trình và ngưỡng trong tài liệu Tổng quát (phần lớn ghi plan), script tạo hạ tầng (Bicep) mới chạy thử ở chế độ xem trước (what-if), chưa tạo thật. Chưa có: thư mục `agents/`, script đẩy agent, workflow CI/CD, mẫu intake và charter, bộ golden, evaluator, registry. Chưa agent nào đi qua quy trình.

Việc cần làm:
- [ ] Mẫu phiếu đề xuất và mẫu bản mô tả agent trong `docs/templates/`.
- [ ] Định dạng bộ golden và cách DA xác nhận; số câu cho nhóm A.
- [ ] Chạy thử quy trình với một thí điểm nhóm A bằng declarative agent, đo thời gian từng giai đoạn.
- [ ] Workflow CI/CD tối thiểu (dev, uat, smoke).
- [ ] Cập nhật tài liệu Tổng quát (RACI, quy trình đưa use case mới) khi chốt.

**Thuật ngữ dùng trong phần này**

| Thuật ngữ | Nghĩa |
|---|---|
| Phiếu đề xuất (intake) | Biểu mẫu một trang để xin làm agent: vấn đề, người dùng, dữ liệu cần, KPI |
| Bản mô tả agent (charter) | Văn bản ngắn chốt agent làm gì, cho ai, dùng dữ liệu nào, được và không được làm gì |
| Lớp dữ liệu c0–c4 | Mức nhạy cảm của dữ liệu, c0 công khai đến c4 tuyệt mật; c3 trở lên cần DPIA |
| DPIA | Đánh giá tác động dữ liệu cá nhân, bắt buộc theo luật khi xử lý dữ liệu nhạy cảm |
| Bộ golden | Bộ câu hỏi mẫu kèm đáp án đúng do nghiệp vụ xác nhận; dùng để chấm agent trước mỗi lần phát hành |
| Bộ safety | Bộ câu hỏi xấu (lộ dữ liệu, nội dung độc hại) mà agent phải từ chối |
| Evaluator | Chương trình chấm tự động câu trả lời của agent theo bộ golden và safety |
| Declarative agent | Agent tạo bằng khai báo (instructions, model, tool), không viết code |
| Hosted agent | Agent viết bằng code, Foundry chạy và quản lý giúp |
| Human-in-the-loop | Hành động quan trọng (ghi dữ liệu) phải có người duyệt trước khi agent thực hiện |
| Smoke test | Vài câu kiểm nhanh ngay sau deploy để chắc agent còn chạy |
| Red teaming | Cố tình tấn công agent để tìm lỗ hổng trước khi kẻ xấu tìm ra |
| Registry | Sổ theo dõi agent đang chạy: chủ, phiên bản, dữ liệu dùng, ngày review |
| Bicep what-if | Chạy thử script tạo hạ tầng Azure ở chế độ xem trước, chỉ báo sẽ tạo/sửa gì, không tạo thật |

## Nguồn

- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/single-agent-multiple-agents
- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/build-secure-process
- Orchestration patterns: https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns
- Well-Architected AI workloads: https://learn.microsoft.com/en-us/azure/well-architected/ai/
