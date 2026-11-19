# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 327; đối tượng sau duyệt: 397; ngưỡng accepted: IoU >= 0.9
- Đã **loại 24 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 268 | 268/327 = 82.0% | 327 box máy |
| edited (sửa) | 18 | 18/327 = 5.5% | 327 box máy |
| deleted (xoá) | 41 | 41/327 = 12.5% | 327 box máy |
| added (vẽ thêm) | 108 | 108/397 = 27.2% | 397 đối tượng sau duyệt |

Trong số deleted + added, **9 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 99.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 157 | 4 | 6 | 33 |
| bicycle | 2 | 1 | 2 | 6 |
| motorbike | 73 | 5 | 2 | 41 |
| car | 25 | 0 | 5 | 19 |
| bus | 2 | 0 | 0 | 3 |
| truck | 1 | 0 | 1 | 1 |
| three_wheeler | 8 | 8 | 25 | 5 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 63 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 34 | 8 | 29 | 81.0% (KTC 95%: 67–90%) | 54.0% (KTC 95%: 42–66%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 9/12 = 75.0% (KTC 95%: 47–91%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - wrong_class: 3/3
  - extra: 3/3
  - missing: 3/3
  - loose: 0/3
  - lọt: img_014.jpg (loose), img_004.jpg (loose), img_034.jpg (loose)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
