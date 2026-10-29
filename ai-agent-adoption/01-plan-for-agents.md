# Bước 1 · Plan for agents — Lập kế hoạch cho AI agent

> Trả lời: nên làm agent cho việc gì, bằng công nghệ nào, ai làm, dữ liệu lấy từ đâu.
> Lộ trình: [microsoft-foundry-adoption-roadmap.md](microsoft-foundry-adoption-roadmap.md) · Bước tiếp: [02-govern-and-secure-agents.md](02-govern-and-secure-agents.md)

| | |
|---|---|
| **Trạng thái** | Draft v0.4 — B2 đã chốt; B1, B3, B4 là đề xuất chờ chủ nghiệp vụ duyệt. Lịch sử thay đổi: FDY-0004 |
| **Nguồn** | 4 trang Plan của Microsoft CAF (link ở cuối), kiểm 2026-10-28 |
| **Phạm vi công nghệ** | Chỉ Microsoft Foundry (B2) |

## Phần A · Khung Microsoft (rút gọn theo Foundry)

### A1 · Business plan

**Khi nào không dùng agent**
- Tác vụ có cấu trúc, lặp lại, quy tắc rõ → code thường, pipeline, hoặc mô hình ML không sinh.
- Chỉ hỏi đáp / tóm tắt trên tập tài liệu cố định, không cần tool hay suy luận nhiều bước → RAG thường là đủ.

**Khi nào dùng agent**: cần quyết định nhiều bước · dùng nhiều tool/hệ thống theo thứ tự linh hoạt · đầu vào mơ hồ cần diễn giải.

**Chấm điểm ưu tiên** (1–5, ba trục): business impact (khớp chiến lược, giá trị, thời gian change management) · technical feasibility (rủi ro gọi tên được, safeguard đủ, khớp hạ tầng) · user desirability (persona rõ, value proposition rõ, ít kháng cự). Thực hành: không gọi tên được rủi ro thì chưa làm; pilot ở bước khó nhất trước.

**Chỉ số thành công**: KPI nền + baseline → KPI làm cổng go/no-go từng giai đoạn → đo tiếp sau go-live, định kỳ rà để sửa / nghỉ hưu / chuyển nguồn lực.

### A2 · Technology plan — Foundry

Microsoft Foundry (PaaS): nơi build và chạy agent, không quản lý VM/Kubernetes.
- **Hai kiểu agent**: declarative (prompt-based, cập nhật/version dễ) và hosted (code-first, framework tuỳ chọn, runtime được quản lý).
- **Workflows**: điều phối nhiều agent, logic tuần tự / rẽ nhánh / trạng thái.
- **Model**: Foundry Models (OpenAI, Anthropic, Meta, Mistral, ...), Model Router.
- **Memory**: managed hoặc bring-your-own (yêu cầu chủ quyền dữ liệu).
- **Tools**: tool catalog, MCP, OpenAPI, Logic Apps, Functions; A2A; publish lên M365 / Agent 365.
- **Setup**: *basic* (prototype nhanh, không cô lập mạng) · *standard* (kiểm soát dữ liệu/mạng; public networking cho workload không mật, private networking cho workload mật).
- **Kiểm chứng**: thí nghiệm ngắn, ghi tài liệu, stakeholder duyệt, rồi quyết single hay multi-agent (xem [03-build-agents.md](03-build-agents.md)).

### A3 · Organizational readiness

Ba vai: **platform team** (nền tảng, guardrail, kiểm toán chính sách, observability) · **workload team** (sở hữu vòng đời từng agent, yêu cầu nghiệp vụ, dữ liệu miền, tích hợp quy trình) · **AI CoE** (chuẩn chung, tư vấn, đào tạo, đưa RAI vào chính sách).

Kỹ năng: prompt engineering · tối ưu & đánh giá agent · đạo đức & quản trị AI · data engineering cho AI (RAG, vector index) · AI security.

