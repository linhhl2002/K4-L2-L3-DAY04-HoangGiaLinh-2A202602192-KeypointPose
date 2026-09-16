# Mini guideline - nhóm: ______  |  người gán: ______  |  ngày: ______

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file .SVG chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung ->  = 1, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh ->  = 0, **không** đặt chấm.
- Không dùng Hidden (h) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | **Đánh cờ  = 1 (Occluded) & Đặt chấm ước lượng** tại khớp mấu chuyển lớn xương đùi (ngay dưới eo/nếp gấp quần). | Hông là mốc giải phẫu cốt lõi của thân dưới. Dù bị áo/quần dài che khuất bề mặt, khớp vẫn thuộc khung hình nên phải đặt chấm ước lượng và bật cờ  = 1 để giữ đủ 17 điểm COCO, giúp model học đúng bộ xương tổng thể. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | **Đánh cờ  = 1 (Occluded) & Đặt chấm ước lượng** tại vị trí tâm tai giải phẫu (dựa vào trục mắt - hàm hoặc nếp mũ). | Tai nằm trên đầu vẫn nằm trong khung hình. Đánh cờ  = 1 giữ nguyên bộ 5 điểm khuôn mặt, tránh đứt đoạn phần đầu và tuân thủ nguyên tắc không xoá/bỏ điểm khi đối tượng còn trong khung ảnh. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | **Khớp trong khung:** Gán bình thường ( = 2 hoặc  = 1).<br>**Khớp chân bị cắt ra ngoài mép ảnh:** Đánh cờ  = 0 (Outside) và **không đặt chấm**. | Khớp ra ngoài mép ảnh ( = 0) tuyệt đối không đoán bừa hay đặt chấm ngoài khung. Đặt  = 0 giúp model học nhận diện mốc cắt mép ảnh (crop) để không bị phạt điểm hoặc đoán sai tư thế. |
| Cổ tay nằm sau tay lái / sau thân mình | **Đánh cờ  = 1 (Occluded) & Đặt chấm ước lượng** tại vị trí cổ tay ẩn (nối tiếp cẳng tay hướng về tay cầm/lưng). | Cổ tay bị vật thể (tay lái, áo, lưng) che khuất nhưng bộ tay vẫn ở trong khung hình. Đặt chấm  = 1 giúp duy trì liên kết xương cẳng tay - bàn tay, tránh đứt gãy skeleton cánh tay. |
| Hai người chồng lên nhau | **Gán lần lượt từng người (Skeleton riêng).** Các khớp của người bị che lấp nếu còn trong khung thì vẫn đặt chấm ước lượng và chọn  = 1 (Occluded). | Phân tách rõ skeleton từng người giúp tránh lỗi nghiêm trọng 
ham_nguoi (chéo xương sang người bên cạnh). Sử dụng  = 1 giúp model nhận diện đúng các trường hợp va chạm/che lấp trong đám đông. |
| Người nhỏ đến mức nào thì không gán nữa | **Gán tất cả người phân biệt được cấu trúc cơ thể (đầu, thân, chân/tay).** Chỉ bỏ qua nếu quá nhỏ/mờ (< 10-15px) thành đốm nền không rõ dạng. | 20 ảnh thực hành được thiết kế để mọi người đều đủ lớn để gán. Gán 100% người nhận diện được để đạt độ bao phủ (Coverage) so với Gold label, tránh bị trừ điểm do thiếu đối tượng. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

### Minh hoạ ảnh mẫu CVAT cho các luật (Cần chụp & chèn ảnh từ CVAT của nhóm):

1. **Hông bị che bởi quần áo dài:**
   ![Ảnh mẫu Hông](assets/guide/cvat/mini_rule_hip.jpg)
   *Ghi chú: Đặt chấm tại vị trí mấu chuyển lớn xương đùi ước lượng, bật cờ Occluded (v=1).*

2. **Tai bị tóc / mũ che một phần:**
   ![Ảnh mẫu Tai](assets/guide/cvat/mini_rule_ear.jpg)
   *Ghi chú: Đặt chấm tại tâm tai giải phẫu ước lượng, bật cờ Occluded (v=1).*

3. **Người bị cắt ở mép ảnh:**
   ![Ảnh mẫu Cắt mép](assets/guide/cvat/mini_rule_crop.jpg)
   *Ghi chú: Khớp chân nằm ngoài mép ảnh bật cờ Outside (v=0), không đặt chấm.*

4. **Cổ tay nằm sau tay lái / thân mình:**
   ![Ảnh mẫu Cổ tay](assets/guide/cvat/mini_rule_wrist.jpg)
   *Ghi chú: Ước lượng vị trí cổ tay khuất sau tay lái/thân người, bật cờ Occluded (v=1).*

5. **Hai người chồng lên nhau:**
   ![Ảnh mẫu Chồng người](assets/guide/cvat/mini_rule_overlap.jpg)
   *Ghi chú: Gán skeleton riêng cho từng người, khớp bị che lấp đánh cờ v=1.*

6. **Giới hạn kích thước người nhỏ:**
   ![Ảnh mẫu Người nhỏ](assets/guide/cvat/mini_rule_small_person.jpg)
   *Ghi chú: Gán đủ skeleton cho người nhỏ nếu vẫn nhận diện rõ cấu trúc đầu - thân - chân.*

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh ______, người thứ ___, khớp ______

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

### Ca 2 - ảnh ______, người thứ ___, khớp ______

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

### Ca 3 - ảnh ______, người thứ ___, khớp ______

- Mơ hồ ở chỗ nào:
- Bạn quyết thế nào:
- Vì sao:
- Nếu người khác quyết ngược lại thì model học sai cái gì:

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch %v=1 nhiều nhất: ______ (bạn ___% / họ ___%)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
