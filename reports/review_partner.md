# Review bài bạn cùng nhóm

Người gán: `______ (điền tên)`   Người kiểm: `Lý Hồng Phúc`   Ngày: `16-09-2026`

Đã nhận nhãn gốc (`cvat-day2/train/*.txt`, 20 file) và chạy đủ `check_pose_labels.py`,
`visualize_pose.py`, `visibility_report.py`. Chi tiết từng mục xem `reports/REVIEWER_CHECKLIST.md`.

## Lỗi tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_02` | 1 | `nose, left_eye, right_eye, left_ear, right_ear` | Cả 5 khớp mặt bị gắn `v=0` trong khi đầu vẫn nằm trong khung - chỉ quay đi, không phải ra khỏi ảnh | Đổi thành `v=1`, đặt chấm ước lượng theo hướng đầu đang quay |
| `train_04` | 2 (cô gái bên phải) | `left_hip, right_hip` | Hông bị gắn `v=0` dù thân trên vẫn hiển thị đầy đủ đến ngang hông - chỉ bị ba lô/ghi đông che | Đổi thành `v=1`, đặt chấm ước lượng giải phẫu |
| `train_04` | 1 và 2 | `left_ear, right_ear` | Cả 4 tai (2 người) bị gắn `v=0` vì mũ bảo hiểm che kín | Đổi thành `v=1` cho cả 4 |
| `train_11` | 1 | `left_wrist, right_wrist` | Cổ tay bị gắn `v=0`, nhưng người vẫn trong khung, tay chỉ bị con mèo trước mặt che khuất | Đổi thành `v=1`, ước lượng theo hướng cánh tay |

*(Không tính lỗi: `train_01` cũng có 4 khớp chân `v=0` nhưng đã soi ảnh - ảnh thật sự bị cắt
ngay dưới hông, nên `v=0` ở đây là đúng.)*

Còn 11/15 file bị `check_pose_labels.py` cảnh báo chưa soi kỹ từng ảnh (chỉ mới xem 4/20 ảnh
bằng mắt): `train_06, train_09, train_10, train_12, train_13, train_15, train_19, train_20` và
2 dòng còn lại của `train_01`. Số liệu tổng ở dưới cho thấy đây nhiều khả năng cùng một lỗi lặp lại.

## Nhận xét dựa trên số liệu tổng hợp

So `reports/visibility_report.md` (mình) với `reports/visibility_report_partner.md` (họ, chạy
lại trực tiếp từ nhãn gốc - khớp đúng số họ đã gửi ban đầu) - cùng 20 ảnh, 28 skeleton:

| | Mình | Họ | Chênh lệch |
| --- | ---: | ---: | ---: |
| v=2 (nhìn rõ) | 340 | 330 | -10 |
| v=1 (bị che) | 103 | 48 | **-55** |
| v=0 (ngoài khung) | 33 | 98 | **+65** |

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (50% vs 18%), `right_ear` (36% vs 11%), `left_wrist`
  (36% vs 14%), `nose`/`left_eye`/`right_eye` (21-25% vs 0-4%) - chi tiết ở `reports/visibility_compare.md`.
- **Kết luận (đã xác nhận bằng mắt, không còn là giả thuyết)**: họ đang dùng `v=0` (Outside) ở
  nhiều chỗ lẽ ra phải là `v=1` (Occluded) - đúng lỗi số 3 GUIDE/README mô tả. 15/20 file bị
  `check_pose_labels.py` cảnh báo, và 4 ca soi bằng mắt đều xác nhận đúng là lỗi thật, không phải
  cảnh báo nhầm.

## Việc cần làm tiếp

1. Gửi bảng "Lỗi tìm được" ở trên cho bạn cùng nhóm, kèm ảnh `outputs/vis_review/` để họ mở
   đúng chỗ sửa.
2. Soi nốt 8 file còn nghi vấn (`train_06, 09, 10, 12, 13, 15, 19, 20`) nếu có thời gian - nhiều
   khả năng cùng lỗi, nhưng chưa xác nhận bằng mắt từng ảnh.
3. Thảo luận để thống nhất guideline rõ ràng hơn cho "bị che" vs "ra khỏi khung", đặc biệt vùng
   mặt khi quay đầu và tai khi đội mũ bảo hiểm - ghi vào `GUIDELINE_MINI.md` của cả hai.
4. Xin file export COCO gốc của họ nếu cần kiểm mục 7 (51 số/người) trong `REVIEWER_CHECKLIST.md`.
