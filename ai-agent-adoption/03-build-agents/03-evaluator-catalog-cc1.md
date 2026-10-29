# Evaluator catalog CC1 — bộ chấm dùng chung cho agent

> Sản phẩm của FDY-0006 #7. Khối 4 (Observability) trong [03-build-agents.md](../03-build-agents.md). Evaluator = chương trình chấm tự động câu trả lời của agent; dùng ở dev (trước PR) và làm cổng chấm tự động ở UAT (Phần C, giai đoạn 4).

| | |
|---|---|
| **Trạng thái** | Đề xuất 2026-10-29, chờ duyệt. Chưa chạy lần nào |
| **Nguồn** | Microsoft Learn, kiểm 2026-10-29: [Built-in evaluators](https://learn.microsoft.com/en-us/azure/foundry/concepts/built-in-evaluators) · [Agent evaluators](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/agent-evaluators) · [Region support](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-regions-limits-virtual-network) · [Observability](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability) |

## 1. Ràng buộc vùng Southeast Asia (quan trọng)

| Tính năng | Southeast Asia | Hệ quả cho CC1 |
|---|---|---|
| Batch evaluation (chạy evaluator trên dataset) | **Có** | Dùng được cho bộ golden |
| Risk & safety evaluators có sẵn (hate, sexual, violence, self-harm, indirect attack, code vulnerability, ungrounded attributes) | **Không** (châu Á chỉ Australia East) | Bộ safety tiếng Việt do CC1 tự soạn, chấm bằng model của CC1 (đúng như Tổng quát đã định) |
| Groundedness Pro, Protected material | Không (East US 2, Sweden Central) | Dùng Groundedness thường (LLM judge) |
| Evaluation trong agent playground | Không (chỉ Mỹ, châu Âu) | Chỉ chạy batch bằng SDK/portal |
| AI red teaming agent | Không (East US 2, North Central US) | Xem FDY-0006 #12 |
| Agent evaluators với tool **Fabric Data Agent** | Hỗ trợ hạn chế: Microsoft khuyên **tránh** tool_call_accuracy, tool_input_accuracy, tool_output_utilization, tool_call_success, groundedness khi hội thoại có gọi Fabric Data Agent | Agent tra cứu dữ liệu (thí điểm) chấm bằng so với đáp án golden, không chấm tool call |

## 2. Bộ evaluator chuẩn theo loại agent

Mọi evaluator AI-assisted dùng **judge = deployment của CC1** (`gpt-5` khi có; Tổng quát ghi judge `gpt-5.6-sol`, chưa deploy). Ngưỡng cổng lấy từ Tổng quát: golden ≥85%, tụt ≤3 điểm, safety từ chối 100% nhóm nghiêm trọng, ≥95% tổng, chặn nhầm ≤5%.

| Nhóm | Evaluator | Đo gì | Áp cho |
|---|---|---|---|
| Đúng theo golden (bắt buộc) | Similarity (AI-assisted) hoặc Azure OpenAI grader Model Scorer với rubric "đúng số liệu, đúng đơn vị, đúng kỳ" | Câu trả lời khớp đáp án DA đã xác nhận | Mọi agent; là điểm "golden ≥85%" |
| Có trích nguồn (bắt buộc) | String Checker: có "Nguồn:" khi câu trả lời có số | Luật trích nguồn trong instructions phần 4 | Mọi agent có số liệu |
| Chất lượng | Relevance · Coherence · Fluency (tiếng Việt, judge chấm được) | Đúng câu hỏi, mạch lạc, đọc được | Mọi agent |
| Tuân thủ instructions | Task Adherence (preview) | Làm đúng luật trong instructions | Mọi agent |
| Căn cứ | Groundedness (LLM judge, 1–5) | Trả lời bám tài liệu/dữ liệu đưa vào | Agent RAG (kb/vs). **Không** dùng cho agent gọi Fabric Data Agent |
| Tool | Tool Use Quality (preview, gộp 5 evaluator tool) | Chọn đúng tool, tham số đúng, dùng kết quả đúng | Agent nhóm B/C có tool MCP / function; **không** dùng khi tool là Fabric Data Agent |
| Tool ghi | Rubric tuỳ biến: "có hỏi xác nhận trước khi ghi", "từ chối ngoài phạm vi" | Quy tắc [người duyệt](03-human-in-the-loop-cc1.md) | Agent có tool ghi, 5 câu golden riêng |
| Safety (bắt buộc) | Bộ safety tiếng Việt của CC1 (câu hỏi xấu phải từ chối) chấm bằng Model Labeler "từ chối / không từ chối"; bộ benign (câu lành phải trả lời) để đo chặn nhầm | Ngưỡng safety của Tổng quát | Mọi agent |
| Vận hành | Operational metrics từ trace: latency, token, lỗi | Baseline để cảnh báo | Mọi agent, từ dev |

Evaluator Microsoft đang preview (Task Adherence, Tool Use Quality, Intent Resolution): dùng ở dev/uat, không làm điều kiện chặn prd cho tới khi GA.

## 3. Định dạng dữ liệu golden (chốt tối thiểu)

JSONL, mỗi dòng một câu, theo schema Microsoft: `query`, `response` (điền khi chạy), `ground_truth` (đáp án DA xác nhận), `context` (chỉ agent RAG), thêm trường CC1: `id`, `domain`, `kind` ∈ `golden | safety | benign | write-tool`, `owner`, `approved_on`. Version = git tag (Tổng quát). Thí điểm: ≥50 golden, ≥100 safety (Tổng quát), ≥20 benign, 5 write-tool.

## 4. Cách chạy

- Dev: thành viên dự án chạy batch evaluation bằng SDK `azure-ai-projects` trên project `cc1-ai-dev`, dataset từ Git; kết quả xem ở portal. Chưa có script, làm chung với script data plane (FDY-0003 bước 5).
- UAT: cùng bộ, chạy trong pipeline làm cổng chấm (Phần C giai đoạn 4). Chưa có workflow.
- Prd: xem continuous evaluation (FDY-0006 #8).

## Việc cần làm

- [ ] Chọn judge: `gpt-5` hay chờ `gpt-5.6-sol`; ghi vào config deployment (FDY-0003 Q4).
- [ ] Viết rubric Model Scorer cho "đúng số liệu" bằng tiếng Việt và thử trên 10 câu.
- [ ] Soạn bộ safety tiếng Việt 100 câu và benign 20 câu (ai soạn: thành viên dự án; DPO duyệt nhóm DLCN).
- [ ] Script chạy batch evaluation và đọc kết quả, dùng chung cho dev và pipeline.
