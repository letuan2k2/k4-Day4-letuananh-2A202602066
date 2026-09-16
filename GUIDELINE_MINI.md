# Mini guideline - nhóm: Cá nhân  |  người gán: Lê Tuấn Anh  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Chọn v=1 (Occluded), đặt chấm | Vì bị áo che nhưng vẫn có thể ước lượng được vị trí hông qua giải phẫu cơ thể. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Chọn v=1 (Occluded), đặt chấm | Bị che khuất một phần nhưng chưa lọt ra ngoài khung hình. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Chọn v=0 (Outside), không đặt chấm | Khớp gối và cổ chân đã lọt hẳn ra ngoài ảnh, không được cố kéo sát mép. |
| Cổ tay nằm sau tay lái / sau thân mình | Chọn v=1 (Occluded), đặt chấm | Còn nằm trong phạm vi ảnh, cần bắt model học cách ước lượng (reasoning). |
| Hai người chồng lên nhau | Chọn v=1 cho khớp bị che | Áp dụng luật bị che khuất nhưng còn trong khung ảnh. |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả nếu phân biệt được | Giúp mô hình bắt được mọi bối cảnh, trừ những người quá nhỏ không thể đoán vị trí các khớp. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01`, người thứ `2`, khớp `left_knee / right_knee`

- Mơ hồ ở chỗ nào: Đầu gối và cổ chân của người đàn ông và phụ nữ bị cắt ở mép dưới của khung ảnh.
- Bạn quyết thế nào: Chọn v=0 (Outside), không kéo chấm sát xuống mép ảnh.
- Vì sao: Theo luật, khớp đã lọt ra ngoài mép thì không được cố gán tọa độ ở rìa ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Mô hình sẽ học sai rằng bất cứ chân nào bị cắt thì toạ độ gối luôn nằm ở mép y=1.0, làm lệch dự đoán pose.

### Ca 2 - ảnh `train_10`, người thứ `1`, khớp `chân bị che`

- Mơ hồ ở chỗ nào: Người nằm lọt thỏm ở giữa khung hình, nhưng phần chân bị phương tiện che khuất hoàn toàn.
- Bạn quyết thế nào: Chọn v=1 (Occluded) và vẫn đặt chấm ước lượng, tắt v=0 (Outside).
- Vì sao: Do khớp vẫn nằm bên trong khung ảnh, không bị cắt bởi rìa ảnh nên bắt buộc phải chọn Occluded thay vì Outside.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model sẽ không học được khả năng "nhìn xuyên thấu" hoặc ước lượng vị trí tay chân qua ngoại cảnh (occlusion reasoning) và bị đánh lỗi OKS.

### Ca 3 - ảnh `train_16`, người thứ `1`, khớp `shoulder / hip`

- Mơ hồ ở chỗ nào: Không rõ Vai trái / Vai phải do góc nhìn hoặc tư thế của người trong ảnh quay đi.
- Bạn quyết thế nào: Xác định Trái/Phải theo **cơ thể của người đó** (tức là tay trái, tay phải của chính người trong ảnh).
- Vì sao: Đảm bảo tính nhất quán về sinh học, không bị đảo lộn theo góc nhìn của camera.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Đường nối Vai và Hông bị vắt chéo thành hình chữ X, mô hình sẽ học sai hoàn toàn logic giải phẫu cơ thể.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `N/A` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Do làm cá nhân nên không so chéo.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: N/A
