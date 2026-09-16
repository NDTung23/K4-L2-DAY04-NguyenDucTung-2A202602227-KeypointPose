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
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

*(Sẽ trả lời sau khi có kết quả model)*

1.
2.
3.
4.
5.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_06.jpg`, người thứ 1, khớp tai/mặt bên phải. Người này đội mũ bảo hiểm full-face che gần hết phần tai và mặt bên phải, đồng thời góc chụp chếch từ trái qua khiến bên phải cơ thể càng khó thấy hơn. Tuy bị che, hình dạng tổng thể của mũ bảo hiểm và vị trí đầu vẫn cho phép suy luận được tai vẫn nằm trong khung ảnh (không bị cắt ra ngoài mép ảnh) — vì vậy tôi chọn `v=1` (Occluded) thay vì `v=0` (Outside), vẫn đặt điểm ước lượng tại vị trí suy luận được.
