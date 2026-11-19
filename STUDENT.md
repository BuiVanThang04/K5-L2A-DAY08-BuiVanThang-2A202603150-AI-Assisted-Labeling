# Thông tin học viên

- Họ và tên: Bùi Văn Thắng
- MSHV: 2A202603150
- Lớp: L2A (theo tên repo; cần học viên xác nhận)
- Tài khoản GitHub: BuiVanThang04
- Repo bài nộp: https://github.com/BuiVanThang04/K5-L2A-DAY08-BuiVanThang-2A202603150-AI-Assisted-Labeling
- Mốc commit chứa đầy đủ sản phẩm thực hành: `90a4fee831549c04ac6b43e51ddb45bb012a7fc0`. Các cập nhật sau mốc này được ghi trong lịch sử Git.
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

Bùi Văn Thắng tự gán và xuất 4 ảnh gold, ghi thời gian, chạy kiểm tra nhãn, chạy thử YOLO26 trên tập pilot và nạp pre-label vào CVAT task 12. Codex xử lý lỗi PyTorch, chạy các thí nghiệm đã ghi log, review 24 ảnh batch, thực hiện QC và soạn bản thảo báo cáo cùng guideline. Phân tích số đo và quyết định HOLD trong bản thảo do Codex đề xuất. [Chi tiết ở mục 8](submission/REPORT.md)
