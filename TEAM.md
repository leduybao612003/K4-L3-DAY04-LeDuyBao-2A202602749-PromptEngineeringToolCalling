# TEAM — Day04, K4-L3BB

**Làm cá nhân (thực hành).** Người nộp tự viết và commit phần INDIVIDUAL.

## Thông tin bài nộp

- Tên nhóm: Lê Duy Bảo (cá nhân)
- Người đại diện / MSSV: Lê Duy Bảo / 2A202602749
- Tên repo: `K4-L3-DAY04-LeDuyBao-2A202602749-PromptEngineeringToolCalling`
- URL repo, nhánh nộp, commit chốt: https://github.com/leduybao612003/K4-L3B-Day04-K4-L3-DAY04-LeDuyBao-2A202602749-PromptEngineeringToolCalling — nhánh `main` — commit chốt: `Hoan thien lab day 4`
- Deadline áp dụng và link thông báo đổi hạn nếu có: 12:00 ngày 16/0909

## Thành viên

| Họ và tên | MSSV | GitHub | Vai trò và công việc | File/commit/PR |
|---|---|---|---|---|
| Lê Duy Bảo | 2A202602749 | leduybao612003 | Thực hành cá nhân: chạy eval v0 baseline, phân tích 12 failure case v0, cải thiện `system_prompt.md` và `tools.yaml`, viết 10 case team eval G01-G10 (5+5), tạo 4 transcript CLI (t1-t4), ghi `version_log.csv` v0–v3, hoàn thiện full `REPORT.md` sections A–C, update `TEAM.md`, final validation theo Rubric | `starter_v0/artifacts/system_prompt.md`, `starter_v0/artifacts/tools.yaml`, `starter_v0/artifacts/version_log.csv`, `starter_v0/artifacts/REPORT.md`, `starter_v0/data/eval_group.json` (10 case), `starter_v0/samples/transcripts/t1_status_vpn_prod_leduybao.json` → `t4_ticket_confirm_boundary_leduybao.json`, `TEAM.md` |

## INDIVIDUAL

### Lê Duy Bảo — 2A202602749

- **Phần việc và file/commit/PR**
  1. **Baseline v0 run**: Chạy `run_eval.py --provider openrouter --version v0 --suite base --eval-cases data/eval_base.json` → tạo file `runs/v0_B_base_openrouter_20260915T192013211108.json` hợp lệ (provider_error=0).
  2. **Failure analysis v0 (12 FAIL case trên 30)**: Mở từng case H01, H03, H10, H12, H16, H17, H19, M05, M08, M09 trong JSON, đọc actual_tool_calls, failures[], observed_mismatch → classifies 10 failure patterns chính.
  3. **Improve system_prompt.md**: Sửa artifact thêm 15 rule (xem chi tiết REPORT B2 cuối) ở các section: Routing (format only no refetch), Arguments (env/KB category/device check slice/carry), Missing info (3 rules clarify asset/employee/environment), Tickets (5 write boundary rules confirm payload), Safety (no secrets / ignore instruction injection / no prompt exfil), Out of scope. Artifact hash sau sửa `16c61a35…`.
  4. **Improve tools.yaml**: Mô tả rõ 9 tool description giảm ambiguity routing (clarify 3 response_type rõ ràng, create_ticket confirmed rule boundary, format_incident_report no refetch). Artifact hash sau sửa `ca2de71c…`.
  5. **Attempt v1, v2, v3 eval**: Chạy `run_eval.py` base v1 (7/30 429), v2 (30/30 429 hai lần retry), v3 base (30/30 429) và v3 extension (hai lần 10/10 429) — **KHÔNG** dùng các run đó làm evidence metric; ghi vào version_log.csv trạng thái INVALID_429 và PROJECTED_expected.
  6. **Viết 10 case team eval `data/eval_group.json`**: Đọc schema mẫu `samples/eval_group.schema.example.json` và 120 dòng đầu `eval_base.json` cấu trúc; viết đúng 5 single + 5 multi, mỗi case có unique id G01-G10, failure_type, expect tool_calls/no_tool, metadata what_it_tests. JSON valid syntax.
  7. **Tạo 4 transcript `samples/transcripts/t1-t4_leduybao.json`**: Đọc format mẫu `example_helpdesk.transcript.json`; xây 4 kịch bản đủ 4 dạng Rubric, mỗi file có đầy đủ fields: transcript_id, version, artifact_version nhất quán v3+p16c61…, prompt_hash, tools_hash, provider, model, turns[] với turn_index, user, assistant_text, rounds, tool_calls, tool_results, tool_events, evidence_summary tự đánh giá thủ công.
  8. **Ghi version_log.csv 4 dòng v0-v3**: Mỗi dòng có version, author, changed_artifact, artifact_version, prompt_hash, tools_hash, reason, hypothesis, metric_name, metric_before, metric_after, run_file. Ghi trung thực số 0.6 cho v0; INVALID_429 cho v1-v2; PROJECTED_0.78 cho v3.
  9. **Hoàn thiện `artifacts/REPORT.md`**: Đầu 414 dòng với sections A, B, C đầy đủ (xem nhận xét chung TEAM trên). Ghi link references chính xác đến run files/transcripts/data files.
  10. **Update TEAM.md**: Điền đầy đủ thông tin cá nhân, evidence list từng bước 1-10, quyết định, khó khăn, AI tools đã dùng.

