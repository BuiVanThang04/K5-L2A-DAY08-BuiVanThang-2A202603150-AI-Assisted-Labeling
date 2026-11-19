# Label contract — BỔ SUNG v1.1 — 2A202603150 - Bùi Văn Thắng

> Codex AI soạn từ lượt review thực tế, chưa được học viên xác nhận. Chỉ ghi phần bổ sung so với v1.0. Trạng thái HOLD. Không đổi taxonomy.rules.

## 1. Câu hỏi mở và quy tắc vận hành tạm thời

| Câu hỏi | Quy tắc | Ảnh ví dụ | Case |
|---|---|---|---|
| Hàng hóa và giỏ cố định | Box gồm khung và giỏ gắn cố định; loại hàng rời nhô quá khoảng 20% chiều dài xe. Mask chỉ gồm vật liệu xe và giỏ, không tô người hay hàng rời. | img_009, img_031 | CAL-01 |
| Kính và khoảng hở | Kính thuộc xe; khoảng hở không phải vật liệu xe. Người nhìn thấy được gán riêng. Giữ quy tắc lỗ nhỏ 5% ở v1.0. | img_004, img_013 | CAL-03 |
| Confidence để Accept | Không có điểm nào tự động cho phép Accept. Kiểm lớp và bốn cạnh của mọi box; điểm chỉ để ưu tiên kiểm. | img_024, img_029 | CAL-04 |
| Box chồng nhau | Một box mỗi xe gồm mui/khung; người có box riêng. Chỉ bỏ proposal thực sự trùng cá thể, không gộp hai xe liền nhau. | img_003, img_013, img_017 | CAL-02 |
| Cụm xe gần camera | Vật tách được và cao từ 20 px vẫn gán riêng. Ignore chỉ cho cụm thực sự không tách được; không che vật rõ. Rà lại vùng ignore của lượt AI review trước Release. | img_005, img_018, img_024 | CAL-04 |
| Người bị mui che | Giữ ngưỡng còn thấy ít nhất 20% như v1.0; đầu người đơn độc không đủ nếu phần cơ thể còn lại không nhìn thấy. Ca giáp ngưỡng cần người kiểm. | img_013, img_017 | CAL-02 |

## 2. Ca khó và lỗi cần sửa

| Ảnh | Quan sát | Xử lý | Case |
|---|---|---|---|
| img_003, img_013, img_017 | Proposal bao riêng mái và thân cùng xe | Bỏ proposal trùng, giữ box cả xe. Đây là áp dụng quy tắc cá thể đã có. | CAL-02 |
| img_014, img_004, img_034 | Lọt 3/3 bẫy box lỏng | Kiểm bốn cạnh ở mức zoom phù hợp. Giữ nguyên số đo lượt đầu sau khi biết đáp án. | CAL-05 |
| img_008, img_023, img_027 | Không xác nhận được số bánh/phạm vi xe đẩy và xích lô | Giữ can_review và HOLD đến khi người kiểm chốt; không mặc định mọi xe đẩy là three_wheeler. | CAL-06 |
| img_004, img_013 | Mask thân xe đã có nhưng thiếu mui | Thêm prompt/sửa biên; giữ ảnh thử làm bằng chứng. Chưa chứng minh IoU mask đạt 0.85. | CAL-03 |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do |
|---|---|---|
| v1.0 | Bản giảng viên, giữ nguyên | Căn cứ ban đầu |
| v1.1 nháp AI, 2026-11-19 | Làm rõ hàng hóa, kính, overlap, confidence; ghi ca chưa chốt | CAL-01 đến CAL-06; chưa được học viên xác nhận |

## 4. Hai nhóm câu hỏi chưa chốt

1. Xe đạp điện/xe máy điện: chưa có ca được xác nhận để chốt cách phân loại trong lượt này.
2. Xe đẩy/xe chở hàng khi không thấy rõ số bánh: cần người xác nhận phạm vi taxonomy, đặc biệt img_008, img_023, img_027.

Vì vậy qc_report.py chạy với --guideline-gap 2. Taxonomy thực thi vẫn là v1.0; phần bổ sung này chưa được phê duyệt để huấn luyện.
