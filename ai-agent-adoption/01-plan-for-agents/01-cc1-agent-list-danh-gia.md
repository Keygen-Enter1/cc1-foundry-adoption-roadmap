# Đánh giá lại 130 agent CC1 · code / RAG / data agent / agent và trọng số ưu tiên

> Nguồn gốc: cc1-agent-list.pdf (EA Final AI Agent Catalog 130 v1.0, 16/10/2026). File này **sinh tự động** từ PDF + bảng phân loại thủ công; sửa phân loại thì sửa ở cột "Đánh giá lại" rồi báo để tính lại điểm.

> Thuộc [01-plan-for-agents.md](../01-plan-for-agents.md) §B1. Trạng thái: **đề xuất**, chờ user và chủ nghiệp vụ duyệt.

## Cách đánh giá

**Đánh giá lại** (theo Microsoft: khi nào không cần agent):

- **Code/ML** — quy tắc/lịch/ML cố định, không cần suy luận nhiều bước.
- **RAG** — hỏi đáp trên tài liệu cố định, không cần tool.
- **Data agent** — hỏi số liệu trên semantic model đã có, dùng Fabric data agent / tool dữ liệu.
- **Agent** — nhiều bước, nhiều tool hoặc đầu vào mơ hồ, có ghi cần duyệt.

**Điểm ưu tiên** = 0,40 × Tác động + 0,35 × Khả thi + 0,25 × Dữ liệu sẵn (thang 1–5).

| Trục | Cách chấm |
|---|---|
| Tác động | P1 = 5 · P2 = 3 · P3 = 1 (lấy từ catalog) |
| Khả thi | Chỉ đọc = 5 · Có ghi cần duyệt = 2; Data agent +1 (semantic đã có); Agent có ghi −1 (cần human-in-the-loop, tool ghi Odoo) |
| Dữ liệu sẵn | Báo cáo 5 · ERP (cung ứng, kinh doanh, bê tông, mua sắm, tài chính) 4 · Dự án, điều phối 3 · HR, tài liệu, pháp lý, HSE, tài sản, hỗ trợ nội bộ 2 (chưa lên Fabric) · Thí điểm +1 |

**Nhóm ưu tiên** (chỉ cho Data agent và Agent): **A** ≥ 4,0 làm trước · **B** 3,2–3,99 · **C** < 3,2 làm sau · **D** = Code/ML hoặc RAG, không cần custom agent trên Foundry (làm bằng dbt/pipeline/Power BI/Odoo rule hoặc RAG thường).

Trục "mong muốn của người dùng" (user desirability) chưa chấm vì chưa khảo sát; khi có thì thêm trục thứ tư.

## Tổng hợp

| Đánh giá lại | Số agent |
|---|---|
| Agent | 50 |
| Data agent | 25 |
| RAG | 6 |
| Code/ML | 49 |

| Nhóm ưu tiên | Số agent |
|---|---|
| A | 11 |
| B | 15 |
| C | 49 |
| D | 55 |

| Nhóm nghiệp vụ | Agent | Data agent | RAG | Code/ML | Tổng |
|---|---|---|---|---|---|
| Báo cáo và phân tích | 0 | 9 | 0 | 1 | 10 |
| Chuỗi cung ứng | 4 | 1 | 0 | 7 | 12 |
| Phát triển kinh doanh | 3 | 1 | 0 | 4 | 8 |
| Sản xuất bê tông | 1 | 1 | 1 | 3 | 6 |
| Mua sắm | 5 | 1 | 1 | 1 | 8 |
| Tài chính – kế toán – thuế | 5 | 2 | 0 | 10 | 17 |
| Dự án và xây dựng | 7 | 6 | 0 | 2 | 15 |
| Điều phối và quản trị AI | 5 | 1 | 0 | 1 | 7 |
| Hỗ trợ và vận hành nội bộ | 3 | 0 | 0 | 4 | 7 |
| Hợp đồng, pháp lý và quản trị doanh nghiệp | 3 | 0 | 0 | 3 | 6 |
| Nhân sự | 6 | 1 | 1 | 4 | 12 |
| Tài liệu và tri thức | 2 | 0 | 3 | 4 | 9 |
| Chất lượng, HSE và nghiệm thu | 5 | 1 | 0 | 1 | 7 |
| Tài sản, thiết bị và bảo trì | 1 | 1 | 0 | 4 | 6 |