Đào tạo: Microsoft Learn miễn phí + chứng chỉ → workshop/hackathon nội bộ → đối tác → mentor và cộng đồng nội bộ. Change management: nói sớm agent làm được/không làm được gì, lãnh đạo khẳng định ưu tiên, ghi nhận đội làm tốt.

### A4 · Data architecture

- Nền tảng hợp nhất: Fabric OneLake là hồ trung tâm; data domain tạo data product; agent tiêu thụ qua Fabric IQ / Foundry IQ.
- Truy xuất cho custom agent: mặc định dùng tính năng có sẵn (Foundry IQ, Fabric IQ, Fabric data agent tool, MCP server, SharePoint tool, Azure AI Search, OneLake indexer); chỉ custom khi có yêu cầu vượt giới hạn và được ghi nhận.
- MCP server khi agent cần hành động hoặc dữ liệu thời gian thực; định read-only / read-write từ đầu; mọi tool call xác thực; RBAC hai phía.
- Ghi lại quyết định truy xuất theo data domain: search / API / MCP; nguồn chứng nhận; permission.

## Phần B · Áp dụng tại CC1

_Chỉ ghi trạng thái hiện tại. Lý do, phương án đã loại và lịch sử quyết định nằm ở FDY-0004._

### B1 · Business plan CC1

**Danh mục use case**: 130 agent trong cc1-agent-list.pdf (EA Final AI Agent Catalog 130 v1.0, 16/10/2026): 14 nhóm nghiệp vụ, 5 team làm (Data, ERP, AI-Chatbot, AI-Knowledge, Công trường số), 3 thí điểm T12/2026, 4 đợt T1–T4/2027. Ngoài ra 4 agent dữ liệu đề xuất thêm (AGT-19..22) gần với ID 6 và ID 105, nên gộp.

**Đánh giá lại và trọng số** → [01-cc1-agent-list-danh-gia.md](01-plan-for-agents/01-cc1-agent-list-danh-gia.md) (đề xuất, chờ duyệt). Bản chi tiết 13 cột (mã, Khối/Phòng, phân quyền, nguồn dữ liệu, MCP, KB) → sheet "130 agent chi tiết" trong CC1_Foundry_AI_Adoption_Plan_v17.xlsx (nháp AI, FDY-0007). Kết quả tóm tắt:

| Đánh giá lại | Số | Ý nghĩa |
|---|---|---|
| Agent | 50 | Cần agent thật trên Foundry: nhiều bước, nhiều tool, có ghi cần duyệt |
| Data agent | 25 | Hỏi số liệu trên semantic model: Fabric data agent / tool dữ liệu trong Foundry, không cần logic riêng nhiều |
| RAG | 6 | Hỏi đáp tài liệu cố định, làm bằng RAG thường |
| Code/ML | 49 | Quy tắc, lịch, đối chiếu, ML dự báo: làm bằng dbt / pipeline / Power BI / rule trong Odoo, không cần agent |

Nghĩa là gần **42 %** catalog không nên là custom agent; đưa về đúng đội (Data/ERP) sẽ giảm tải cho Foundry và giảm rủi ro.

**Nhóm ưu tiên** (điểm = 0,40 tác động + 0,35 khả thi + 0,25 dữ liệu sẵn; chi tiết trong file đánh giá):

| Nhóm | Số | Làm khi |
|---|---|---|
| A (≥ 4,0) | 11 | Trước — toàn bộ chỉ đọc, dữ liệu đã có trên semantic/DW: 100, 102, 104 (dashboard dự án, tồn kho, rủi ro), 5 Compliance/Audit, 54 Project Risk, 108 Project Audit, 60 Safety Compliance, 76 Policy Compliance, 105 NL Data Explorer, 115 Inventory Valuation, 121 Concrete Production Cost |
| B (3,2–3,99) | 15 | Sau khi nhóm A chạy ổn. Gồm 10 data agent chỉ đọc P2/P3 (18, 39, 42, 47, 13, dashboard 97–103) và 5 agent P1 có ghi trên dữ liệu ERP (30 Invoice Intake, 12 Sales Approval, 20 Procurement Approval, 33 Payment, 118 Concrete Production Order) |
| C (< 3,2) | 49 | Đợt sau; P2/P3 có ghi, dữ liệu chưa lên Fabric (HR, tài liệu, pháp lý, HSE, tài sản) |
| D | 55 | Không làm agent trên Foundry; chuyển Data/ERP làm bằng code/RAG |

