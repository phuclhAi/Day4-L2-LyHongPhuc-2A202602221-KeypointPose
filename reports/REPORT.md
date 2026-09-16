# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: `Lý Hồng Phúc - 2A202602221`   Nhóm: `2A202602221 - 2A202602184`   Ngày: `16-09-2026`

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 (sau rework `train_13`, trước rework là 28) |
| v=2 / v=1 / v=0 | 351 / 109 / 33 (sau rework; trước rework 340 / 103 / 33) |
| Thời gian trung bình mỗi ảnh | 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 50%
2. `right_ear` - 36%
3. `left_wrist` - 36% (đồng hạng với `right_ear`)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có, chúng là những khớp khó gán nhất

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.930 | 0.953 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.931 | 1.000 |
| Lỗi `dao_trai_phai` | 1 (`train_08`) | **0** - hết hẳn lỗi này |
| Lỗi `thieu_nguoi`* | 1 (`train_13`) | 0 - đã thêm đủ 29/29 người |
| Lỗi `nham_nguoi` / `xoa_khop_bi_che` | 0 (không xuất hiện trong `outputs/eval_vs_gold.json`) | 0 |

*Template ghi `nham_nguoi` nhưng tool `evaluate_pose_annotations.py` thực tế trả về khoá
`thieu_nguoi` (1 người trong `train_13` chưa được gán) - không có lỗi nhầm người thật sự.
Xem đủ 5 khoá gốc trong `outputs/eval_vs_gold.json -> summary.findings`.

Theo `RUBRIC.md`, cổng qua bài yêu cầu **OKS trung bình >= 0.75, OKS@0.75 >= 0.70, và không còn
lỗi `dao_trai_phai` nào** - thiếu điều kiện cuối thì bài bị khoá ở mức "Cần rework" bất kể điểm
đẹp thế nào. Sau 2 vòng rework, bài đã đạt cả 3 điều kiện - `evaluate_pose_annotations.py` xác
nhận "Không có skeleton nào cần rework: mọi người đều đạt OKS >= 0.75 và không có lỗi đã phân loại."

**Tôi đã sửa gì qua các lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- Vòng 1: `train_08.jpg` người #1 - đảo trái/phải toàn bộ skeleton (OKS 0.286 -> hết lỗi này)
- Vòng 1: `train_13.jpg` - thêm skeleton cho người thiếu (người nhỏ/mờ phía xa) - 28 -> 29
  skeleton, 0 người thiếu, OKS@0.50 và @0.75 đạt 1.000, nhưng người mới thêm lại bị đảo trái/phải
  (OKS riêng 0.795)
- Vòng 2: `train_13.jpg` người mới thêm (khớp `gold_person 1`) - sửa lại đúng trái/phải
  (`left_hip`/`right_hip` và các khớp liên quan) -> hết sạch lỗi `dao_trai_phai` trong toàn bài,
  OKS trung bình 0.949 -> 0.953

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Xảy ra ở `train_08.jpg` - người ngồi xe máy quay mặt thẳng vào camera, không vặn mình,
không bị che - đây là **ảnh dễ**, không có gì mơ hồ. Đúng như GUIDE cảnh báo trước: "lỗi đảo
trái/phải không xảy ra ở ảnh khó... nó xảy ra ở ảnh dễ, lúc bạn đang làm nhanh" có thể do làm nhanh/chủ quan vì
tưởng ảnh dễ nên không kiểm kỹ

## 3. Kiểm chéo

Bạn cùng nhóm: `Nguyễn Thường Huy`

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm (từ `reports/visibility_compare.md`):

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 50% | 18% | 32 | Gán sai thật - soi bằng mắt (`reports/REVIEWER_CHECKLIST.md`) xác nhận họ dùng `v=0` cho tai bị mũ bảo hiểm che, đáng lẽ `v=1` |
| right_ear | 36% | 11% | 25 | Cùng nguyên nhân với `left_ear` |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- không có luật mới nào cần bổ sung

## 4. Model

Số liệu từ `outputs/eval_model.json` (10 ảnh test).

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

