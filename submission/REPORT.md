# Báo cáo Day 08 — 2A202603150 - Bùi Văn Thắng

> Bùi Văn Thắng tự gán và xuất bộ gold, chạy kiểm tra nhãn và nạp pre-label cho batch vào CVAT. Codex thực hiện lượt review batch, QC và soạn bản phân tích. Quyết định đề xuất cho lô nhãn là **HOLD**.

## 1. Model và recipe (Part 2–3)

| Lớp | Model | Ngưỡng | Căn cứ |
|---|---|---|---|
| person, bicycle, motorbike | YOLO26-n, CPU | 0.30 | r01/r02/r05; sweep không đạt recall 90%, giữ mặc định theo script |
| car | YOLO26-n, CPU | 0.45 | Sweep: recall 8/8 = 100%, precision 8/12 = 66.7% trên gold |
| bus, truck | YOLO26-n, CPU | 0.30 | Gold có 0 bus, 1 truck; không đủ 5 vật để chọn ngưỡng riêng |
| three_wheeler | YOLOE-26s-seg, CPU | 0.15 | r03/r06; prompt tuk-tuk, cyclo, cargo tricycle; recall 7/7 = 100%, precision 7/9 = 77.8% |

Nguồn: model_log.csv và qc/threshold_sweep.*. Số sweep trước bước gộp/loại proposal chồng lấn.

Lệnh gộp thực tế:

```powershell
python scripts/merge_prelabels.py work/prelabel/yoloe_all/pre.coco.json work/prelabel/y26_all/pre.coco.json --thr "car=0.45,three_wheeler=0.15" --conf 0.3 --iou 0.6 --split "batch,gold" --out work/prelabel/merged_review/pre.coco.json
```

SAM: **SAM 2.1 tiny**, sam2.1_t.pt, để giảm chi phí CPU. Máy có RTX 3050 Ti nhưng torch hiện là bản CPU. Tour detector-to-SAM tạo 8 mask/2 ảnh; 3 ảnh point-prompt lưu riêng ở masks/.

Quan sát AI: img_002 có tuk-tuk bị YOLO26 gọi truck ở confidence 0.37. Tăng lên 0.5 chỉ làm mất proposal. YOLOE tìm tuk-tuk ở 0.93 nhưng cũng gọi nhầm xe bán tải bên phải là three_wheeler 0.37. Đổi ngưỡng không sửa được phạm vi lớp. Cần review từng proposal.

Chỉ có 4 ảnh gold, chọn và chấm trên cùng dữ liệu nên số đo lạc quan. Ngưỡng là gợi ý thô; confidence không phải xác suất đúng đã được hiệu chuẩn.

## 2. AI error profile

Nguồn: qc/review_stats.md và .json, từ lượt AI review đầu đã export khỏi CVAT. Script loại 24 đối tượng quanh bẫy khỏi profile.

| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---:|---:|---|
| accepted | 268 | 82.0% | 327 box máy được tính |
| edited | 18 | 5.5% | 327 box máy được tính |
| deleted | 41 | 12.5% | 327 box máy được tính |
| added | 108 | 27.2% | 397 đối tượng sau review được tính |

Có 9 trường hợp đổi lớp tính deleted + added; số bỏ sót thật ước tính theo script là 108 - 9 = 99. Đây là so với nhãn AI review, không phải ground truth độc lập.

three_wheeler có 25 deleted, 8 edited, 8 accepted; lỗi trùng/sai lớp thấy ở img_003, img_013, img_014. motorbike có 41 added, nhất là cảnh đông/đêm. Không suy rộng lớp nào luôn tệ nhất ngoài tập này.

So với gold: TP=34, FP=8, FN=29. Precision máy **34/42 = 81.0%**, recall **34/63 = 54.0%**. Gold có 65 annotation nhưng chỉ 63 đối tượng trong mẫu số recall sau khi bỏ ignore_region. Chỉ 4 ảnh nên kết luận yếu.

## 3. Thiên kiến tự động hóa và ảnh bẫy

Bắt **9/12 = 75.0%**, dưới yêu cầu ít nhất 10/12. wrong_class 3/3, extra 3/3, missing 3/3, loose **0/3**. Bẫy lọt: img_014, img_004, img_034. Nguồn: qc/review_stats.md.

Key chỉ được script đọc sau khi batch_reviewed.json đã được export/lưu. Không sửa pre-label, key hoặc số đo lượt đầu sau khi biết đáp án.

Đánh giá AI: accepted 82% không chứng minh review tốt; bước kiểm sát bốn cạnh chưa chặt. Đây là lỗi của lượt AI review, không phải phép đo thiên kiến của học viên. Không kết luận học viên đã/không đã làm đúng checklist từ số này.

