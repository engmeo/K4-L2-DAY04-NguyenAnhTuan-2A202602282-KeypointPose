# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Anh Tuấn   Nhóm: SOLO   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20|
| Số skeleton |30 |
| v=2 / v=1 / v=0 |328 / 152 / 30 |
| Thời gian trung bình mỗi ảnh |10 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1.left_ear — 57%
2.right_ear — 50%
3.right_hip — 47%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. --> em thấy các khớp này cũng tương đối khó gán, nhưng %v=1 chủ yếu phản ánh việc khớp hay bị che, không hoàn toàn phản ánh độ khó xác định vị trí giải phẫu. Hai tai thường bị đầu, tóc hoặc người khác che; trong khi hông phải có thể bị thân người hoặc tư thế che khuất, khiến vị trí khớp khó quan sát trực tiếp. Vì vậy cần phân biệt trường hợp bị che nhưng vẫn suy ra được vị trí với trường hợp khó xác định chính xác vị trí giải phẫu.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | |0.9339 |
| OKS@0.50 | |0.9667 |
| OKS@0.75 | |0.9333 |
| Lỗi `dao_trai_phai` | |0 |
| Lỗi `nham_nguoi` | |3 |
| Lỗi `xoa_khop_bi_che` | |0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào): Không áp dụng. em thực hiện annotation solo và không có vòng kiểm chéo/rework, nên không có hai lần chạy trước và sau để ghi nhận các thay đổi cụ thể.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” --> Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 |0.8450 |0.8450 |0.0000 |
| pose_mAP50-95 | 0.6853|0.6908 |+0.0055 |
| pose_precision |0.9734 |0.9792 | +0.0058|
| pose_recall |0.8462 |0.8462 |0.0000 |
| box_mAP50-95 |0.8119 |0.8041 | −0.0078|

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

pose_mAP50-95 tăng từ 0.6853 lên 0.6908, tức +0.0055. Vì chỉ số tăng nên điều kiện “nếu nó giảm” không xảy ra. Đồng thời, kết quả Chặng 6 không đủ bằng chứng để kết luận cụ thể 20 ảnh đã dạy model điều gì hoặc làm hỏng điều gì.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

Sau fine-tune, box_mAP50-95 = 0.8041, trong khi pose_mAP50-95 = 0.6908, chênh 0.1133. Điều này cho thấy trên tập test này, việc xác định vùng người (box) đạt điểm cao hơn việc xác định chính xác các keypoint (pose). Lý do cụ thể cần kiểm tra trực quan các ảnh dự đoán; riêng các chỉ số này chưa đủ để xác định nguyên nhân.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

train_10 và train_03 có lỗi về số lượng người: ở train_10, model phát hiện 2 người trong khi nhãn có 1 người; ở train_03, model phát hiện 4 người trong khi nhãn có 3 người. Đây là trường hợp nhầm người / phát hiện thêm người, cần xem trực quan ảnh để xác định chính xác lỗi keypoint thuộc loại nào trong bốn loại của slide 43.
Ngoài ra, xét theo OKS model so với nhãn, train_13 có mức bất đồng thấp nhất với OKS = 0.58, tiếp theo là train_06 với 0.582. Các ảnh này cần được xem lại bằng tools/visualize_pose.py để xác định lỗi cụ thể.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh có OKS thấp nhất giữa model và nhãn của tôi là train_13, OKS = 0.58. Tuy nhiên, OKS thấp chỉ cho thấy model và nhãn bất đồng nhiều, không tự chứng minh bên nào đúng. Vì vậy cần xem trực quan train_13 bằng tools/visualize_pose.py và đối chiếu với gold nếu có. Từ bảng Chặng 6 hiện tại chưa đủ bằng chứng để kết luận model hay annotation đúng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

Không trùng nhau: ảnh có OKS thấp nhất giữa model và nhãn của tôi là train_13 với 0.58. Trong khi đó, kết quả gold trước đó cho thấy OKS thấp nhất của nhãn tôi so với gold là train_03 với 0.7227.

Do đó, hai ảnh tệ nhất theo hai phép so sánh không phải cùng một ảnh. Điều này cho thấy sự bất đồng giữa model và annotation không nhất thiết đồng nghĩa với annotation sai; có thể model gặp khó khăn ở train_13, trong khi annotation gặp vấn đề khác ở train_03. Cần kiểm tra trực quan từng ảnh để xác định nguyên nhân.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
python3 tools/evaluate_pose_annotations.py --pred dataset/labels/train --gold gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json

train_XX + người thứ X + keypoint left/right_xxx. em chọn v=1 vì quan sát thấy phần cơ thể liền kề và vị trí khớp vẫn có thể xác định được dù bị che một phần bởi [vật che/trang phục/người khác]. Khớp vẫn nằm trong phạm vi ảnh và không bị khuất hoàn toàn. Vì vậy em giữ keypoint là visible/occluded (v=1), thay vì v=0.