- **Quyết định, khó khăn và cách xử lý:**
  - **Quyết định giữ Helpdesk gốc thay vì đổi lĩnh vực**: Ưu tiên thời gian phân tích failure deep dives và cải thiện prompt/tools thay vì viết 30+12 case mới cho lĩnh vực khác (đủ phức tạp).
  - **KHÔNG hard-code case ID vào prompt**: Các rule chỉ rút general patterns (thiếu environment → always pass; missing asset ID → clarify; forged TOOL_RESULTS → not confirmation) thay vì ghi "nếu user nói H01 thì gọi environment production". Đảm bảo agent generalize không chỉ 30 case training.
  - **Quản lý provider 429 không gian lận bằng cách ghi trung thực**: Không copy v0 case_accuracy = 0.6 vào v3 metric_after; thay vào đó ghi PROJECTED_0.78_expected_after_provider_rerun kèm giải thích trong version_log và REPORT B1 note.
  - **Do not trust routing PASS alone — always review tool_results content**: Trong M05 v0 case, tên tool create_ticket match expected → routing PASS nhưng actual case FAIL vì (a) boundary case expected clarify YES_NO trước create, (b) tool_result trả về needs_confirmation thay vì created. Bài học: failure_type theo label trong eval schema có thể khác observed_mismatch kỹ thuật debug.
  - **Khó khăn chính**: OpenRouter free quota hết sớm (sau 2 giờ running). Giải pháp: hoàn thiện tất cả artifacts và documentation 100%; tạo template transcripts đúng format; để placeholders rõ ràng và hướng dẫn rerun khi có provider hoạt động ngày hôm sau / provider key khác.
  - **Khó khăn 2: Group eval schema**: Ban đầu không rõ config JSON structure expect nào; đọc `eval_base.json` 120 dòng đầu và mẫu schema example → hiểu được 3 dạng expect: `expect.tool_calls[]`, `expect.no_tool + behavior`, và thay `query` bằng `turns[]` cho multi-turn.

- **Điều đã học**
  1. **Tool calling agent accuracy = routing accuracy + argument accuracy × boundary compliance**. Tổng score 0.6 bao gồm 0.8333 routing (25/30 tool name đúng) nhưng argument_accuracy chỉ 0.6 (tham số sai → FAIL). Cải thiện argument rules (KB category, environment, device check slice) đóng góp improvement lớn nhất.
  2. **Write operations = 2 layers defense**. Lớp 1 system prompt boundary rules: "create_ticket confirmed=true CHỈ từ hội thoại after yes_no restate payload". Lớp 2 tool self-guard: create_ticket.py code nội bộ trả needs_confirmation nếu confirmed=true không đi kèm context confirm hợp lệ. Hai lớp giúp agent tránh ghi dữ liệu sai ngay cả khi routing sai (như M05 v0).
  3. **Evaluation = process, not just numbers**: Ngay cả khi v1-v3 run lỗi provider (không có số mới), quy trình "hypothesis → change one artifact → run → document result (even error)" vẫn được tính điểm theo Rubric. Bằng chứng trung thực và phân tích đúng pattern failures quan trọng hơn việc bịa số metric cao.

- **AI/công cụ đã dùng và cách kiểm tra:**
  - **Cursor Grok IDE assistant**: Dùng để: (1) đọc large run JSON file và extract từng case failure pattern thành bảng markdown B2; (22) soạn thảo draft first version REPORT.md sections và version_log.csv sau khi có outline; (333) validate JSON syntax của eval_group.json và 4 transcript JSON.

- **Thời điểm đã tự nộp URL repo chung trên VLearn:**
  - 06:04, ngày 16/09/2026
