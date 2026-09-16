0# Mini guideline - nhóm: ______  |  người gán: Nguyễn Anh Tuấn  |  ngày: ______

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
| Hông của người mặc quần áo dài |Nếu vẫn xác định được vị trí hông dựa trên cấu trúc cơ thể và phần thân liền kề thì đặt keypoint tại vị trí ước lượng và dùng v = 1 |Quần áo che bề mặt khớp nhưng không đồng nghĩa khớp nằm ngoài ảnh hoặc không thể ước lượng. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần |Nếu xác định được vị trí tai từ hình dạng đầu, tóc/mũ và vị trí tai tương đối thì vẫn đặt điểm và dùng v = 1. |Tai thường khó quan sát trực tiếp; visibility report cũng cho thấy left_ear và right_ear có tỷ lệ v=1 cao. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) |Các khớp nằm ngoài phần ảnh quan sát được dùng v = 0 và không đặt chấm. Các khớp vẫn nằm trong khung vẫn phải được gán. | v = 0 dùng cho khớp thực sự nằm ngoài mép ảnh, không phải chỉ vì khó nhìn thấy.|
| Cổ tay nằm sau tay lái / sau thân mình | Nếu cổ tay vẫn nằm trong khung nhưng bị vật hoặc cơ thể che thì đặt điểm tại vị trí ước lượng và dùng v = 1.|Phân biệt trường hợp bị che (v=1) với trường hợp ra ngoài ảnh (v=0). |
| Hai người chồng lên nhau | Mỗi người vẫn giữ một bộ đủ 17 keypoint; keypoint của người nào thì phải được đặt theo đúng cơ thể người đó, không kéo xương sang người còn lại.| Tránh lỗi nhầm người và xương nối sang cơ thể khác.|
| Người nhỏ đến mức nào thì không gán nữa |Không bỏ người chỉ vì người đó nhỏ. Nếu người xuất hiện trong ảnh thì vẫn gán đủ 17 điểm theo quy tắc chung; những điểm không quan sát được thì xử lý theo v=1 hoặc v=0. |Quy tắc chung yêu cầu mọi người trong ảnh đều có đủ 17 điểm. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh train_20, người thứ 1, khớp right_wrist

- Mơ hồ ở chỗ nào:Người lái xe máy có hai tay ở vùng tay lái; vị trí cổ tay bị các bộ phận của xe và tay che một phần nên khó xác định chính xác.
- Bạn quyết thế nào:Chọn v = 1 và đặt chấm tại vị trí ước lượng của cổ tay.
- Vì sao:Cổ tay vẫn nằm trong khung ảnh và có thể ước lượng từ quan hệ giữa cẳng tay, bàn tay và tay lái; đây là trường hợp bị che chứ không phải nằm ngoài ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Model có thể học rằng cổ tay trong các tư thế cầm tay lái là keypoint không tồn tại hoặc bị bỏ, thay vì học vị trí ước lượng khi bị che.

### Ca 2 - ảnh train_13, người thứ 2, khớp right_ankle

- Mơ hồ ở chỗ nào:Người thứ 2 bị cắt ở phần dưới ảnh; phần chân vẫn được suy ra từ các khớp phía trên nhưng vị trí cổ chân nằm ngoài vùng ảnh.
- Bạn quyết thế nào:Chọn v = 0 và không đặt chấm cho right_ankle.
- Vì sao:Theo quy tắc visibility, khớp thực sự nằm ngoài mép ảnh phải dùng v=0; không được đặt một vị trí ước lượng bên trong ảnh khi khớp đã ra ngoài khung.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Model có thể học vị trí cổ chân giả nằm trong ảnh đối với người bị crop, làm sai phân bố vị trí keypoint ở biên ảnh.

### Ca 3 - ảnh train_06, người thứ 1, khớp right_eye

- Mơ hồ ở chỗ nào:Người đi xe máy đội mũ bảo hiểm, vùng mắt bị che một phần bởi cấu trúc mũ/visor nên vị trí mắt không thể quan sát rõ như trường hợp bình thường.
- Bạn quyết thế nào:Chọn v = 1 và vẫn đặt keypoint tại vị trí ước lượng của mắt.
- Vì sao:Khớp vẫn nằm trong khung ảnh; phần đầu và vị trí tương đối của hai mắt vẫn cho phép ước lượng vị trí mắt dù bị che.
- Nếu người khác quyết ngược lại thì model học sai cái gì:Model có thể học rằng các keypoint mắt bị che phải bị bỏ khỏi annotation, tạo ra sự không nhất quán giữa các trường hợp bị che nhưng vẫn còn trong ảnh.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
