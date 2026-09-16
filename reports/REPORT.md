# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Tuấn Anh   Nhóm: Cá nhân   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 344 / 102 / 30 |
| Thời gian trung bình mỗi ảnh | Khoảng 4 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (57%)
2. right_ear (43%)
3. left_hip (32%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.
Đúng, vì đây là những khớp rất hay bị che khuất. Tai thường bị tóc, mũ bảo hiểm hoặc góc xoay của đầu che đi. Hông thường bị che bởi quần áo (áo dài trùm xuống) hoặc bị cánh tay vắt ngang qua che khuất nên luôn phải phán đoán vị trí (v=1).

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.869 | 0.886 |
| OKS@0.50 | 0.931 | 0.966 |
| OKS@0.75 | 0.931 | 0.931 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 3 | 1 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_06.jpg` - người thứ 1 - các khớp vai/hông: Đảo lại trái phải do lúc gán bị nhầm hướng cơ thể.
- `train_03.jpg` - người thứ 2 - `right_elbow`, `right_wrist`: Kéo điểm từ tay của người đứng cạnh về lại đúng cơ thể của người thứ 2.
- `train_04.jpg` - người thứ 1 - `left_wrist`: Cố gắng điều chỉnh lại vị trí tay để không bị nhầm người.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?
Xảy ra ở ảnh `train_06`. Đây là một ảnh có độ khó tương đối do người trong ảnh đứng xoay ngang hoặc góc máy chụp từ phía sau/bên cạnh, khiến việc định vị hướng tay (đâu là tay phải, tay trái của người đó) dễ bị nhầm thành định hướng của người nhìn vào màn hình.

## 3. Kiểm chéo

Bạn cùng nhóm: Làm cá nhân

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| N/A | N/A | N/A | N/A | Làm cá nhân nên không đối chiếu |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- (Không có do không so sánh chéo)

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.845 | 0.845 | 0.0 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
Thay đổi rất nhẹ (+0.0055). Chỉ với 20 ảnh, mô hình học thêm được một số cách ước lượng occluded (khớp bị che) và không bị suy giảm khả năng tổng quát quá nhiều, tuy vậy độ chính xác mAP cho bounding box (box_mAP) có giảm nhẹ do overfitting trên tệp dữ liệu nhỏ.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
`box_mAP50` (0.96) cao hơn `pose_mAP50` (0.845). Chênh lệch 0.115. Model tìm hộp bao (người) dễ hơn tìm khớp, bởi vì người là một thực thể lớn, đặc trưng hiển thị rõ, trong khi khớp là một điểm nhỏ, thường xuyên biến dạng, bị che lấp bởi vật thể hoặc bộ phận cơ thể khác.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:
Trong tập test, mô hình đôi khi vướng phải lỗi **lệch nhẹ** ở cổ tay/cổ chân do các khớp này biến dạng đa dạng nhất.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
Thường là các ảnh có người bị cắt mép hoặc xếp chồng. Đa số con người (người gán nhãn) có khả năng hình dung (reasoning) bị che khuất tốt hơn, trong khi mô hình dễ bị trượt hẳn.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?
Có sự tương quan. Những bức ảnh nhiễu, chồng chéo người nhiều khiến người gán bối rối thì model (được fine-tune bằng chính dữ liệu đó) cũng sẽ gặp khó khăn để nội suy tính năng, dẫn đến dự đoán kém đi.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_10.jpg`, người thứ 1, phần khớp chân (gối, cổ chân).
- **Căn cứ thị giác**: Người này nằm hoàn toàn ở giữa khung hình, nhưng phần nửa thân dưới bị một chiếc xe che lấp.
- **Lý do chọn v=1**: Dù không nhìn thấy trực tiếp khớp gối, nhưng vị trí đôi chân chắc chắn vẫn đang nằm bên trong bức ảnh (không thể vượt ra ngoài mép ảnh). Do đó, dựa theo luật của lab, tôi chọn `v=1` (Occluded) thay vì `v=0` (Outside), sau đó ước lượng vị trí chấm điểm đằng sau chiếc xe để dạy model tư duy nội suy vật thể bị che khuất.