## Bảng đầy đủ (sắp theo nhóm ưu tiên rồi điểm)

| Nhóm | Điểm | ID | Tên | Nhóm nghiệp vụ | Catalog | Loại | Đánh giá lại | Tác động | Khả thi | Dữ liệu | Ghi chú |
|---|---|---|---|---|---|---|---|---|---|---|---|
| A | 5.00 | 100 | Project Dashboard Agent | Báo cáo và phân tích | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 5 | Số liệu dự án: tiến độ, chi phí, rủi ro |
| A | 5.00 | 102 | Inventory Dashboard Agent | Báo cáo và phân tích | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 5 | Số liệu tồn kho: tồn, nhập, xuất, hàng chậm luân chuyển |
| A | 5.00 | 104 | Risk & Compliance Dashboard Agent | Báo cáo và phân tích | P1 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 5 | Số liệu rủi ro và tuân thủ: ngoại lệ, vi phạm, cảnh báo |
| A | 4.50 | 5 | Compliance/Audit Agent | Điều phối và quản trị AI | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 3 | Kiểm tra lịch sử thao tác, phân quyền, bằng chứng và dấu vết phê duyệt |
| A | 4.50 | 54 | Project Risk Agent | Dự án và xây dựng | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 3 | Nhận diện rủi ro tiến độ, chi phí và hợp đồng |
| A | 4.50 | 108 | Project Audit Agent | Dự án và xây dựng | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Data agent | 5 | 5 | 3 | Kiểm tra kế hoạch kiểm toán, phát hiện, bằng chứng và hành động khắc phục của dự án. |
| A | 4.25 | 60 | Safety Compliance Agent | Chất lượng, HSE và nghiệm thu | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Agent | 5 | 5 | 2 | Kiểm tra hồ sơ và cảnh báo vi phạm an toàn |
| A | 4.25 | 76 | Policy Compliance Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Agent | 5 | 5 | 2 | Đối chiếu giao dịch với quy chế nội bộ |
| A | 4.20 | 105 | Natural-language Data Explorer | Báo cáo và phân tích | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Data agent | 3 | 5 | 5 | Người dùng hỏi dữ liệu bằng tiếng Việt và nhận bảng/biểu đồ |
| A | 4.05 | 115 | Inventory Valuation Agent | Chuỗi cung ứng | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Data agent | 5 | 3 | 4 | Tính, kiểm tra và giải thích giá trị tồn kho theo phương pháp và kho. |
| A | 4.05 | 121 | Concrete Production Cost Agent | Sản xuất bê tông | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Data agent | 5 | 3 | 4 | Tập hợp và phân tích chi phí vật liệu, sản xuất và chi phí mua của bê tông. |
| B | 3.95 | 18 | Supplier Comparison Agent | Mua sắm | P2 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Data agent | 3 | 5 | 4 | So sánh giá, điều khoản, năng lực và lịch sử nhà cung cấp |
| B | 3.95 | 39 | Expense Audit Agent | Tài chính – kế toán – thuế | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Agent | 3 | 5 | 4 | Kiểm tra đề nghị thanh toán, tạm ứng và hoàn ứng |
| B | 3.95 | 42 | P&L Reporting Agent | Tài chính – kế toán – thuế | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Data agent | 3 | 5 | 4 | Lập báo cáo lãi/lỗ theo tháng |
| B | 3.70 | 47 | Progress Monitoring Agent | Dự án và xây dựng | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Data agent | 3 | 5 | 3 | Tổng hợp tiến độ và cảnh báo công việc trễ |
| B | 3.60 | 30 | Invoice Intake Agent | Tài chính – kế toán – thuế | P1 · Thí điểm · T12/26 | Có ghi – cần duyệt | Agent | 5 | 1 | 5 | Đọc PDF/XML, trích xuất và tạo hóa đơn nháp |
| B | 3.40 | 97 | Executive Dashboard Agent | Báo cáo và phân tích | P3 · Đợt 3 · S6 · T3/27 | Chỉ đọc | Data agent | 1 | 5 | 5 | Số liệu điều hành tổng hợp cho lãnh đạo |
| B | 3.40 | 98 | Sales Dashboard Agent | Báo cáo và phân tích | P3 · Đợt 3 · S6 · T3/27 | Chỉ đọc | Data agent | 1 | 5 | 5 | Số liệu kinh doanh: pipeline, doanh số, khách hàng |
| B | 3.40 | 99 | Finance Dashboard Agent | Báo cáo và phân tích | P3 · Đợt 4 · S7 · T4/27 | Chỉ đọc | Data agent | 1 | 5 | 5 | Số liệu tài chính: doanh thu, chi phí, công nợ, dòng tiền |
| B | 3.40 | 101 | Procurement Dashboard Agent | Báo cáo và phân tích | P3 · Đợt 4 · S7 · T4/27 | Chỉ đọc | Data agent | 1 | 5 | 5 | Số liệu mua sắm: yêu cầu mua, đơn mua, nhà cung cấp |
| B | 3.40 | 103 | HR Dashboard Agent | Báo cáo và phân tích | P3 · Đợt 4 · S8 · T4/27 | Chỉ đọc | Data agent | 1 | 5 | 5 | Số liệu nhân sự: biên chế, chấm công, tuyển dụng |
| B | 3.35 | 12 | Sales Approval Agent | Phát triển kinh doanh | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 4 | Kiểm tra giá, chiết khấu, hạn mức và xin phê duyệt |
| B | 3.35 | 20 | Procurement Approval Agent | Mua sắm | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 4 | Kiểm tra ngân sách, thẩm quyền và luồng phê duyệt |
| B | 3.35 | 33 | Payment Agent | Tài chính – kế toán – thuế | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 4 | Lập đề nghị thanh toán và ghi nhận thanh toán |
| B | 3.35 | 118 | Concrete Production Order Agent | Sản xuất bê tông | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 4 | Tạo nháp, kiểm tra vật tư và theo dõi lệnh sản xuất bê tông. |
| B | 3.25 | 13 | Customer 360 Agent | Phát triển kinh doanh | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 4 | Tổng hợp hồ sơ, giao dịch, công nợ và lịch sử tương tác |
| C | 3.15 | 43 | Financial Analysis Agent | Tài chính – kế toán – thuế | P3 · Đợt 4 · S8 · T4/27 | Chỉ đọc | Data agent | 1 | 5 | 4 | Phân tích doanh thu, chi phí, biên lợi nhuận và biến động |
| C | 3.10 | 1 | Enterprise AI Orchestrator | Điều phối và quản trị AI | P1 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Nhận yêu cầu, phân loại nghiệp vụ và điều phối agent chuyên môn |
| C | 3.10 | 2 | E2E Process Agent | Điều phối và quản trị AI | P1 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Chạy quy trình xuyên phòng ban theo E2E, SOP và workflow |
| C | 3.10 | 3 | Approval Coordinator | Điều phối và quản trị AI | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Thu thập hồ sơ, chuyển phê duyệt, nhắc hạn và kiểm tra thẩm quyền |
| C | 3.10 | 6 | Master Data Steward | Điều phối và quản trị AI | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Kiểm tra trùng lặp, chuẩn hoá và đề xuất sửa master data |
| C | 3.10 | 7 | Management Copilot | Điều phối và quản trị AI | P1 · Đợt 2 · S3 · T2/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Tổng hợp KPI, cảnh báo và đề xuất hành động cho lãnh đạo |
| C | 3.10 | 44 | Project Setup Agent | Dự án và xây dựng | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Tạo dự án, giai đoạn, WBS và đội thực hiện |
| C | 3.10 | 45 | Project Planning Agent | Dự án và xây dựng | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Xây kế hoạch công việc, nguồn lực và mốc tiến độ |
| C | 3.10 | 48 | Construction Daily Log Agent | Dự án và xây dựng | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Tổng hợp nhật ký thi công, nhân lực, thiết bị, thời tiết |
| C | 3.10 | 51 | IPC/Payment Certificate Agent | Dự án và xây dựng | P1 · Đợt 2 · S3 · T2/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Chuẩn bị và kiểm tra hồ sơ thanh toán khối lượng |
| C | 3.10 | 53 | Project Document Controller | Dự án và xây dựng | P1 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 3 | Phân loại và liên kết tài liệu với dự án/gói thầu |
| C | 3.00 | 49 | Site Material Agent | Dự án và xây dựng | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 3 | Theo dõi nhu cầu và sử dụng vật tư công trường |
| C | 3.00 | 50 | Cost Control Agent | Dự án và xây dựng | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 3 | So sánh dự toán, cam kết, chi phí thực tế và dự báo |
| C | 3.00 | 107 | Job Costing Agent | Dự án và xây dựng | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 3 | Lập, cập nhật và phân tích giá thành công trình theo vật tư, nhân công, máy và chi phí chung. |
| C | 2.85 | 126 | Office Correspondence Agent | Tài liệu và tri thức | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Agent | 5 | 1 | 2 | Tiếp nhận, phân loại, luân chuyển và theo dõi công văn đến, đi. |
| C | 2.75 | 70 | Workforce Planning Agent | Nhân sự | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 2 | Phân tích biên chế, tải công việc và nhu cầu tuyển dụng |
| C | 2.75 | 90 | Asset Cost Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Data agent | 3 | 3 | 2 | Phân tích chi phí vòng đời thiết bị |
| C | 2.55 | 10 | Quotation Agent | Phát triển kinh doanh | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Lập báo giá từ yêu cầu khách hàng hoặc tài liệu đính kèm |
| C | 2.55 | 11 | Sales Order Agent | Phát triển kinh doanh | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Tạo và kiểm tra đơn bán hàng |
| C | 2.55 | 16 | Purchase Requisition Agent | Mua sắm | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Tiếp nhận, kiểm tra và điều phối yêu cầu mua hàng |
| C | 2.55 | 17 | RFQ Agent | Mua sắm | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Tạo RFQ, gửi yêu cầu báo giá và theo dõi phản hồi |
| C | 2.55 | 19 | Purchase Order Agent | Mua sắm | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Lập và xác nhận đơn mua |
| C | 2.55 | 28 | Material Requisition Agent | Chuỗi cung ứng | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Xử lý yêu cầu vật tư cho công trường/dự án |
| C | 2.55 | 35 | AR Collection Agent | Tài chính – kế toán – thuế | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Theo dõi công nợ phải thu và nhắc khách hàng |
| C | 2.55 | 41 | Month-end Closing Agent | Tài chính – kế toán – thuế | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Lập checklist, theo dõi và cảnh báo khóa sổ |
| C | 2.55 | 111 | Purchase Agreement Agent | Mua sắm | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Quản lý thỏa thuận mua hàng, nhà cung cấp, thời hạn và các đơn mua phát sinh. |
| C | 2.55 | 112 | Intercompany Supply Agent | Chuỗi cung ứng | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Điều phối giao dịch bán, mua và dịch chuyển kho giữa các công ty thành viên. |
| C | 2.55 | 114 | Inter-Warehouse Transfer Agent | Chuỗi cung ứng | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Lập và theo dõi luân chuyển vật tư, hàng hóa giữa kho và địa điểm dự án. |
| C | 2.55 | 116 | Return Reason Agent | Chuỗi cung ứng | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 4 | Phân loại nguyên nhân trả hàng, theo dõi xử lý và phân tích xu hướng. |
| C | 2.30 | 52 | Variation Order Agent | Dự án và xây dựng | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 3 | Quản lý phát sinh, thay đổi phạm vi và tác động chi phí |
| C | 2.30 | 109 | Subcontractor Contract Agent | Dự án và xây dựng | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 3 | Quản lý hợp đồng thầu phụ, phạm vi, khối lượng, điều khoản và nghĩa vụ thanh toán. |
| C | 2.05 | 55 | QA/QC Inspection Agent | Chất lượng, HSE và nghiệm thu | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Lập checklist, kiểm tra và theo dõi nghiệm thu |
| C | 2.05 | 56 | Material Acceptance Agent | Chất lượng, HSE và nghiệm thu | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Xử lý nghiệm thu vật liệu đầu vào |
| C | 2.05 | 57 | Work Acceptance Agent | Chất lượng, HSE và nghiệm thu | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Xử lý nghiệm thu công việc xây dựng |
| C | 2.05 | 59 | HSE Incident Agent | Chất lượng, HSE và nghiệm thu | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Tiếp nhận, phân loại và điều tra sự cố |
| C | 2.05 | 62 | Recruitment Agent | Nhân sự | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Sàng lọc hồ sơ, lập shortlist và lên lịch phỏng vấn |
| C | 2.05 | 63 | Onboarding Agent | Nhân sự | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Điều phối hồ sơ, thiết bị, đào tạo và bàn giao nhân sự mới |
| C | 2.05 | 65 | Leave Agent | Nhân sự | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Kiểm tra số dư, tạo đơn và điều phối phê duyệt nghỉ phép |
| C | 2.05 | 68 | Performance Appraisal Agent | Nhân sự | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Điều phối đánh giá và tổng hợp phản hồi |
| C | 2.05 | 69 | Training Agent | Nhân sự | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Xác định nhu cầu, lập kế hoạch và theo dõi đào tạo |
| C | 2.05 | 71 | Contract Review Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Trích xuất điều khoản và đánh dấu rủi ro |
| C | 2.05 | 74 | Corporate Governance Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Điều phối nghị quyết, họp và hành động sau họp |
| C | 2.05 | 80 | Document Linking Agent | Tài liệu và tri thức | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Liên kết tài liệu với hợp đồng, hóa đơn, dự án hoặc đối tác |
| C | 2.05 | 87 | Maintenance Triage Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Phân loại yêu cầu sửa chữa và điều phối đội xử lý |
| C | 2.05 | 91 | Helpdesk Triage Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Phân loại ticket, ưu tiên và chuyển nhóm |
| C | 2.05 | 93 | Email Intake Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Đọc email đến và tạo bản ghi nghiệp vụ tương ứng |
| C | 2.05 | 94 | Meeting Action Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Tạo biên bản, nhiệm vụ và nhắc việc sau họp |
| C | 2.05 | 124 | Overtime Planning Agent | Nhân sự | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Agent | 3 | 1 | 2 | Lập kế hoạch tăng ca, đối chiếu chấm công và điều phối phê duyệt. |
| C | 1.95 | 61 | Quality Trend Agent | Chất lượng, HSE và nghiệm thu | P3 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Data agent | 1 | 3 | 2 | Phân tích lỗi lặp lại và đề xuất cải tiến |
| D | 5.00 | 23 | Inventory Monitor Agent | Chuỗi cung ứng | P1 · Thí điểm · T12/26 | Chỉ đọc | Code/ML | 5 | 5 | 5 | Kiểm tra tồn kho, tồn tối thiểu và hàng chậm luân chuyển |
| D | 4.75 | 31 | Invoice Validation Agent | Tài chính – kế toán – thuế | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Code/ML | 5 | 5 | 4 | Kiểm tra MST, số hóa đơn, thuế, trùng lặp và PO |
| D | 4.75 | 40 | Tax Compliance Agent | Tài chính – kế toán – thuế | P1 · Đợt 1 · S1 · T1/27 | Chỉ đọc | Code/ML | 5 | 5 | 4 | Kiểm tra hóa đơn, thuế suất và thời hạn kê khai |
| D | 4.20 | 106 | Scheduled Reporting Agent | Báo cáo và phân tích | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Code/ML | 3 | 5 | 5 | Tự lập và phân phối báo cáo theo lịch |
| D | 3.95 | 21 | Supplier Risk Agent | Mua sắm | P2 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Code/ML | 3 | 5 | 4 | Cảnh báo chậm giao, chất lượng thấp hoặc phụ thuộc nhà cung cấp |
| D | 3.95 | 22 | Tender Assistant | Mua sắm | P2 · Đợt 2 · S3 · T2/27 | Chỉ đọc | RAG | 3 | 5 | 4 | Kiểm tra hồ sơ mời thầu, hồ sơ dự thầu và tiêu chí đánh giá |
| D | 3.70 | 32 | Invoice Posting Agent | Tài chính – kế toán – thuế | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 4 | Chuẩn bị và thực hiện đăng sổ theo quyền |
| D | 3.70 | 82 | Knowledge Base Agent | Tài liệu và tri thức | P2 · Thí điểm · T12/26 | Chỉ đọc | RAG | 3 | 5 | 3 | Tìm kiếm và trả lời dựa trên tài liệu nội bộ |
| D | 3.70 | 117 | Concrete Load Dispatch Agent | Sản xuất bê tông | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 4 | Lập và điều phối chuyến bê tông, tải, phương tiện, thời gian và trạng thái giao nhận. |
| D | 3.70 | 119 | Concrete Mix Design Agent | Sản xuất bê tông | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | RAG | 5 | 2 | 4 | Quản lý cấp phối, vật liệu trộn, định mức và điều kiện áp dụng. |
| D | 3.70 | 122 | Concrete Ticket Agent | Sản xuất bê tông | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 4 | Lập và quản lý phiếu bê tông gắn với chuyến, lệnh sản xuất và giao nhận. |
| D | 3.45 | 4 | Exception & Escalation Agent | Điều phối và quản trị AI | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 3 | Phát hiện SLA trễ, thiếu dữ liệu hoặc sai luồng và chuyển cấp |
| D | 3.45 | 46 | Project Task Agent | Dự án và xây dựng | P1 · Đợt 1 · S2 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 3 | Tạo, phân công và theo dõi nhiệm vụ |
| D | 3.45 | 67 | Payroll Validation Agent | Nhân sự | P2 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Code/ML | 3 | 5 | 2 | Kiểm tra dữ liệu đầu vào bảng lương |
| D | 3.45 | 77 | Document Classification Agent | Tài liệu và tri thức | P2 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Code/ML | 3 | 5 | 2 | Tự động phân loại và gắn metadata |
| D | 3.45 | 78 | Document Naming Agent | Tài liệu và tri thức | P2 · Đợt 1 · S2 · T1/27 | Chỉ đọc | Code/ML | 3 | 5 | 2 | Chuẩn hóa tên, mã và phiên bản tài liệu |
| D | 3.45 | 83 | SOP Assistant | Tài liệu và tri thức | P2 · Đợt 2 · S3 · T2/27 | Chỉ đọc | RAG | 3 | 5 | 2 | Hướng dẫn nhân viên thực hiện đúng SOP/WI |
| D | 3.45 | 92 | SLA Monitoring Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 2 · S4 · T2/27 | Chỉ đọc | Code/ML | 3 | 5 | 2 | Theo dõi thời hạn và tự động escalation |
| D | 3.45 | 110 | Project Schedule Import Agent | Dự án và xây dựng | P1 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 3 | Nhập, kiểm tra và đối chiếu kế hoạch dự án từ Microsoft Project hoặc Excel. |
| D | 3.20 | 66 | Attendance Agent | Nhân sự | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 2 | Phát hiện thiếu công, sai ca và bất thường chấm công |
| D | 3.20 | 125 | Attendance Device Agent | Nhân sự | P1 · Đợt 1 · S1 · T1/27 | Có ghi – cần duyệt | Code/ML | 5 | 2 | 2 | Theo dõi máy chấm công, người dùng thiết bị, đồng bộ dữ liệu và lỗi truyền nhận. |
| D | 3.15 | 14 | Sales Forecast Agent | Phát triển kinh doanh | P3 · Đợt 4 · S8 · T4/27 | Chỉ đọc | Code/ML | 1 | 5 | 4 | Dự báo doanh thu và khả năng chốt theo pipeline |
| D | 3.15 | 37 | Cashflow Forecast Agent | Tài chính – kế toán – thuế | P3 · Đợt 4 · S8 · T4/27 | Chỉ đọc | Code/ML | 1 | 5 | 4 | Dự báo dòng tiền theo hợp đồng và chứng từ |
| D | 2.90 | 8 | CRM Lead Agent | Phát triển kinh doanh | P2 · Đợt 2 · S3 · T2/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Tạo, phân loại và chấm điểm khách hàng tiềm năng |
| D | 2.90 | 9 | Opportunity Follow-up Agent | Phát triển kinh doanh | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Theo dõi cơ hội, nhắc chăm sóc và chuyển giai đoạn |
| D | 2.90 | 15 | Contract Renewal Agent | Phát triển kinh doanh | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Theo dõi hợp đồng sắp hết hạn và khởi tạo gia hạn |
| D | 2.90 | 24 | Replenishment Agent | Chuỗi cung ứng | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Đề xuất mua/bổ sung hàng theo nhu cầu |
| D | 2.90 | 25 | Receiving Agent | Chuỗi cung ứng | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Đối chiếu PO, phiếu nhận và số lượng thực nhận |
| D | 2.90 | 26 | Delivery Agent | Chuỗi cung ứng | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Chuẩn bị và xác nhận giao hàng |
| D | 2.90 | 27 | Stock Exception Agent | Chuỗi cung ứng | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Phát hiện âm kho, sai vị trí, sai lô/serial |
| D | 2.90 | 29 | Logistics Tracking Agent | Chuỗi cung ứng | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Theo dõi tiến độ vận chuyển và cảnh báo trễ |
| D | 2.90 | 34 | Three-way Matching Agent | Tài chính – kế toán – thuế | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Đối chiếu PO, nhận hàng và hóa đơn |
| D | 2.90 | 36 | AP Payable Agent | Tài chính – kế toán – thuế | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Quản lý công nợ phải trả và lịch thanh toán |
| D | 2.90 | 38 | Budget Control Agent | Tài chính – kế toán – thuế | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Kiểm tra ngân sách trước cam kết chi phí |
| D | 2.90 | 113 | Landed Cost Allocation Agent | Chuỗi cung ứng | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Phân bổ vận chuyển, bốc dỡ, bảo hiểm và chi phí nhập hàng vào giá trị tồn kho. |
| D | 2.90 | 120 | BOM Version Control Agent | Sản xuất bê tông | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Quản lý phiên bản định mức, hiệu lực và lịch sử thay đổi. |
| D | 2.90 | 128 | Borrowing Loan Agent | Tài chính – kế toán – thuế | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Quản lý hợp đồng vay, kế hoạch giải ngân, lãi và lịch hoàn trả. |
| D | 2.90 | 129 | Lending Loan Agent | Tài chính – kế toán – thuế | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Quản lý khoản cho vay, giải ngân, tính lãi và thu hồi. |
| D | 2.90 | 130 | Debt Offset Agent | Tài chính – kế toán – thuế | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 4 | Lập, kiểm tra và theo dõi bù trừ công nợ giữa các đối tác hoặc công ty. |
| D | 2.65 | 81 | Document Summary Agent | Tài liệu và tri thức | P3 · Đợt 4 · S8 · T4/27 | Chỉ đọc | RAG | 1 | 5 | 2 | Tóm tắt file/thư mục và trích xuất điểm chính |
| D | 2.40 | 58 | NCR/CAPA Agent | Chất lượng, HSE và nghiệm thu | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Ghi nhận không phù hợp và theo dõi hành động khắc phục |
| D | 2.40 | 64 | Employee Service Agent | Nhân sự | P2 · Đợt 2 · S4 · T2/27 | Có ghi – cần duyệt | RAG | 3 | 2 | 2 | Trả lời chính sách và tra cứu thông tin nhân sự |
| D | 2.40 | 72 | Contract Obligation Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Theo dõi nghĩa vụ, bảo lãnh và mốc hợp đồng |
| D | 2.40 | 73 | Legal Case Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Quản lý vụ việc, hồ sơ và thời hạn pháp lý |
| D | 2.40 | 75 | Entity Portfolio Agent | Hợp đồng, pháp lý và quản trị doanh nghiệp | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Quản lý pháp nhân, công ty con và giấy phép |
| D | 2.40 | 79 | Document Request Agent | Tài liệu và tri thức | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Yêu cầu người phụ trách bổ sung hồ sơ còn thiếu |
| D | 2.40 | 84 | Records Retention Agent | Tài liệu và tri thức | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Quản lý thời hạn lưu trữ, khóa và tiêu hủy hồ sơ |
| D | 2.40 | 85 | Asset Register Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Tạo và chuẩn hóa hồ sơ tài sản |
| D | 2.40 | 86 | Preventive Maintenance Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Lập lịch bảo trì định kỳ |
| D | 2.40 | 88 | Equipment Inspection Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S7 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Theo dõi checklist kiểm định |
| D | 2.40 | 89 | Fleet Agent | Tài sản, thiết bị và bảo trì | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Quản lý xe, nhiên liệu, booking và sửa chữa |
| D | 2.40 | 95 | Internal Reminder Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S5 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Nhắc việc theo lịch hoặc điều kiện dữ liệu |
| D | 2.40 | 96 | Workspace Organizer Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Tạo thư mục, di chuyển và chuẩn hóa tài liệu |
| D | 2.40 | 123 | Recruitment Request Agent | Nhân sự | P2 · Đợt 4 · S8 · T4/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Lập, duyệt và theo dõi nhu cầu tuyển dụng theo đơn vị và vị trí. |
| D | 2.40 | 127 | Meeting Room Agent | Hỗ trợ và vận hành nội bộ | P2 · Đợt 3 · S6 · T3/27 | Có ghi – cần duyệt | Code/ML | 3 | 2 | 2 | Tra cứu, đặt và kiểm soát lịch sử dụng phòng họp. |
