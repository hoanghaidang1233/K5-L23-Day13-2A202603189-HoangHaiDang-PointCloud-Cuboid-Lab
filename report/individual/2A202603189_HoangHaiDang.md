# Báo cáo Day 13 — Hoàng Hải Đăng, 2A202603189

## 1. Kết luận trước
- Mã số: 2A202603189 · Task: `D13-2A202603189`
- 10 frame: s01f18, s01f53, s02f40, s02f55, s03f08, s03f51, s04f10, s04f26, s05f16, s05f31
- Windows 11 · trình duyệt: Opera GX · CVAT 2.83.0 · bạn ngồi cạnh (MSSV): không có

Tổng 67 cuboid trên 10 frame; ít nhất là s02f55 (4 cuboid) vì là đoạn đường cong nhiều cây, lúc đó chỉ có 4 xe trong ~23 m. `selfcheck` lần cuối cờ 6/67 (3 xe con cao ~1 m, 2 xe đáy chìm/lơ lửng, 1 người đi bộ có điểm sát mép); lần đầu cờ 66/67 vì yaw nằm ngang thân xe, đã sửa. Tin nhất: xe con gần (< 15 m) cùng chiều — đáy lệch < 0,1 m, yaw 270°, kích thước đúng bảng 4.3. Yếu nhất: chiều cao/đáy xe bị dải cây che và yaw của xe bên kia dải phân cách.

Cách dùng CVAT: kéo chuột trái để xoay, chuột phải để dời, con lăn để phóng to. Chọn label → **Shape** → nháy đúp lên cụm điểm; chỉnh tâm, cạnh, yaw ở **Top**, chỉnh đáy và chiều cao ở **Side/Front**. Bật **Appearance → Cuboid orientation** để vạch đỏ (+X) chỉ về đầu xe; kiểm trên 7 camera bằng `python scripts/lab.py preview`.

## 2. Đọc frame (Bước 2)
Ảnh: `t1-s01f18-3d`, `t1-s01f18-top`, `t1-s01f53-3d`, `t1-s01f53-top`.

| Frame | Phía trước; +x, +y | Mặt đường | Vật xa nhất | Loại vật | Phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f18 | Cạnh trên (−y, khớp cam01); +x sang trái, +y xuống dưới | Phẳng, thấp hơn LiDAR ~1,7 m | ~37 m, xe con trước-trái | Xe con, xe buýt, dải cây giữa đường | Sau: 3 xe con (11–25 m); trái: 2 xe sau dải cây | Dải điểm thấp dọc hai mép đường (bụi cây hay rào) |
| s01f53 | Cạnh trên; +x sang trái, +y xuống dưới | Phẳng, thấp hơn LiDAR ~1,75 m | ~16,5 m, xe đạp trước-phải | Xe con dừng chờ, xe đạp có người, người đi bộ | Sau: 2 xe con (6,5 m, 16 m); phải: 2 xe đạp, 2 người trên vỉa hè | Dãy xe đạp dựng không người (cam07), không vẽ |

Chỉ camera cho biết: màu và loại xe (xe buýt, taxi); vạch kẻ, mũi tên làn, biển báo; đầu/đuôi xe và người đang đạp hay dắt xe. Chỉ LiDAR cho biết: khoảng cách bằng mét (xe đạp 10,0 m, xe xa nhất 37,4 m); kích thước thật (xe buýt dài ~12,4 m) và mặt đường ở ~−1,7 m; vật bị che trên ảnh vẫn còn điểm (xe sau dải cây ở s01f18 có 187–456 điểm).

