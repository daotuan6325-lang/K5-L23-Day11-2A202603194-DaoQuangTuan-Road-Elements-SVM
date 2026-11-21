# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): thannt        Người kiểm (học viên): Đào Quang Tuấn (daotuan, 2A202603194)  
Ngày: 21/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `thannt-front.zip`, `thannt-fisheye.zip`, `thannt-manifest.json` — revision 1 (sha256 của hai zip khớp manifest; `contains_images: false`)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 105 (81 shape + 24 tag)  

Mẫu kiểm: A01, A04, A06, A08 (camera trước) và B03 (fisheye_rear), B06 (fisheye_left) — chọn để phủ đủ làn, vạch đôi, vùng lái, đèn/biển dày đặc và hai loại camera fisheye.

Cách kiểm: chạy `road_check lint` trên hai zip; đối chiếu từng nhãn của 6 ảnh với ảnh gốc (phóng to ×2–×5 có lưới toạ độ). Chỉ ghi lỗi khi đã nhìn thấy trên ảnh; chỗ còn phân vân đưa vào "Ca mơ hồ", không tính lỗi.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 10 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 22 | 1 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 22 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 22 | 1 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 22 | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 18 | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 20 | 1 | nặng |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 41 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 21 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 14 | 1 | chính |
| QC1-12 file | lỗi file | 3 | 0 | nhẹ / nặng |

Cách đếm số mục kiểm: QC1-01 = 1 tag camera × 6 ảnh; QC1-02 = 2 vạch (L1, R1) × 4 ảnh front + 2 cho B03 (fisheye_rear; B06 là fisheye_left nên không xét L1/R1); QC1-03 đến QC1-06 = 22 polyline lane; QC1-07 = 12 drivable + 4 crosswalk + 2 road_curb; QC1-08 = 20 đèn; QC1-09 = 20 đèn + 21 biển; QC1-10 = 21 biển; QC1-11 = 14 shape trên B03, B06; QC1-12 = 2 zip + manifest.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-03 | A08 | lane #5, điểm đầu (0,321) | nặng | Bên trái có hai vạch trắng liền khác nhau, hội tụ về (460–490, 275–280); cả hai đều gán `position = L2`. Vạch ngoài (lane #5) phải là `other` — hai polyline cùng L2 cho hai vạch khác nhau làm sai thứ tự làn. |
| 2 | QC1-08 | A06 | traffic_light #5, box (945,342)–(965,365) | nặng | Đèn đi bộ `facing = front` đang sáng hình người đi màu trắng (tín hiệu "đi"), nhưng gán `color = off`. Đúng là `green`. Sai màu trên đèn quay về xe là mức nặng theo thang mức lỗi. |
| 3 | QC1-07 | A06 | crosswalk (bỏ sót) | chính | Vạch ngựa vằn cắt ngang đường ở y ≈ 425–465, từ mép trái tới x ≈ 900, nhưng ảnh không có polygon `crosswalk` nào. |
| 4 | QC1-05 | A01 | lane #3 (0,603) + lane #9 (0,599) | chính | L2 là một vạch trắng liền đơn, nhưng vẽ thành hai polyline gần như trùng nhau (cách nhau ≈ 4 px suốt chiều dài). Một vạch phải là một polyline. |
| 5 | QC1-06 | A01 | lane #3, lane #9 | chính | Cùng vạch L2 ở trên: `kind = double`, đúng là `single` (chỉ có một vệt sơn; hai vệt của vạch đôi ở gần xe phải cách nhau rõ, không phải 4 px). |
| 6 | QC1-11 | B06 | lane #3, (163,258) → (277,253) | chính | fisheye_left: vạch đứt phía xa chỉ vẽ một đoạn ngắn ≈ 115 px rồi dừng, trong khi các vệt đứt còn thấy tiếp theo cung cong lên phía phải ảnh. Polyline dừng sớm, không theo hết cung. |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| B03 | Camera sau (fisheye_rear): vạch đứt sát xe nằm bên phải ảnh nhưng là bên trái của xe theo chiều chạy. Bạn gán R1/R2 (theo phía trên ảnh); cách gán khác là L1/L2 (theo phía của xe). Guideline chưa nói rõ quy ước trái/phải cho camera sau. | Chưa có |
| A08 | lane #3 (L1) gán `style = solid`; trên ảnh vạch rất mờ, có vài vệt ngắt quãng gợi ý `dashed`. Không đủ chắc để tính lỗi. | Chưa có |
| A04, A06 | Tag weather: A04 gán `clear` nhưng trời trắng xám, không có bóng nắng (nghiêng về `overcast`); A06 gán `overcast` dưới gầm đường sắt trên cao, phần trời thấy được khá sáng. Tag weather không có mã QC1 riêng nên không tính lỗi. | Chưa có |
| A04, A06 | Vạch đôi vẽ 2 polyline, `kind = double` (lint báo QC1-05). Theo hướng dẫn của giảng viên trong buổi học nên không tính lỗi. | Giảng viên: chấp nhận 2 polyline |

## Quyết định
Lỗi nặng: 2   Lỗi chính: 4 / 105 nhãn = 3,8 %   Lỗi nhẹ: 0   Ca chờ lead: 3  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): Lô còn 2 lỗi nặng (QC1-03 ở A08, QC1-08 ở A06). Theo luật, chỉ cần một lỗi nặng là Rework, không xét tỉ lệ lỗi chính (3,8 % < 5 %, nếu đứng riêng thì không làm Rework). Ba ca mơ hồ không làm thay đổi kết luận nên không chọn Hold.

## Ghi chú cho người gán (sửa trước khi nộp lại)
1. A08: đổi lane #5 sang `position = other`; giữ lane #4 là L2.
2. A06: traffic_light #5 đổi `color = green`; thêm polygon `crosswalk` cho vạch ngựa vằn ở y ≈ 425–465.
3. A01: xoá một trong hai polyline L2 (lane #3 hoặc #9), polyline còn lại đặt `kind = single`.
4. B06: kéo dài lane #3 theo các vệt đứt tới điểm xa nhất còn thấy; đặt điểm dày ở đoạn cong.
5. Kiểm lại các ảnh còn lại theo cùng mẫu lỗi: lint báo vạch tách đôi ở A08, A09 và L1/R1 thiếu ở A05, A09, A14 — xin coach xác nhận miễn QC1-02 trước khi nộp lại.
6. Khi nộp lại: cất bản cũ vào `r1/`, export lại cùng tên zip, tạo manifest `--revision 2`.
