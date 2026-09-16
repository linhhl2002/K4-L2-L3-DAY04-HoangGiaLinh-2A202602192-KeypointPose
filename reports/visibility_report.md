# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 13.29 khớp có v > 0 mỗi người
- Tổng: v=2 328 | v=1 44 | v=0 104

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 0 | 7 | 0% |
| 1 | left_eye | 19 | 1 | 8 | 4% |
| 2 | right_eye | 20 | 1 | 7 | 4% |
| 3 | left_ear | 12 | 7 | 9 | 25% |
| 4 | right_ear | 16 | 6 | 6 | 21% |
| 5 | left_shoulder | 24 | 3 | 1 | 11% |
| 6 | right_shoulder | 26 | 1 | 1 | 4% |
| 7 | left_elbow | 22 | 2 | 4 | 7% |
| 8 | right_elbow | 24 | 1 | 3 | 4% |
| 9 | left_wrist | 18 | 3 | 7 | 11% |
| 10 | right_wrist | 19 | 5 | 4 | 18% |
| 11 | left_hip | 19 | 6 | 3 | 21% |
| 12 | right_hip | 22 | 4 | 2 | 14% |
| 13 | left_knee | 15 | 2 | 11 | 7% |
| 14 | right_knee | 18 | 1 | 9 | 4% |
| 15 | left_ankle | 17 | 0 | 11 | 0% |
| 16 | right_ankle | 16 | 1 | 11 | 4% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