*(Ngoài bảng template: `box_mAP50` cũng giảm nhẹ, 0.9785 -> 0.96, chênh -0.0185.)*

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` **tăng nhẹ** +0.0055 (0.6853 -> 0.6908), không giảm - nên câu hỏi "nếu giảm"
   không áp dụng trực tiếp. Đáng chú ý hơn: `box_mAP50-95` lại **giảm** -0.0078 cùng lúc pose
   tăng. 20 ảnh của bạn nhiều khả năng dạy model một số điều kiện cụ thể của bộ ảnh core (góc
   chụp, kiểu che khuất, tỉ lệ occluded/outside theo đúng guideline nhóm) giúp pose nhích lên
   chút, nhưng đổi lại làm model hơi "quên" một phần khả năng định vị hộp bao tổng quát đã học
   từ COCO gốc - dấu hiệu overfit nhẹ vào 20 ảnh, đúng như cảnh báo của GUIDE "20 ảnh là quá ít
   để ra một model dùng được".

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Ở bản fine-tune: `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) tới **~0.113**
   (tương tự ở bản gốc: 0.8119 vs 0.6853, chênh ~0.127). Model tìm **người** (box) dễ hơn hẳn
   tìm **khớp** (pose). Lý do: định vị box chỉ cần ước lượng vùng bao quanh cả người (sai số vài
   chục pixel vẫn được tính đúng), còn định vị khớp cần đúng từng điểm nhỏ, nhiều khớp lại hay
   bị che/mờ - đúng loại khó khăn mà chính mình gặp khi gán tay (mục 5, ca `train_13`).

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ảnh test 02 - Lỗi nhầm người

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

    Ảnh train 08 có OKS thấp nhất giữa nhãn của tôi và model. Tôi đúng vì tôi đã chấm với gold rồi và tôi đã sửa lại sao cho nó gần khớp với gold nhất

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   **Không trùng.** Theo `outputs/eval_vs_gold.json` (xác nhận bằng số liệu cục bộ), ảnh bạn gán
   tệ nhất **so với gold** là `train_13.jpg` - 2/3 người trong ảnh nằm trong nhóm OKS thấp nhất
   toàn bộ 29 người (0.795 và 0.804, so với trung bình 0.949); ảnh kế tiếp đã cách xa hẳn
   (`train_15.jpg` ở 0.899). Trong khi đó ở câu 4, ảnh OKS thấp nhất **giữa nhãn của bạn và model**
   lại là `train_08.jpg` - một ảnh mà chính gold cũng xác nhận nhãn của bạn đúng (đã hết lỗi sau
   rework).

   Hai ảnh khác nhau cho thấy hai loại khó khăn khác nhau: `train_13.jpg` khó với **con người**
   (mờ, xa, 3 người chồng lấn, một người từng bị bỏ sót) - khó vì bản thân dữ liệu ảnh xấu, gold
   cũng phải ước lượng chứ không chắc tuyệt đối đúng. Còn `train_08.jpg` là ảnh **dễ** với người
   gán (quay mặt thẳng, không che, không vặn mình) nhưng lại là nơi **model** lệch nhiều nhất so
   với nhãn đúng của bạn - tức là lỗi ở đây nằm ở khả năng của model (có thể model có thiên lệch
   khi đoán trái/phải với tư thế ngồi xe máy cụ thể này), không phải ở độ khó của ảnh. Nói cách
   khác: ảnh khó với người không nhất thiết là ảnh khó với model, và ngược lại - hai bên mắc lỗi
   vì hai nguyên nhân khác nhau.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Ảnh `train_13.jpg`, người #1 (người đứng gần, mặc vest), khớp `left_knee/right_knee/left_ankle/
right_ankle`. Hông của người này vẫn thấy rõ (`v=2`) nhưng phần chân bị mờ do khoảng cách và độ
sâu trường ảnh của bức hình gốc, không đủ chi tiết để xác định vị trí khớp thật. Tôi chọn `v=0`
(không đặt chấm) thay vì `v=1` vì quy tắc `v=1` yêu cầu "đặt chấm ở vị trí ước lượng" - mà ảnh mờ
đến mức không có cơ sở hình học nào để ước lượng, đặt chấm đoán mò sẽ đưa toạ độ nhiễu vào dữ liệu
train còn hại hơn là bỏ trống. Rủi ro: script `check_pose_labels.py` coi đây là dấu hiệu nghi vấn
(người nằm giữa ảnh mà nhiều khớp `v=0`), nên đây là ranh giới cần nhóm thống nhất thêm giữa "mờ
không đoán được" và "lạm dụng Outside" (đã ghi Ca 3 trong `GUIDELINE_MINI.md`).