## 4. Máy có đáng dùng không?

Nguồn time_log.csv: gold_manual **22 phút/4 ảnh = 5.50 phút/ảnh**, do học viên cung cấp. batch_review là **6.87 phút/24 ảnh = 0.286 phút/ảnh** của đoạn AI review được đo, gồm đồng bộ CVAT, **không gồm setup, lần xem 4 ảnh đầu và QC**. Đây không phải thời gian review của học viên. Khác người và phạm vi nên không tính phần trăm tiết kiệm từ hai dòng.

Đánh giá tạm thời của AI: pre-label hữu ích để tạo ứng viên, nhưng chưa chứng minh tiết kiệm tổng thời gian ở chất lượng đạt chuẩn. Recall gold 54% và QC còn lỗi cao yêu cầu kiểm lại. Học viên cần tự xác nhận kết luận.

Ba quy tắc đề xuất để học viên xác nhận:

1. Xem ảnh gốc không nhãn ở 24/24 ảnh trước khi đối chiếu proposal; ghi audit từng ảnh.
2. Kiểm đủ bốn cạnh mỗi box ở mức zoom phù hợp, đặc biệt img_004, img_014, img_034.
3. Quét riêng người lái và xe, ghi vật bổ sung; không dùng accepted rate làm thước đo chất lượng duy nhất.

## 5. Mask SAM cho 3 vật

Prompt và nhận xét do Codex thực hiện, tọa độ pixel ảnh gốc.

| Ảnh | Model | Click | Phạm vi | Lỗi quan sát | File |
|---|---|---|---|---|---|
| img_013 | SAM 2.1 tiny | 2 dương, 1 âm | Thân/bánh xe, loại người lái chính | Mui thiếu nhiều pixel | masks/tuktuk_img013.jpg |
| img_004 | SAM 2.1 tiny | 2 dương, 1 âm | Tuk-tuk gần nhất, loại người bên phải | Mui thiếu mảng, khoảng hở cần kiểm | masks/tuktuk_img004.jpg |
| img_003 | SAM 2.1 tiny | 2 dương, 2 âm | Thùng xe và bánh, loại phần lớn người/hàng | Thiếu mui, dính một phần áo trẻ | masks/cyclo_img003.jpg |

Lệnh tái lập:

```powershell
python scripts/sam2_points.py --image img_013.jpg --pos "450,440" --pos "350,570" --neg "745,300" --sam sam2.1_t.pt --out submission/masks --name tuktuk_img013
python scripts/sam2_points.py --image img_004.jpg --pos "735,425" --pos "740,535" --neg "885,365" --sam sam2.1_t.pt --out submission/masks --name tuktuk_img004
python scripts/sam2_points.py --image img_003.jpg --pos "505,480" --pos "395,560" --neg "330,300" --neg "550,365" --sam sam2.1_t.pt --out submission/masks --name cyclo_img003
```

Guideline 4.2 xác định phần nhìn thấy, bóng đổ và người riêng. CAL-01/CAL-03 bổ sung hàng và kính. Chưa có mask gold để đo IoU; các ảnh này là kết quả thử có lỗi, chưa chứng minh đạt 0.85. Không thực hiện Interactor SAM tùy chọn.

## 6. Calibration

Có **6 quyết định** tại qc/calibration_decisions.csv, gồm cau_hoi_mo, loi_may, bay_lot và can_review. Codex đề xuất, chờ học viên xác nhận. Quy tắc ở guidelines/guideline_v1.1_calibrated.md.

CAL-01 làm rõ hàng; CAL-03 kính/khoảng hở; CAL-04 confidence. CAL-02 và CAL-05 củng cố quy tắc có sẵn, không biến lỗi model/reviewer thành định nghĩa lớp mới. CAL-06 giữ mở phạm vi xe đẩy/số bánh. Hai nhóm chưa chốt: xe điện và xe đẩy khi không thấy rõ số bánh.

Nguyên nhân gồm guideline chưa rõ, lỗi máy và lỗi review AI. Không sửa taxonomy.rules. Taxonomy thực thi vẫn v1.0; v1.1 là phần bổ sung nháp chưa được phê duyệt.

## 7. QC và quyết định

Đã chạy build_release.py, qc_sample.py --n 5 --seed 42, xem các ảnh render rồi điền qc_review.csv. Mẫu **8/24 ảnh: 3 can_review + 5 ngẫu nhiên**, 160 đối tượng. Reviewer và QC đều là Codex; **không có kiểm tra độc lập của người**.

Nguồn: qc/qc_sample.csv, qc/qc_review.csv, qc/qc_report_auto.md.

