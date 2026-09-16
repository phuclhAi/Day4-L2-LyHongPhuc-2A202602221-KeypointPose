# Mini guideline - nhóm: <2A202602221> - <2A202602184>  |  người gán: Lý Hồng Phúc - 2A202602221  |  ngày: 16-09-2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | `v = 1`, đặt chấm ở vị trí ước lượng giải phẫu (giao điểm xương chậu) | Hông gần như không bao giờ thấy được qua quần áo, nhưng vẫn còn trong khung -> không phải `v = 0` |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | `v = 1`, ước lượng theo hình dạng đầu, **không** dùng `v = 0` | Bị vật/tóc che là "occluded" chứ không phải "ra khỏi khung" - hai khái niệm khác nhau |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp chi dưới (gối, mắt cá) không nằm trong ảnh -> `v = 0`, không đặt chấm. Hông vẫn `v = 1`/`v = 2` nếu thân trên còn hiển thị | Đúng định nghĩa: chỉ khớp thật sự ra ngoài mép ảnh mới là `v = 0` |
| Cổ tay nằm sau tay lái / sau thân mình | `v = 1`, đặt chấm ước lượng theo hướng cánh tay | Vẫn còn trong khung, chỉ bị vật/cơ thể khác che |
| Hai người chồng lên nhau | Mỗi người vẫn đủ 17 điểm riêng; khớp bị người kia che -> `v = 1` ở đúng vị trí ước lượng của người đó, không gán nhầm sang khung người kia | Tránh lỗi "nhầm người" nêu ở chặng 3 |
| Người nhỏ đến mức nào thì không gán nữa | Nếu ở zoom 100% không phân biệt được từng khớp bằng mắt thường (ước lượng bbox cao dưới ~20-25px) thì bỏ qua, ghi chú lại | Đặt chấm đoán mò còn hại hơn là bỏ qua có ghi chú |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

**Hông của người mặc quần áo dài** - hai người mặc jean, không thấy được khớp hông qua vải:

![hong bi quan che](outputs/vis_train/train_03.jpg)

**Tai bị mũ bảo hiểm che hoàn toàn** - cả hai người đội mũ fullface, tai hoàn toàn không thấy:

![tai bi mu che](outputs/vis_train/train_04.jpg)

**Người bị cắt ở mép ảnh + cổ tay sau tay lái** - chỉ thấy từ hông trở lên, hai tay đang nắm
tay lái xe máy nên cổ tay bị chính thân xe che:

![cat mep anh va co tay sau tay lai](outputs/vis_train/train_10.jpg)

**Hai người chồng lên nhau** - hai người đứng sát nhau, bounding box giao nhau lớn (IoU ~0.44):

![hai nguoi chong len nhau](outputs/vis_train/train_03.jpg)

**Người nhỏ đến mức không gán nữa** - người mặc áo xanh phía xa bên trái không có skeleton nào
được vẽ vì quá nhỏ/mờ để gán tin cậy:

![nguoi nho khong gan](outputs/vis_train/train_13.jpg)

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `2`, người thứ `1`, khớp `left/right_shoulder, left/right_hip, left/right_knee, left/right_ankle`

- Mơ hồ ở chỗ nào: người đạp xe đang **vặn mình** - thân quay gần hết về phía máy ảnh (kiểu
  quay lưng) nhưng đầu ngoái lại nhìn theo hướng ngược lại - nên mặt và thân "lệch chiều nhau"
  trên ảnh, dễ tưởng là đảo trái/phải.
- Bạn quyết thế nào: gán mặt (mắt/tai) theo chiều đầu đang ngoái, gán thân (vai/hông/gối/cổ
  chân) theo chiều thân đang xoay - hai chiều khác nhau nhưng đều đúng vì đây là tư thế vặn mình.
- Vì sao: trái/phải luôn tính theo giải phẫu của từng bộ phận tại thời điểm chụp, không phải
  theo một "hướng nhìn chung" của cả người - khi vặn mình, đầu và thân được phép lệch hướng.
  GUIDE cũng nói đúng ca này: "gần như không bao giờ tự cắt chéo ở thân - trừ khi vặn mình".
- Nếu người khác quyết ngược lại thì model học sai cái gì: augmentation lật ảnh sẽ nhân đôi lỗi
  đảo trái/phải - model học nhầm cấu trúc cơ thể (vai trái nối với hông phải...) một cách hệ thống.

### Ca 2 - ảnh `6`, người thứ `1`, khớp `toàn bộ khớp trái/phải`

- Mơ hồ ở chỗ nào: cũng là người quay lưng về camera, giống Ca 1.
- Bạn quyết thế nào: gán trái/phải theo cơ thể người (đứng vào vị trí họ, giơ tay trái lên) một
  cách nhất quán cho cả mặt lẫn thân - kiểm lại toạ độ thì mắt, vai, hông, gối, cổ chân đều theo
  đúng một chiều, không mâu thuẫn như Ca 1.
- Vì sao: quy tắc "trái/phải theo cơ thể người, không theo ảnh" ở mục 1 áp dụng như nhau cho mọi
  khớp trên cùng một người - không có lý do để mặt và thân theo hai chiều khác nhau.
- Nếu người khác quyết ngược lại thì model học sai cái gì: giống Ca 1 - lỗi đảo trái/phải bị
  augmentation lật ảnh nhân đôi.

### Ca 3 - ảnh `train_13`, người thứ `1`, khớp `left_knee, right_knee, left_ankle, right_ankle`

- Mơ hồ ở chỗ nào: người này ở xa, hình mờ - hông vẫn thấy rõ (`v=2`) nhưng chân mờ đến mức
  không chắc đặt chấm đúng khớp thật hay chỉ đang đoán mò.
- Bạn quyết thế nào: gắn cả 4 khớp là `v=0` (không đặt chấm) thay vì `v=1` ước lượng.
- Vì sao: `v=1` yêu cầu "vẫn đặt chấm ở vị trí ước lượng" - nhưng ảnh mờ đến mức không có cơ sở
  để ước lượng vị trí, đặt bừa còn hại hơn không đặt. `check_pose_labels.py` cảnh báo ca này vì
  không phân biệt được "mờ không đoán được" với "lạm dụng Outside" - đây là ranh giới cần nhóm
  thống nhất thêm (xem mục 2, dòng "người nhỏ đến mức nào thì không gán nữa").
- Nếu người khác quyết ngược lại thì model học sai cái gì: nếu dùng `v=1` với toạ độ đoán bừa,
  model bị dạy sai vị trí khớp bằng dữ liệu nhiễu; nếu tất cả nhóm đều chọn `v=0` cho ca mờ thì
  model chỉ mất một ít recall trên người xa, ít hại hơn.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `50%` / họ `18%`, lệch 32 điểm %). Theo sau là
  `right_ear` (36% / 11%), `left_wrist` (36% / 14%), và cả ba khớp mặt `nose`/`left_eye`/`right_eye`
  (21-25% / 0-4%) - xem chi tiết `reports/visibility_compare.md`.
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: guideline chưa rõ cho
  vùng đầu/mặt - lệch tập trung đúng vào nose/eyes/ears (5 trên 6 khớp lệch nhiều nhất), gợi ý
  hai bên đang xử lý khác nhau khi mặt quay nghiêng/bị tóc che, chứ không phải lỗi rải rác ngẫu nhiên.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: tôi không còn luật nào mới bổ sung vào mục 2
