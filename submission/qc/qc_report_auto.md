# Báo cáo QC tự động (nháp)

> **Gợi ý** do script tính; bạn xác nhận. Script chỉ đếm theo phiếu kiểm bằng mắt người.

## Mẫu
- 8 ảnh (3 ca khó + 5 ngẫu nhiên), 160 đối tượng.
- taxonomy v1.0

## Số đo
| Chỉ số | Giá trị | Mẫu số |
|---|---|---|
| Lỗi **theo đối tượng** | 7/160 = 4.4% | 160 đối tượng trong mẫu |
| **Ảnh có lỗi** (dùng cho ngưỡng) | 6/8 = 75.0% | 8 ảnh trong mẫu |
| Máy: accepted / edited / deleted | 268/327 = 82.0% / 18/327 = 5.5% / 41/327 = 12.5% | 327 box máy |
| Máy: added | 108/397 = 27.2% | 397 đối tượng sau duyệt |
| Bắt lỗi bẫy | 9/12 = 75.0% | 12 bẫy |

## Top 3 loại lỗi
| Loại lỗi | Số lỗi | Ảnh ví dụ |
|---|---|---|
| sai_vi_tri_kich_thuoc | 4 | img_003.jpg, img_006.jpg, img_010.jpg |
| sai_thuoc_tinh | 1 | img_008.jpg |
| thua | 1 | img_010.jpg |

**Lỗi hệ thống**: 'sai_vi_tri_kich_thuoc' lặp lại -> sửa quy tắc / recipe, không chỉ sửa từng ảnh.

## Giới hạn thống kê
- 6 ảnh lỗi trong 8 ảnh mẫu: cận trên 95% (Wilson) của tỉ lệ ảnh lỗi thật là **93%**. Ngưỡng 2% không thể được **chứng minh** bằng mẫu nhỏ như vậy: hãy nói rõ giới hạn này trong báo cáo.

## Gợi ý quyết định
**HOLD**

- guideline chưa rõ (2 ca) / dữ liệu có vấn đề (0 ảnh) -> giữ lại làm rõ

Cây quyết định: Hold (guideline chưa rõ / dữ liệu hỏng) -> Rework (> ngưỡng, lỗi lặp, bắt bẫy / nhất quán thấp) -> Release.