Nhận xét so với catalog: catalog xếp nhiều agent "có ghi" vào Đợt 1 (vd 3 Approval Coordinator, 117 Concrete Load Dispatch, 44–48 dự án). Theo cách chấm ở đây chúng rơi xuống B/C vì cần human-in-the-loop và tool ghi Odoo chưa có. Đề xuất: Đợt 1 chỉ gồm nhóm A + 3 thí điểm; cần user và chủ nghiệp vụ quyết.

**Còn thiếu**
- [ ] Trục *user desirability*: khảo sát nhanh chủ nghiệp vụ cho nhóm A/B (ai dùng, đau ở đâu, có sẵn sàng không).
- [ ] KPI baseline cho 3 thí điểm (23 Inventory Monitor, 30 Invoice Intake, 82 Knowledge Base) và cổng go/no-go cho PoC 11/2026 (Tổng quát §07).
- [ ] Chủ nghiệp vụ duyệt bảng phân loại; sau duyệt cập nhật lại catalog gốc.

### B2 · Technology plan CC1

Quyết định đã chốt (lý do và lịch sử: FDY-0004; hiện thực IaC: FDY-0003):

| Hạng mục | Quyết định |
|---|---|
| Nền tảng | **Microsoft Foundry** là nơi build và chạy mọi agent. Fabric là nguồn dữ liệu; M365/Teams/Power BI là kênh tiêu thụ. Không dùng Copilot Studio, M365 Copilot agent, GPU/container tự host |
| Topology | Cách bố trí tài nguyên theo từng tầng: subscription → resource group → Foundry resource → project → asset. **Giữ tên tạo tay** (chốt 2026-10-29): RG `foundry_dev` (dev + uat chung) và `foundry` (prd), resource `cc1-ai-<env>-resource`; Bicep FDY-0003 nhận quản lý resource có sẵn và tạo thêm log/appi/kv/diag. Sơ đồ: mục "Mô hình dev → uat → prd" bên dưới |
| Project | **Một project cho mọi agent trong mỗi resource**: `cc1-ai-dev` · `cc1-ai-uat` · `cc1-ai-prd` (đã có). Tách project riêng `cc1-ai-<nhóm>-<env>` khi agent cần dữ liệu lớp c2 trở lên hoặc có ghi vào Odoo, vì project là ranh giới RBAC duy nhất trong resource. Asset bên trong: bảng "Asset trong Foundry" bên dưới |
| Subscription | dev + uat: sub tài trợ Microsoft (Sponsored, 5.000 USD; tên thật trong `infra/README.md`). prd: sub trả phí (CSP; tên thật trong `infra/README.md`) ngay từ đầu. Foundry resource không move được giữa subscription, nên subscription là tham số trong `infra/config/<env>.json`; hết credit thì tạo lại dev/uat ở sub mới bằng Bicep |
| Mạng | Không private networking. Public networking ở cả ba stage; `disableLocalAuth` (chỉ Entra ID) ở uat/prd. Hiện cả ba resource còn bật API key |
| Model | `gpt-5`, `gpt-5.4-mini`, `text-embedding-3-large`; thêm sau. Chưa stage nào có deployment. Kiểu deployment (GlobalStandard hay Data Zone APAC) và capacity từng stage chưa chốt |
| Code | Mọi thành phần (agent, workflow, tool, IaC, config) nằm trong `ai-workspace/projects/foundry`; portal chỉ để xem |

**Asset trong Foundry và quy tắc đặt tên** (quy ước §14 Tổng quát, điều chỉnh tên project theo quyết định trên; **chưa asset nào được tạo**)

Asset ở **cấp resource** dùng chung cho mọi project trong resource đó; asset ở **cấp project** là nơi agent sống. Tên asset không mang env/region, giống hệt ở ba stage; chữ thường, số, `-`, bắt đầu bằng chữ, ≤64 ký tự.

