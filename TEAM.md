# TEAM — Day04, K4-L3B

**Làm cá nhân (thực hành).** Người nộp tự viết và commit phần INDIVIDUAL.

## Thông tin bài nộp

- Tên nhóm: Lê Duy Bảo (cá nhân)
- Người đại diện / MSSV: Lê Duy Bảo / 2A202602749
- Tên repo: `K4-L3-DAY04-LeDuyBao-2A202602749-PromptEngineeringToolCalling`
- URL repo, nhánh nộp, commit chốt: https://github.com/leduybao612003/K4-L3B-Day04-K4-L3-DAY04-LeDuyBao-2A202602749-PromptEngineeringToolCalling — nhánh `main` — commit chốt: *(điền SHA sau khi commit bản artifacts)*
- Deadline áp dụng và link thông báo đổi hạn nếu có: 23:59 ngày làm lab, Asia/Ho_Chi_Minh theo [SUBMISSION.md](SUBMISSION.md); chưa có thông báo đổi hạn.

## Thành viên

| Họ và tên | MSSV | GitHub | Vai trò và công việc | File/commit/PR |
|---|---|---|---|---|
| Lê Duy Bảo | 2A202602749 | leduybao612003 | Thực hành cá nhân: chạy eval v0, phân tích failure, cải thiện `system_prompt.md` và `tools.yaml`, ghi `version_log.csv` (v0–v3) và `REPORT.md` | `starter_v0/artifacts/*`, `TEAM.md`, `starter_v0/runs/v0_B_base_openrouter_20260915T192013211108.json` |

## Nhận xét chung

- Kết quả và bằng chứng: Run base hợp lệ duy nhất là `starter_v0/runs/v0_B_base_openrouter_20260915T192013211108.json` (`provider_error_cases == 0`, 18/30 PASS, `case_accuracy = 0.6`). `version_log.csv` đủ v0–v3: metric chỉ điền 0.6 ở v0; v1–v3 ghi artifact hash hiện tại (`v3+p16c61a353735+tca2de71c2a7f`) và run 429, `metric_after` để trống. Chi tiết: [starter_v0/artifacts/REPORT.md](starter_v0/artifacts/REPORT.md).
- Thay đổi hiệu quả nhất (giả thuyết, chưa đo lại): làm rõ luôn truyền `environment` khi user nêu môi trường; `clarify` khi thiếu ID; `create_ticket` chỉ sau xác nhận đúng payload hiện tại. Artifact sau phân tích nằm ở `system_prompt.md` và `tools.yaml`.
- Giới hạn còn lại: chưa có run v1–v3 / adversarial / group hợp lệ; chưa UI và transcript live; OpenRouter free hết quota ngày.
- Cách phân công và tích hợp: một người làm toàn bộ vòng phân tích và viết artifact.

## INDIVIDUAL

### Lê Duy Bảo — 2A202602749

- Phần việc và file/commit/PR: chạy `run_eval.py` suite base v0; đọc tool result/error trong JSON; sửa `starter_v0/artifacts/system_prompt.md`, `tools.yaml`, `version_log.csv` (v0–v3), `REPORT.md`; khai báo TEAM.
- Quyết định, khó khăn và cách xử lý: giữ Helpdesk starter thay vì đổi lĩnh vực. Không dùng run 429 làm điểm số. Không hard-code case ID vào prompt; chỉ rút quy tắc từ pattern lỗi (thiếu arg, thiếu clarify, vượt boundary ticket).
- Điều đã học: routing PASS không đủ — thiếu `environment=production` hoặc `check` sai vẫn FAIL; `create_ticket` có thể trả `needs_confirmation` nên phải đọc `tool_results`, không chỉ tên tool.
- AI/công cụ đã dùng và cách kiểm tra: Cursor Grok để soạn artifact/report từ run JSON; tự đối chiếu `summary` và từng case fail trong file run v0, không bịa metric.
- Thời điểm đã tự nộp URL repo chung trên VLearn: *(điền sau khi nộp)*