| Frame lướt | Phía trước | Điều lạ |
|---|---|---|
| s02f40 | Cạnh trên | Gần nút giao, 2 xe sau-trái chạy ngang (yaw ~0°/360°) |
| s02f55 | Cạnh trên | Đường cong, cây che dày, chỉ 4 xe |
| s03f08 | Cạnh trên | Xe nối đuôi; xe tối màu ít điểm trên kính, nóc |
| s03f51 | Cạnh trên | Xe idx 5 bị cờ chìm, có thể do mặt đất cục bộ là dải phân cách |
| s04f10 | Cạnh trên | 2 xe ở ~31 m phía sau vẫn đủ điểm |
| s04f26 | Cạnh trên | Đường dốc xuống phía trước: z đáy xe ở 20–25 m ≈ −2,3 m |
| s05f16 | Cạnh trên | Xe ngược chiều (yaw ~90°) rẽ bên trái; xe tải ~40 m phía sau |
| s05f31 | Cạnh trên | Không có gì lạ |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist` (chụp trước khi sửa hướng nên ô L/W trên ảnh còn đảo).

| Vật | Frame, idx | Class | Yaw đúng? | Đáy chạm đường? | Kiểm 3 view + ảnh? | Bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con | s01f18, 1 | Car | Đã sửa 0° → 270,0° | Có, lệch 0,08 m | Có (cam01/03) | 4,18 × 1,80 × 1,46 m — trong khoảng |
| (b) xe buýt | s01f18, 7 | Bus | Đã sửa → 270,0° | Có, lệch 0,03 m | Có (cam01/03) | 12,37 × 2,65 × 3,37 m — rộng hơn 0,05 m, trong ±5% |
| (c) xe hai bánh | s01f53, 7 | Cyclist | Đã sửa → 270,0° | Có, lệch 0,12 m | Có (cam07) | 1,61 × 0,62 × 1,61 m — trong khoảng |

Vòng hiệu chuẩn: không tham gia theo yêu cầu của giảng viên.

Bỏ qua vì không đủ cơ sở: hàng xe dừng ngoài ~20 m phía trước ở s01f53 (LOW_VISIBILITY); xe tải xanh sau dải cây phía trước-trái ở s05f16 (LOW_VISIBILITY). Dãy xe đạp không người ở s01f53 không vẽ theo guideline.

## 4. Bốn hiện tượng chất lượng (Bước 4)
Cả 4 lấy từ task của tôi.

| Hiện tượng | Frame / ảnh | Nguyên nhân (giả thuyết) | Thấy gì | Hệ quả khi gán |
|---|---|---|---|---|
| 1. Bóng che | s01f18 · `t4-1-bong-che.png` | Giả thuyết: xe buýt cao 3,4 m và dải cây chặn tia laser | Vùng trống sau xe buýt; xe idx 4, 5 sau dải cây chỉ còn phần trên, cao ~1,0 m | Bật occluded; cuboid bao phần suy luận được |
| 2. Thưa ở xa | s01f18 · `t4-2-thua-o-xa.png`, `-b.png` | Giả thuyết: góc giữa các tia cố định nên ở xa các đường quét cách nhau rộng | Xe idx 8 ở 37,4 m: 61 điểm; xe idx 1 ở 6,4 m: 2151 điểm | Thấy ~15–20% thể tích thì vẽ; không thì bỏ, ghi LOW_VISIBILITY |
| 3. Phản xạ thấp | s03f08 · `t4-3-phan-xa-thap.png`, `-b.png` | Giả thuyết: sơn tối màu và kính hấp thụ hoặc phản xạ gương tia laser | Xe tối màu phía trước: điểm ở cản, thân dưới; kính, nóc gần như trống | Chỉ ghi giả thuyết; đối chiếu camera |
| 4. Điểm lạ | s01f18 · `t4-4-diem-la.png` | Giả thuyết: phản xạ nhiều lần (multipath) làm LiDAR tính sai quãng đường | 19 điểm xếp thành vệt ngang dài ~8 m, phía trước ~47 m, z ≈ −22 m (dưới mặt đường ~20 m) | Không gán thành vật |

## 5. Số từ cuboid của tôi (Bước 5)

| Cuboid | z | h | z đáy | dist_m | r | yaw_deg | Nhận xét |
|---|---|---|---|---|---|---|---|
| s03f08, 2, Car | −0,975 m | 1,570 m | −1,760 m | 6,00 m | 6,07 m | 270,0° | Chạm đường |
| s05f16, 6, Car | −0,252 m | 2,061 m | −1,283 m | 8,62 m | 8,63 m | 91,3° | Lơ lửng ~0,4 m |
| s03f51, 5, Car | −1,261 m | 1,524 m | −2,023 m | 14,78 m | 14,83 m | 270,8° | Chìm ~0,32 m |

(a) Gần nhất: s05f31 idx 1, `dist_m` 4,16 m, r 4,27 m. Xa nhất: s05f16 idx 5 (Truck), `dist_m` 40,34 m, r 40,34 m.
(b) Người đi bộ s01f53 idx 10: 0,35 × 0,34 × 1,24 m, cả ba chiều dưới khoảng người lớn (0,4–0,9 × 0,5–0,9 × 1,50–1,95 m) vì chỉ có 37 điểm, đứng sát người khác nên tôi vẽ hộp chật, chân ít điểm nên đáy cao. Xe con s01f18 idx 1: 4,18 × 1,80 × 1,46 m, trong khoảng sedan.
(c) Xe cùng chiều phía trước s03f08 idx 5: 270,0°. Xe ngược chiều s05f16 idx 7: 89,0°. Đúng gần 270° và 90°.

| Cuboid | dist_m | points_in |
|---|---|---|
| s04f10, 1, Car | 4,6 m | 4504 |
| s03f08, 3, Car | 10,0 m | 1216 |
| s05f16, 4, Car | 22,3 m | 342 |
| s01f18, 8, Car | 37,4 m | 61 |

Quy luật: xa gấp ~2 lần thì số điểm còn ~1/4 (gần 1/r²); trên 30 m giảm nhanh hơn vì xe còn bị che. Cùng xu hướng với slide (1968 / 718 / 214 / 72 điểm ở 0–10 / 10–20 / 20–30 / 30–40 m); xe 37,4 m của tôi 61 điểm, gần 72.

Đã sửa sau `selfcheck`: s01f18 idx 1 và s01f53 idx 7 — vạch đỏ nằm ngang thân (L < W, yaw 0°), xoay 90° thành yaw 270,0° (sửa tương tự cho 66/67 cuboid). Giữ nguyên: s01f18 idx 4, 5 (bị dải cây che nửa dưới, đã bật occluded); s01f53 idx 10 (điểm sát mép là người đứng cạnh, idx 11); s03f51 idx 5 (có thể mặt đất cục bộ là dải phân cách — chưa chắc). s03f51 idx 2 và s05f16 idx 6 (lơ lửng) chưa kịp sửa.

Đổi kiểm: chưa đổi kiểm với bạn nào; tự đối chiếu với bảng A hai số `dist_m` s05f31 idx 1 = 4,16 m và `yaw_deg` s05f16 idx 7 = 89,0°, đều khớp.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
(a) Model học vị trí, kích thước, hướng của vật trong không gian thật để xe tính khoảng cách và quỹ đạo, nên nhãn phải đo bằng mét chứ không phải pixel vốn đổi theo camera và khoảng cách. Vật thưa điểm dễ bị vẽ sai hoặc bỏ sót không nhất quán, nên cần quy tắc riêng (15–20% thể tích, occluded, LOW_VISIBILITY) để model không học nhãn nhiễu.

(b) Cách dùng AI: cài đặt môi trường và CVAT, các lệnh `scripts/lab.py`, cách dùng CVAT 3D, tóm tắt bài lab, hỗ trợ tổng hợp thông tin.

(c) Chưa chắc: hai xe lơ lửng chưa hạ đáy; xe bên kia dải phân cách ở s01f18, s02f55 có thể là xe ngược chiều (yaw phải ~90° thay vì 270°); hộp người đi bộ s01f53 idx 10 có thể quá nhỏ.
