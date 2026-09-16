# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hoàng Gia Linh  Ngày: 2026-09-16

> Cách dùng: copy file này thành 
eports/REPORT.md. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | v=2: 328 | v=1: 44 | v=0: 104 |
| Thời gian trung bình mỗi ảnh | ~4 phút / ảnh |

Ba khớp có %v=1 cao nhất (từ reports/visibility_report.md):

1. left_ear: 25% (7/28 skeleton)
2. right_ear: 21% (6/28 skeleton)
3. left_hip: 21% (6/28 skeleton)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Tai và hông là hai khớp thường xuyên bị che khuất bởi tóc, mũ bảo hiểm, áo khoác dài hoặc trang phục rộng. Tai dễ bị nhầm lẫn giữa che khuất một phần (v= 1) và che khuất hoàn toàn dưới mũ bảo hiểm (v= 0). Hông không nhìn thấy trực tiếp bề mặt mà phải ước lượng qua nếp gấp quần áo và trung điểm eo, phải dự đoán, và nhiều trường hợp cũng bị che khuất ở một cài tư thế như lái xe, ảnh chụp từ 1 bên.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.886 | 0.925 |
| OKS@0.50 | 0.966 | 0.985 |
| OKS@0.75 | 0.931 | 0.960 |
| Lỗi dao_trai_phai | 0 | 0 |
| Lỗi 
ham_nguoi | 0 | 0 |
| Lỗi xoa_khop_bi_che | 15 | 2 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- 	Train_20.jpg người #1: Sửa khớp left_elbow và left_hip từ v=0 thành v=1 (đặt chấm ước lượng tại vị trí khuất sau tay lái và áo khoác).
- 	Train_10.jpg người #1: Sửa khớp left_hip và right_hip từ v=0 thành v=1 (ước lượng vị trí mấu chuyển lớn xương đùi).
- 	Train_04.jpg người #1: Sửa khớp left_wrist từ v=0 thành v=1 (ước lượng vị trí cổ tay trên cẳng tay hướng về tay cầm).
- 	Train_13.jpg người #2: Bổ sung các mốc khuôn mặt nose, left_eye, right_eye và chuyển left_shoulder, left_knee từ v=0 sang v=1.
- 	Train_15.jpg người #1: Kiểm tra lại mốc left_hip / right_hip bị cảnh báo chéo với mắt, căn chỉnh lại chuẩn xác theo trục cơ thể người. => Không sửa vì ảnh đó là người đứng nghiêng chứ không phải bị bắt chéo

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải (dao_trai_phai) nào trong toàn bộ 20 ảnh (kết quả chấm với gold đạt 0 lỗi đảo trái/phải).

## 3. Kiểm chéo

Bạn cùng nhóm: Lab làm cá nhân theo hướng dẫn, không kiểm chéo

Khớp lệch %v=1 nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
|  |  |  |  |  |

Luật mới đã bổ sung vào GUIDELINE_MINI.md sau khi thống nhất:


## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. pose_mAP50-95 tăng nhẹ **+0.0055** (từ 0.6853 lên 0.6908) và pose_precision tăng **+0.0058** (từ 0.9734 lên 0.9792). Việc fine-tune 80 epochs trên 20 ảnh với quy tắc visibility (v = 1) và 20 ảnh hơi giống tập test nên model khớp tốt hơn một chút. Mức tăng nhỏ, chưa đủ kết luận model “giỏi hơn hẳn”; chỉ cho thấy nhãn của tôi không phá hỏng hoàn toàn kiến thức gốc.

2. box_mAP50-95 (0.8041) thấp hơn pose_mAP50 (0.8450). Model tìm **khớp (pose)** dễ hơn và chính xác hơn tìm **bounding box người**. Lý do là cấu trúc bộ xương 17 điểm có tính ràng buộc hình học không gian cao, giúp model suy đoán mốc khớp chính xác ngay cả khi thân người bị che khuất một phần khiến việc dự đoán bounding box toàn thân bị chênh lệch.

3. Ở ảnh test Test_02.jpg và Test_09.jpg, model dự đoán sai theo kiểu **lệch nhẹ (slight displacement)** ở khớp cổ chân do bị khuất sau xe/vật cản, và mắc lỗi **nhầm người (person mismatch)** khi 2 người đứng sát chồng lên nhau khiến đường nối skeleton cẳng tay kéo sang người bên cạnh.

4. Ở mục 6 (Colab notebook), ảnh có OKS thấp nhất giữa nhãn tự gán và model dự đoán là 	Train_14.jpg (OKS = 0.724) và Train_15.jpg (OKS = 0.761). Nhãn tự gán ĐÚNG vì nhãn tuân thủ đặt chấm v = 1 cho khớp bị che khuất theo mốc giải phẫu, trong khi model dự đoán tự động conf < 0.5 nên bỏ sót hoặc đánh tụt các mốc khớp này.

5. Ảnh gán OKS thấp nhất so với gold (Train_13.jpg người #1/2 và Train_10.jpg) CŨNG LÀ những ảnh model bị lệch số người dự đoán (Train_13: model 3 / bạn 2, Train_10: model 2 / bạn 1). Điều này cho thấy đây là những bức ảnh có độ khó cao (người ở xa bị mờ, đông người chồng lấp, vật cản lớn), gây thách thức cho cả con người lẫn model AI trong việc xác định ranh giới đối tượng.

## 5. Một rule evidence bạn đã dùng

Trong ảnh train_18.jpg - khớp đầu gối phải (right_knee) và khớp cổ chân phải (right_ankle):
Cậu bé đang đạp xe, đầu gối và cổ chân phải bị thân xe đạp che khuất. Phần này nằm hoàn toàn trong khung hình, nên khớp không hề bị cắt ra ngoài mép ảnh. Do đó, áp dụng rule evidence, tôi không sử dụng v = 0. Tôi xác định hướng kéo dài của cẳng chân, đặt chấm ước lượng tại mốc cổ chân bị che khuất và chọn cờ v = 1 (Occluded).
