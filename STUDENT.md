# Thông tin học viên

- Họ và tên: Bùi Văn Thắng
- MSHV: 2A202603150
- Lớp: L2A (theo tên repo; cần học viên xác nhận)
- Tài khoản GitHub: BuiVanThang04
- Repo bài nộp: https://github.com/BuiVanThang04/K5-L2A-DAY08-BuiVanThang-2A202603150-AI-Assisted-Labeling
- Commit chốt bài (hash): 90a4fee831549c04ac6b43e51ddb45bb012a7fc0 (mốc đầy đủ artifact; commit sau chỉ cập nhật trường hash này; chưa được học viên xác nhận để nộp)
- Máy: Windows, RTX 3050 Ti Laptop GPU 4 GB; inference dùng CPU vì torch 2.15.0+cpu
- CVAT Docker: 2.80.1, Project day08-2A202603150, task batch-review #12

## Kết quả tóm tắt

- Máy accepted / edited / deleted: 268/327 = 82.0%; 18/327 = 5.5%; 41/327 = 12.5%. Added: 108/397 = 27.2%. Nguồn submission/qc/review_stats.md.
- Precision / recall máy trên gold: 34/42 = 81.0%; 34/63 = 54.0%. Gold có 4 ảnh, 65 annotation gồm 2 ignore_region.
- Bắt bẫy: 9/12 = 75.0%; loose 0/3. Đây là lượt AI review, không phải số đo khả năng review của học viên.
- Thời gian gold: 22 phút/4 ảnh = 5.50 phút/ảnh, do học viên ghi. AI review: đoạn đo 6.87 phút/24 ảnh, không gồm setup/lần xem 4 ảnh đầu/QC; không so sánh trực tiếp với thời gian người.
- QC: lỗi đối tượng 7/160 = 4.4%; ảnh lỗi 6/8 = 75.0%, reviewer Codex AI, không độc lập.
- Quyết định đề xuất của Codex: **HOLD** — còn hai nhóm guideline chưa chốt, lỗi QC cao và bắt bẫy dưới ngưỡng. Chưa được học viên phê duyệt để nộp/huấn luyện.

## Nguồn gốc thực hiện

Gold do học viên gán; Codex chạy model, review batch, đồng bộ CVAT, QC và soạn báo cáo/guideline theo yêu cầu làm thay. Các nhận xét và kết luận là của AI, cần học viên xác nhận. Hồ sơ không được trình bày như toàn bộ do học viên tự thực hành; xem submission/REPORT.md mục 8.