| Cấp | Asset | Mã | Ví dụ (§14) | Ghi chú |
|---|---|---|---|---|
| Resource | Model deployment | tên model; `<model>-next` (ứng viên, chỉ dev); `<model>-strict` (c3) | `gpt-5.4-mini`, `gpt-5.4-mini-next` | Quota, capacity ở đây; chỉ Lead tạo |
| Resource | Connection | `conn-<kind>[-<target>]`, kind ∈ `kv` `appi` `srch` `kb` `mcp` | `conn-kv`, `conn-appi`, `conn-kb-catalog` | Target đổi theo stage, tên không đổi |
| Resource | Guardrail / blocklist | `rai-<profile>` / `bl-<topic>[-vi]` | `rai-standard`, `bl-profanity-vi` | Áp cho deployment hoặc agent |
| Resource | Project | `cc1-ai-<env>`; tách `cc1-ai-<nhóm>-<env>` | `cc1-ai-dev` | Ngoại lệ duy nhất mang env; nằm trong URL endpoint, bất biến |
| Project | Agent | `agt-NNNN`, NNNN = ID catalog 4 chữ số (chốt 2026-10-29) | `agt-0100` (Project Dashboard Agent) | Tên agent trên Foundry **không đổi được sau khi tạo**, tối đa 63 ký tự (Learn [Agent development lifecycle](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [Configure agent](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/configure-agent), kiểm 2026-10-29); mô tả ngắn đặt ở trường description (đổi được), tên cho người dùng đặt ở tên hiển thị Teams |
| Project | Knowledge base (Foundry IQ) | `kb-<usecase>[-<scope>]` | `kb-catalog`, `kb-sop-antoan` | Một kb cho mỗi nhóm người dùng cùng quyền đọc |
| Project | Knowledge source | `ks-<kind>-<content>`, kind ∈ `blob` `web` `idx` `onelake` `sp` | `ks-blob-catalog`, `ks-sp-quytrinh` | Index/indexer tự sinh giữ tên tự sinh |
| Project | Vector store (File Search) | `vs-<usecase>[-<content>]` | `vs-catalog` | Dựng lại ở mỗi stage từ corpus trong Git |
| Project | Toolbox / routine | `tbx-<usecase>` / `rtn-<usecase>-<purpose>` | `tbx-catalog`, `rtn-catalog-smoke` | |
| Project | Dataset / evaluation | `ds-<usecase hoặc shared>-<suite>` / `eval-<usecase>-<suite>` | `ds-catalog-golden`, `eval-catalog-golden` | Version dataset = git tag |
| Kênh | Tên hiển thị Teams | `CC1 · <tên nghiệp vụ>` (+ "(UAT)") | "CC1 · Tra cứu dữ liệu" | Bot Service nằm ngoài Foundry, tên theo §13 |

Asset thuộc một agent dùng cùng số: `agt-0100` → `vs-0100`, `kb-0100`, `ds-0100-golden`. Agent ngoài catalog lấy số tiếp theo từ 0131.

**Mô hình dev → uat → prd**

Trạng thái 2026-10-29 (kiểm bằng `az resource list`):

| Sub | Hiện có trên Azure | Bicep sẽ thêm (what-if dev OK, chưa apply) | Việc còn lại |
|---|---|---|---|
| Tài trợ | RG `foundry_dev`: `cc1-ai-dev-resource` (project `cc1-ai-dev`), `cc1-ai-uat-resource` (project `cc1-ai-uat`, log/appi tự sinh); 3 resource tạo nhầm `project-cc1-dev-*`, `proj-cc1-*`. Chưa có deployment | tag §16; `cc1-ai-dev-resource-logs`, `-appinsights`; `kv-cc1-ai-<env>-sea`; diagnostic | `deploy.ps1 -Env dev -Apply`, rồi uat; xoá 3 resource tạo nhầm |
| CSP | RG `foundry`: `cc1-ai-prd-resource` (chưa deployment); `foundry-cc1` + `ai001`–`ai004` là thí nghiệm, giữ lại | tag §16; `kv-cc1-ai-prd-sea`; diagnostic; `lock-nodelete` | `deploy.ps1 -Env prd -Apply` |

Asset bên trong project (agent, kb, vs, eval): chưa tạo ở stage nào.

Sơ đồ pipeline dev → uat → prd. Ký hiệu: không dấu = có trên Azure · `[B]` = trong Bicep, chưa apply · `[Q]` = quy ước §14, chưa có code tạo:

```
Tenant Entra ID CC1 · Sub tài trợ (dev, uat) · Sub CSP (prd)

                ┌─ DEV (sandbox) ──────┐ ┌─ UAT (pre-prod) ─────┐ ┌─ PRD (deploy-only) ──┐
                │ sub tài trợ          │ │ sub tài trợ          │ │ sub CSP              │
                │ RG foundry_dev       │ │ RG foundry_dev       │ │ RG foundry           │
FOUNDRY         │ ┌──────────────────┐ │ │ ┌──────────────────┐ │ │ ┌──────────────────┐ │
(resource)      │ │ cc1-ai-dev-      │ │ │ │ cc1-ai-uat-      │ │ │ │ cc1-ai-prd-      │ │
                │ │ resource         │ │ │ │ resource         │ │ │ │ resource         │ │
                │ └──────────────────┘ │ │ └──────────────────┘ │ │ └──────────────────┘ │
                │  deployment [B]      │ │  deployment [B]      │ │  deployment [B]      │
                │  · gpt-5             │ │  ← same              │ │  ← same              │
                │  · gpt-5.4-mini      │ │    capacity uat      │ │    capacity prd      │
                │  · text-embedding-3- │ │                      │ │                      │
                │    large             │ │                      │ │                      │
                │  conn-kv · conn-appi │ │  ← same [Q]          │ │  ← same [Q]          │
                │  [Q]                 │ │                      │ │                      │
                └──────────────────────┘ └──────────────────────┘ └──────────────────────┘

                ┌──────────────────────┐ ┌──────────────────────┐ ┌──────────────────────┐
PROJECT         │ ┌──────────────────┐ │ │ ┌──────────────────┐ │ │ ┌──────────────────┐ │
(agent + asset) │ │   cc1-ai-dev     │ │ │ │   cc1-ai-uat     │ │ │ │   cc1-ai-prd     │ │
                │ └──────────────────┘ │ │ └──────────────────┘ │ │ └──────────────────┘ │
                │  asset [Q]           │ │  asset [Q]           │ │  asset [Q]           │
                │  · agt-NNNN          │ │  ← same items        │ │  ← same items        │
                │    (NNNN = ID)       │ │    (cùng tên)        │ │    (cùng tên)        │
                │  · kb-* · ks-* · vs-*│ │                      │ │                      │
                │  · ds-*-golden       │ │                      │ │  → Teams             │
                │  · eval-* · rai-*    │ │                      │ │                      │
                └──────────────────────┘ └──────────────────────┘ └──────────────────────┘

                  deploy ───── golden test → tag ───── approve ──────────────→ promote
                  (PR merge)                           (chủ nghiệp vụ)        (chỉ pipeline)

Khác biệt:  API key    bật (hiện có) │  bật, sẽ tắt [B]      │  bật, sẽ tắt [B]
            lock       không         │  không                │  lock-nodelete [B]
            sửa tay    được          │  không                │  không
            kênh       playground    │  Teams "(UAT)" [Q]    │  Teams [Q]
```

Hiện trạng theo tầng (tên thật) và đích (`[B]` Bicep `infra/config/<env>.json`, `[Q]` quy ước §14):

| Tầng | dev hiện có | uat hiện có | prd hiện có | Đích (chưa tạo) |
|---|---|---|---|---|
| Subscription | sub tài trợ (Sponsored) | cùng dev | sub trả phí (CSP) | giữ nguyên |
| Resource group | `foundry_dev` | `foundry_dev` | `foundry` | giữ nguyên |
| Foundry resource | `cc1-ai-dev-resource` | `cc1-ai-uat-resource` | `cc1-ai-prd-resource` | giữ nguyên, thêm tag [B] |
| Log / App Insights | không có | `cc1-ai-uat-resource-logs`, `-appinsights` (portal tự sinh) | `cc1-ai-prd-resource-logs`, `-appinsights` (portal tự sinh) | dev: tạo `cc1-ai-dev-resource-logs`, `-appinsights`; uat/prd: Bicep nhận quản lý cái có [B] |
| Key Vault | không có | không có | không có | `kv-cc1-ai-<env>-sea` [B] |
| API key | bật | bật | bật | tắt ở uat/prd [B] |
| Lock | không | không | không | `lock-nodelete` ở prd [B] |
| Deployment | không có | không có | không có | gpt-5 · gpt-5.4-mini · text-embedding-3-large [B], đang `deployModels=false` |
| Connection | không có | không có | không có | `conn-kv` · `conn-appi` [Q] |
| Project | `cc1-ai-dev` | `cc1-ai-uat` | `cc1-ai-prd` | giữ nguyên |
| Asset trong project | không có | không có | không có | `agt-* kb-* vs-* ds-* eval-*` [Q] |

**Promote** (GitHub Actions, một pipeline; chưa có workflow): PR merge → deploy DEV → golden test → tag → deploy UAT → approve → deploy PRD. Bicep lo resource · project · deployment; Python lo agent · kb · eval · publish.

**Luồng dữ liệu và kênh** (plan, chưa nối gì):

```
Fabric bi_<env> (semantic model, RLS/OLS)  ── Fabric data agent tool ──→  Foundry resource <env>  (data agent, chỉ đọc)
Fabric ai_<env> (lh_ai_docs, lh_ai_output) ── OneLake indexer / Blob ──→  kb-* · vs-*             (RAG, c0–c1)
Odoo (API chính thức)                      ── MCP server đã có ────────→  agt-* nhóm B/C          (đọc trước, ghi qua cổng duyệt)
Project cc1-ai-prd                         ── Bot Service ─────────────→  Teams "CC1 · <tên nghiệp vụ>"
Foundry resource <env> (trace, metric)     ── diag ────────────────────→  Log Analytics / App Insights của stage
```

Stage của Foundry khớp stage của Fabric: `cc1-ai-dev` đọc `ai_dev`/`bi_dev`, `cc1-ai-prd` đọc `ai_prd`/`bi_prd`; không chéo stage.

**Lưu ý:**
- Deployment, quota, connection, mạng ở **cấp resource**; agent, knowledge, evaluation, trace ở **cấp project**. Ba resource nên quota tách hẳn.
- Sub tài trợ hết credit: tạo lại dev/uat ở sub mới bằng cùng Bicep; prd không bị ảnh hưởng.
- DR prd (cold, Japan East) là quy ước Tổng quát §02, chưa có code tạo.

Lệch với tài liệu nền cần sửa khi cập nhật phiên bản Tổng quát: §02/§13 (subscription không còn là một sub có sẵn; giữ tên tạo tay `foundry_dev`/`foundry`, `cc1-ai-<env>-resource`, log/appi `<foundry>-logs`/`-appinsights` thay cho `rg-/aif-/log-/appi-cc1-ai-<env>-sea`), §14 (tên project `cc1-ai-<env>` thay `proj-<usecase>`, điều kiện tách project; mã agent `agt-NNNN` thay `agt-<usecase>`, asset con cùng số), §07 (kiểu deployment).

Việc còn mở:
- [ ] Chốt kiểu deployment và capacity theo stage, rồi bật `deployModels` trong config (FDY-0003 Q4).
- [ ] Hỏi Pacisoft: sub tài trợ có đổi offer sang Pay-As-You-Go được không (FDY-0003 Q6).
- [ ] Ghi kết quả thí nghiệm đã chạy trên `foundry-cc1` và `ai001`–`ai004` (cái gì chạy được, hạn chế).

### B3 · Sẵn sàng tổ chức CC1

**Ba vai** — đã đồng ý chia team theo 3 vai. Chức năng nhiệm vụ đề xuất (để gắn người sau):

| Vai | Nhiệm vụ chính | Đầu ra sở hữu | Gợi ý số người |
|---|---|---|---|
| **Platform** | Dựng và giữ 3 Foundry resource, project, IaC, CI/CD, identity/RBAC, mạng, quota, giám sát, chi phí; cưỡng chế luật F1–F10 và guardrail; vận hành runbook | Repo hạ tầng, pipeline, dashboard giám sát, registry agent | 1–2 |
| **Workload** | Mỗi agent/nhóm agent: viết yêu cầu nghiệp vụ, instructions, chọn tool/knowledge, golden test, UAT với chủ nghiệp vụ, tích hợp vào kênh dùng, theo dõi chất lượng sau go-live | Thư mục agent trong repo, bộ test, tài liệu use case | theo đợt; tối thiểu 2 |
| **AI CoE** | Chuẩn chung (template agent, instruction, evaluator), review kiến trúc trước khi lên prd, chính sách RAI, đào tạo nội bộ, đầu mối với chủ nghiệp vụ và Pháp chế/DPO | Bộ chuẩn, lịch đào tạo, quyết định kiến trúc | 1, có thể kiêm |

Ánh xạ vào RACI hiện có (Tổng quát §20): [ ] cần đối chiếu từng dòng RACI với 3 vai; dự kiến Platform = "Platform Owner/Operator", Workload = "Agent Owner", CoE = "Architect/Reviewer".

**Kỹ năng và chứng chỉ** — để xin ngân sách. Tên và mã kiểm trên Microsoft Learn ngày 2026-10-28; giá thi vùng Việt Nam cần xác nhận lại khi lập ngân sách.

| Vai | Chứng chỉ đề xuất | Mục tiêu |
|---|---|---|
| Tất cả | **AI-900** Azure AI Fundamentals | Ngôn ngữ chung, 1–2 tuần |
| Workload, CoE | **AI-102** Azure AI Engineer Associate | Build agent trên Foundry, RAG, evaluation, Content Safety |
| Platform | **AZ-104** Azure Administrator (hoặc AZ-900 nếu mới) + **AZ-400** DevOps Engineer (tuỳ chọn) | Identity, mạng, RBAC, IaC, pipeline |
| Platform, CoE | **SC-900** Security, Compliance & Identity Fundamentals | Entra, Purview, DLP cho FDY-0002 |
| Data (phụ trợ) | **DP-600** Fabric Analytics Engineer / **DP-700** Fabric Data Engineer | Semantic model, OneLake, data agent |
| Bổ sung | Microsoft Applied Skills về generative AI / agent trên Foundry (không cần thi tập trung) | Kiểm chứng kỹ năng nhanh |

Lộ trình học miễn phí: Foundry learning path, tutorial "Build and evaluate an enterprise agent", AI Skills Navigator. Đề xuất ngân sách: [ ] số người × (lệ phí thi + 1 khoá luyện thi nếu cần); [ ] 1 workshop nội bộ 1 ngày sau khi 2–3 người có AI-102.

**Change management** — gợi ý khung đơn giản, phù hợp quy mô team nhỏ và 130 use case trải nhiều phòng ban:

1. **Thông điệp một trang** từ lãnh đạo: vì sao làm, agent *không* thay người duyệt (nguyên tắc "agent chỉ đọc, gợi ý, soạn nháp; mọi ghi vào Odoo phải có người bấm duyệt" đã có trong catalog), lộ trình 4 đợt.
2. **Chủ nghiệp vụ là đồng sở hữu**: mỗi agent nhóm A/B có 1 người nghiệp vụ ký tên trong charter (xem bước 3), tham gia UAT, nhận báo cáo KPI.
3. **Kênh Teams "AI agent CC1"**: thông báo release, hỏi đáp, thu feedback; 1 buổi demo 30 phút mỗi đợt.
4. **Đào tạo người dùng cuối theo đợt**: 1 giờ/nhóm nghiệp vụ, nội dung: hỏi thế nào, agent làm được/không làm được gì, cách báo lỗi.
5. **Đo và công bố**: mỗi quý công bố KPI (số lượt dùng, thời gian tiết kiệm, tỷ lệ trả lời đúng) và ghi nhận phòng ban dùng tốt.
6. **Lo ngại thường gặp**: mất việc, sai số liệu, lộ dữ liệu. Trả lời sẵn bằng: nguyên tắc human-in-the-loop, golden test và citation bắt buộc, phân quyền kế thừa từ semantic model (B4).

Microsoft có bộ "Agent Success Kit" trên adoption.microsoft.com, dùng làm mẫu truyền thông và khảo sát; nhưng nó viết cho M365 Copilot, chỉ lấy phần change management.

### B4 · Kiến trúc dữ liệu CC1

**Hiện trạng** (đã chốt): dữ liệu lịch sử ERP đã nằm trọn trên Data Warehouse Fabric; **giai đoạn đầu chỉ cung cấp cho agent qua semantic model** vì đã được xử lý, mô tả và phân quyền. Khớp Microsoft: agent chỉ đọc data product đã quản trị; semantic model là data product gần nhất CC1 đang có. Tham chiếu: Tích hợp Fabric §3–§6, Tổng quát §08.

**Bảng quyết định truy xuất theo data domain** (đề xuất, điền tiếp theo từng đợt):

| Data domain | Nhóm agent dùng | Cách truy xuất | Nguồn chứng nhận | Permission | Trạng thái |
|---|---|---|---|---|---|
| Số liệu ERP đã mô hình (dự án, tồn kho, tài chính, mua sắm, kinh doanh, bê tông) | Data agent nhóm A/B (100, 102, 104, 105, 115, 121, ...) | **Fabric data agent trên semantic model** làm tool trong Foundry (search bằng ngôn ngữ tự nhiên → DAX/SQL) | Semantic model đã publish, có mô tả measure, DA duyệt | Kế thừa RLS/OLS của semantic model; identity passthrough người hỏi | Đề xuất |
| Giao dịch thời gian thực và ghi vào Odoo (nhóm B/C có ghi) | Agent có ghi | **MCP server Odoo đã có** (truy vấn + ghi). Agent dùng read-only trước; bật ghi sau khi có cổng duyệt | API Odoo chính thức | Agent identity riêng + human-in-the-loop | Có MCP, chưa nối vào Foundry |
| Dữ liệu chưa lên Fabric (HR, HSE, tài sản, pháp lý) | Nhóm C | Chưa cung cấp; chờ đưa vào DW/semantic | — | — | Chưa làm |

Việc cần làm:
- [ ] Liệt kê semantic model hiện có theo domain; ứng viên đầu tiên đủ mô tả để làm data agent: `sm_bi_models` (đã có RLS/OLS 3 persona, cùng phiên bản ở `bi_uat`/`bi_prd`, Tích hợp Fabric Q16). Kiểm các model khác của QTDAXL, TAICHINHXAYLAP, SLA_report, SANLUONGDODANG.
- [ ] Thử 1 Fabric data agent trên `sm_bi_models` làm tool cho agent 100 (nhóm A, điểm cao nhất).
- [ ] MCP server Odoo (đã có, quyền truy vấn và ghi): ghi vào tài liệu nơi chạy, ai sở hữu, cách xác thực; đăng ký làm connection kiểu `conn-mcp` (§14) ở dev với quyền đọc trước.

## Nguồn

- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/business-strategy-plan
- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/technology-solutions-plan-strategy
- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/organization-people-readiness-plan
- https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/data-architecture-plan
- Chứng chỉ: https://learn.microsoft.com/en-us/credentials/ (AI-900, AI-102, AZ-104, SC-900, DP-600, DP-700; kiểm 2026-10-28)
- Agent Success Kit: https://adoption.microsoft.com/ai-agents/success-kit/
