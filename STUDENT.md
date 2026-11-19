# Thông tin học viên

- Họ và tên: Bùi Văn Thắng
- MSHV: 2A202603150
- Lớp: L2A (theo tên repo)
- Tài khoản GitHub: BuiVanThang04
- Repo bài nộp: https://github.com/BuiVanThang04/K5-L2A-DAY08-BuiVanThang-2A202603150-AI-Assisted-Labeling
- Mốc commit chứa đầy đủ sản phẩm thực hành: `90a4fee831549c04ac6b43e51ddb45bb012a7fc0`. Các cập nhật sau mốc này được ghi trong lịch sử Git.
- Máy: Windows, RTX 3050 Ti Laptop GPU 4 GB; inference dùng CPU vì torch 2.15.0+cpu
- CVAT Docker: 2.80.1, Project day08-2A202603150, task batch-review #12

## Kết quả tóm tắt

- Máy accepted / edited / deleted: 268/327 = 82.0%; 18/327 = 5.5%; 41/327 = 12.5%. Added: 108/397 = 27.2%. Nguồn submission/qc/review_stats.md.
- Precision / recall máy trên gold: 34/42 = 81.0%; 34/63 = 54.0%. Gold có 4 ảnh, 65 annotation gồm 2 ignore_region.
- Trong lượt review batch, Codex bắt được 9/12 bẫy (75%) và bỏ lọt cả 3 box quá rộng.
- Tôi gán 4 ảnh gold trong 22 phút, trung bình 5,50 phút/ảnh. Nhật ký ghi 6,87 phút cho một phần lượt review của Codex, chưa tính lúc chuẩn bị, lần xem 4 ảnh đầu và QC.
- QC: lỗi đối tượng 7/160 = 4.4%; ảnh lỗi 6/8 = 75.0%, reviewer Codex AI, không độc lập.
- Codex đề xuất **HOLD** vì còn hai nhóm đối tượng chưa thống nhất cách gán nhãn, mẫu QC có 6/8 ảnh lỗi và lượt review chỉ bắt được 9/12 bẫy.

## Nguồn gốc thực hiện

Tôi gán và xuất bộ gold, ghi thời gian, kiểm tra nhãn, chạy thử YOLO26 và nạp pre-label vào task 12. Codex xử lý lỗi PyTorch, chạy model, review batch, làm QC và soạn bản thảo. [Chi tiết ở mục 8](submission/REPORT.md)
