# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: NGUYỄN ĐỨC TÙNG   Nhóm: Không có   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 324 / 127 / 25 |
| Thời gian trung bình mỗi ảnh | ~5-10 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (54%)
2. right_ear (39%)
3. left_wrist (36%) — đồng hạng với left_hip, left_knee, right_ankle (đều 36%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng một phần. `left_ear`/`right_ear` bị che nhiều là hợp lý vì tóc, mũ bảo hiểm hoặc góc quay đầu thường xuyên che một bên tai — đây là "hay bị che" thật sự, dễ nhận biết bằng mắt. Ngược lại, các khớp cổ tay/hông/đầu gối/mắt cá bị Occluded nhiều hơn là do "khó xác định vị trí giải phẫu" khi hai người chồng lên nhau hoặc trang phục rộng che dáng cơ thể, chứ không hẳn là bị vật cản che trực tiếp bề mặt khớp.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.931 | 0.931 |
| OKS@0.50 | 0.966 | 0.966 |
| OKS@0.75 | 0.966 | 0.966 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 1 | 1 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- Không sửa gì. Không có "lần chạy thứ hai" — chỉ chạy một lần `evaluate_pose_annotations.py`, giữ nguyên nhãn ban đầu.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Làm cá nhân — không có bạn cùng nhóm để đối chiếu visibility report.

## 4. Model

*(Chưa điền — cần chạy xong Chặng 6 trên Colab để lấy số liệu từ `outputs/eval_model.json`)*

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` **không giảm mà tăng nhẹ** +0.0055 (từ 0.6853 lên 0.6908). Với chỉ 20 ảnh train, mức tăng này rất nhỏ nhưng cho thấy dữ liệu của tôi có bổ sung thêm vài biến thể tư thế/occlusion (ví dụ người bị che một phần, hai người chồng lên nhau) mà COCO gốc có thể chưa bao phủ đủ dày, giúp model tinh chỉnh nhẹ độ chính xác định vị khớp. Đổi lại, `box_mAP50-95` lại giảm nhẹ (-0.0078) — có thể do overfit nhẹ lên đặc điểm khung hình của 20 ảnh nhỏ, đánh đổi một chút khả năng định vị bounding box tổng quát để lấy độ chính xác keypoint tốt hơn.

2. Baseline: `box_mAP50-95` (0.8119) cao hơn `pose_mAP50-95` (0.6853) tới 0.1266. Sau fine-tune: chênh lệch còn 0.1133 (0.8041 - 0.6908). Ở cả hai giai đoạn, model **tìm người (box) dễ hơn tìm khớp (pose) rõ rệt**. Lý do: xác định bounding box chỉ cần bao trọn vùng chứa người, sai số cho phép lớn; còn định vị 17 keypoint đòi hỏi độ chính xác cao hơn nhiều, đặc biệt khó khi khớp bị che hoặc hai người đứng/ngồi sát nhau.

3. Ảnh `test_09.jpg` (hai người ngồi trên xe máy): model bị lỗi **"nhầm người"** — đường nối skeleton (đặc biệt vùng cổ tay - hông) của người ngồi trước (áo xám) bị vắt chéo lấn sang vùng thân của người ngồi sau (áo trắng), thay vì giữ đúng skeleton riêng biệt cho từng người. Đây là lỗi cùng loại với lỗi tôi từng gặp khi tự gán nhãn ở `train_03.jpg` (2 người khoác tay quá gần nhau).

4. *(Cần bổ sung — chưa có sẵn OKS chi tiết theo từng ảnh test so với nhãn `dataset/labels/test`. Cần chạy `tools/evaluate_pose_annotations.py --pred <dự đoán model> --gold dataset/labels/test --images dataset/images/test` để lấy con số cụ thể cho câu này.)*

5. **Không** — vì ảnh tôi gán nhãn tệ nhất (`train_03.jpg`, `train_13.jpg`) nằm trong **tập train** (20 ảnh tôi tự gán), còn ảnh model bị đánh giá là ở **tập test** (10 ảnh test riêng biệt, nhãn có sẵn, chưa từng dùng để train). Hai tập ảnh này hoàn toàn khác nhau, không trùng lặp — đây chính là mục đích của việc tách train/test, nên không thể có "ảnh chung" để so sánh trực tiếp giữa lỗi gán nhãn của tôi và lỗi dự đoán của model.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_06.jpg`, người thứ 1, khớp tai/mặt bên phải. Người này đội mũ bảo hiểm full-face che gần hết phần tai và mặt bên phải, đồng thời góc chụp chếch từ trái qua khiến bên phải cơ thể càng khó thấy hơn. Tuy bị che, hình dạng tổng thể của mũ bảo hiểm và vị trí đầu vẫn cho phép suy luận được tai vẫn nằm trong khung ảnh (không bị cắt ra ngoài mép ảnh) — vì vậy tôi chọn `v=1` (Occluded) thay vì `v=0` (Outside), vẫn đặt điểm ước lượng tại vị trí suy luận được.