- Lỗi đối tượng **7/160 = 4.4%**; ảnh lỗi **6/8 = 75.0%**, vượt ngưỡng 2%.
- Sai vị trí/kích thước: 4 lỗi; img_003 #17, img_006 #67, img_010 #91, img_035 #413. ID theo ảnh QC release candidate.
- Sai thuộc tính: img_008 #70 thiếu can_review cho xe đẩy mơ hồ.
- Thừa: img_010 #93 chưa đủ bằng chứng là người riêng.
- Sai lớp: img_018 #163 ghi bicycle thay vì motorbike.

Cận trên Wilson 95% của tỉ lệ ảnh lỗi là **93%** theo script. Mẫu nhỏ, gồm ca khó chọn có chủ đích và không có reviewer độc lập; không suy rộng như lấy mẫu ngẫu nhiên đơn giản toàn bộ dữ liệu.

Script kiểm cấu trúc, lớp, đủ ảnh và chênh lệch file; không xác nhận được box ôm sát, lớp đúng thực tế, hay vật cùng bị model và AI bỏ sót. check_submission.py sạch không có nghĩa được Release.

**Quyết định đề xuất của Codex: HOLD, ngày 2026-11-19.** Theo Hold → Rework → Release: còn 2 nhóm guideline chưa chốt nên Hold trước. Sau khi chốt, vẫn phải Rework do ảnh lỗi 75% và bắt bẫy 75%. release/ là **candidate bị giữ lại**, chưa được phê duyệt.

Cần người kiểm chốt xe điện/xe đẩy; sửa lỗi QC và bẫy lỏng; rà ignore, cảnh đông/đêm; export sửa thành qc_fixed.json, build và QC vòng mới. Không sửa lịch sử 9/12 thành 100% sau khi biết key. Chưa thực hiện vòng sửa QC thứ hai; giữ lỗi có audit để người kiểm đối chiếu.

## 8. Nguồn gốc thực hiện và ghi chú

- **Bùi Văn Thắng** tự gán box trên 4 ảnh gold trong CVAT, xuất nhãn và chạy `validate_labels.py --all`. Sau khi kiểm tra các cảnh báo về box thấp hơn 20 px, bộ gold cuối có 65 đối tượng và qua bước kiểm tra. Học viên cũng chạy thử YOLO26 trên tập pilot ở ngưỡng 0.25 và 0.5, cung cấp traceback khi PyTorch gặp lỗi và trực tiếp nạp `pre_traps_import.coco.json` vào task 12 để bắt đầu review batch.
- **Codex** xử lý lỗi môi trường, chạy các lượt suy luận đã ghi log, chọn prompt và khảo sát ngưỡng theo lớp. Sau khi học viên nạp pre-label, Codex kiểm tra và chỉnh box trên 24 ảnh, cập nhật task 12, xuất bộ nhãn đã review, thực hiện QC và soạn bản thảo báo cáo cùng phần bổ sung guideline. Các nhận xét và quyết định HOLD đang chờ học viên tự đánh giá. [Nhật ký chỉnh nhãn từng ảnh](qc/ai_review_audit.csv)
- **Thời gian:** gold 22 phút do học viên ghi; batch 6.87 phút là đoạn AI review đo thực, không phải thời gian người, không gồm setup/lần xem 4 ảnh đầu/QC. Không so sánh tốc độ trực tiếp.
- **Máy:** Windows, Python 3.12.6, RTX 3050 Ti 4 GB; torch 2.15.0+cpu nên chạy CPU; CVAT Docker 2.80.1. Không dùng SAM 3.
- **Cache:** không dùng data/cache/; model chạy thật có log. Không sửa ảnh/split.
- **Sửa script:** common.pick_device từng nuốt lỗi torch rồi trả CPU, dẫn tới import lần hai gây hiểu nhầm. Đã giữ traceback gốc. Thiếu VCRUNTIME140_THREADS.dll được khắc phục bằng Microsoft Visual C++ Runtime chính thức.
- **CVAT:** snapshot trước sửa được giữ trong work/review_ai; đồng bộ qua annotation service của CVAT Docker và export bằng exporter COCO của CVAT. Source của shape có thể ghi manual theo API; không dùng trường đó để khẳng định con người đã review.
- **Bài cá nhân:** phần Codex thực hiện vượt phạm vi hỗ trợ AI mà RULES.md mô tả. Hồ sơ này là thực hành có AI hỗ trợ, không được trình bày như toàn bộ do học viên tự làm. Cần học viên xem lại và làm theo yêu cầu giảng viên trước khi nộp.
- **Lần sau:** tách giờ setup/inference/review/QC; kiểm độc lập; ưu tiên biên box và giữ số đo vòng đầu.
