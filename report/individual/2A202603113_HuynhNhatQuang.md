# Báo cáo Day 13 — Huỳnh Nhật Quang, 2A202603113

## 1. Kết luận trước
Mã số `2A202603113`, task `D13-2A202603113`. 10 frame: s01f30, s01f50, s02f21, s02f54, s03f06, s03f23, s04f05, s04f31, s05f01, s05f27. Hệ điều hành: Linux. Trình duyệt: Chrome. Phiên bản CVAT: 2.83.1

Tổng cộng 82 cuboid (69 Car, 7 Cyclist, 3 Pedestrian, 3 Truck), 27 cuboid bật occluded. `selfcheck` cờ 3 cuboid trên 82 (s01f50#9, s02f54#3, s05f27#8). Phần tin nhất là hướng của vật và yếu nhất là xác định đáy của vật.
Cách dùng CVAT, 3 dòng: 
(1) đổi góc nhìn (nhìn từ trên xuống, xoay, dời). 
(2) vẽ và chỉnh một cuboid (Shape, nháy đúp, Top/Side/Front). 
(3) bật mũi tên hướng (Cuboid orientation) và xem cuboid trên ảnh camera (`preview`).

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ: (s01f30 và s01f50). Ảnh: `t1-s01f30-3d`, `t1-s01f50-top`.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f30 | Phía trước xe cạnh trên của ảnh, tia x chỉ qua trái, tia y chỉ về sau | Phẳng; z đáy 8 cuboid từ −1,57 đến −1,93 m nên mặt đường thấp hơn LiDAR khoảng 1,6–1,9 m | ~49 m (s01f30#8, chỉ 34 điểm); camera khó nhận ra | 7 Car, 1 Truck (s01f30#6, 30,8 m) | 7/8 cuboid ở phía sau (y > 0): s01f30#2 (8,1 m, sau phải), #3 (16,7 m), và hàng #5–#8 (24–49 m, đều occluded) ở phía sau bên trái; chỉ s01f30#1 ở phía trước (trước trái, 11,3 m) | Hai dải điểm mờ chạy dọc hai bên đường (nhiều khả năng là lề hoặc dải phân cách, chưa xác định) và vài điểm rải rác ngoài đường: không gán thành vật |
| s01f50 | Phía trước là −y; +x sang trái xe, +y ra sau | Phẳng; z đáy 4 xe phía trước từ −1,63 đến −1,76 m nên mặt đường thấp hơn LiDAR khoảng 1,6–1,8 m | ~21 m (s01f50#4, Car, 55 điểm, occluded); cam01 còn thấy xe và biển chỉ đường xa hơn nhưng chưa có cuboid | 6 Car, 2 Cyclist, 2 Pedestrian | Bên phải có 2 Cyclist (s01f50#7 sau phải, #8 trước phải) và 2 Pedestrian (#9, #10, trước phải, occluded); phía sau có Car s01f50#5 (7,1 m) và #6 (10,2 m) | s01f50#4 (20,9 m, 55 điểm) và hai Pedestrian s01f50#9, #10 (36 và 84 điểm) khó đọc; cam01 thấy dãy xe xa phía trước nhưng chưa gán nhãn |


Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f21 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Cyclist s02f21#5 (6,1 m) và Pedestrian s02f21#4 (12,5 m) ở gần; xe ngược chiều (yaw ≈ 90°). Điểm thưa, nền tối, khó thấy mép đường |
| s02f54 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Chỉ 5 Car, xếp chéo thành dải xiên phía trước (đường cong nhẹ); điểm rất thưa, mặt đường gần như không thấy |
| s03f06 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Kẹt xe: dãy Car nối đuôi dày đặc phía trước, cam03 và cam05 thấy xe trắng sát xe thu; có Truck s03f06#7 (22,6 m, occluded) và Cyclist s03f06#9 bên trái; nhiều cuboid bị che |
| s03f23 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Chỉ 5 Car, cách xa nhau và nhỏ trên ảnh; cam01 thấy trạm có mái đỏ bên phải đường, điểm ở xa rất thưa |
| s04f05 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Đường rộng nhiều làn; Car s04f05#1 rất gần (3,6 m, 6507 điểm); có một cuboid nét đứt (occluded) ở xa phía trước bên trái; hàng rào vàng ở mép đường |
| s04f31 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Đường rộng, hàng rào cam; có Cyclist s04f31#4 (6,5 m) gần xe thu, Car s04f31#6 xa 40,6 m ít điểm; hai cuboid nét đứt (occluded) ở xa |
| s05f01 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Truck s05f01#4 (12,6 m, 2577 điểm) lớn nổi bật phía trước bên phải; Car s05f01#7 ở 33,6 m chỉ 17 điểm; vài cuboid nét đứt (occluded) |
| s05f27 | Phía trước là −y (về phía cam01); +x sang trái xe, +y ra sau | Gần ngã tư có vạch qua đường (cam05); 2 Cyclist ở hai phía (s05f27#4 và #10, #10 có z đáy −2,26 m, thấp hơn mặt đường ~0,5 m nên cần xem lại); 11 cuboid, nhiều nét đứt |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f30#2 (Car, cách 8,13 m, 1587 điểm, ở phía sau bên phải xe thu) | Car | Nháp: yaw 272,5°, cùng chiều xe thu (≈ 270°), cần mở lại xem mũi tên có chỉ đúng đầu xe | Nháp: z đáy −1,565 m, gần mặt đường (≈ −1,7 m) | Nháp: đã xem Top/Side/Front và ảnh cam, cần xác nhận lại | Có: 4,597 × 1,900 × 1,400 m trong khoảng sedan (3,8–5,0 × 1,70–1,90 × 1,40–1,60 m), rộng 1,90 m sát mép |
| (b) xe buýt hoặc xe tải | s01f30#6 (Truck, cách 30,8 m, 310 điểm, occluded; bảng A không có Bus) | Truck | Nháp: yaw 89,7° (ngược chiều xe thu), cần xác nhận mũi tên | Nháp: z đáy −1,905 m, thấp hơn mặt đường ~0,2 m, cần kiểm lại ở Side | Nháp: cần xác nhận lại | Nháp: 6,3 × 2,3 × 2,4 m, cần đối chiếu dòng Truck trong bảng 4.3 |
| (c) người đi bộ hoặc xe hai bánh | s01f50#8 (Cyclist, cách 10,1 m, 443 điểm, bên phải xe thu) | Cyclist | Nháp: yaw 269,8°, cùng chiều xe thu, cần xác nhận | Nháp: z đáy −1,304 m, cao hơn mặt đường ~0,4 m, cần kiểm lại ở Side | Nháp: cần xác nhận lại | Có: 1,847 × 0,646 × 1,654 m trong khoảng xe đạp có người (1,6–1,9 × 0,50–0,70 × 1,60–1,90 m) |


## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | s05f01, #66 / `t4-1-bong-che` | Xe tải lớn đứng chắn ngay phía trước nên LiDAR không quét tới xe nhỏ nằm sau nó (giả thuyết) | Cuboid Car nét đứt (occluded) nằm sát cạnh thân xe tải; phía sau xe tải gần như không có điểm, chỉ vài tia cong và một mảng điểm ở đuôi | Giữ cuboid, bật `occluded`, bao phần suy luận được từ phần điểm còn thấy và ảnh cam |
| 2. Thưa ở xa | s05f01, #64/ `t4-2-thua-o-xa` | Mật độ điểm tỉ lệ nghịch bình phương khoảng cách, nên vật xa chỉ nhận vài tia (giả thuyết) | Hộp ở xa chỉ chứa lác đác vài chục điểm, nhìn thấy một mặt bên; các hộp ở xa hơn nữa gần như chỉ là khung | Vẽ khi còn thấy khoảng 15–20% thể tích, nếu không thì bỏ qua và ghi LOW_VISIBILITY |
| 3. Phản xạ thấp / ít điểm bất thường | s05f72, #79 / `t4-3-phan-xa-thap` | Có thể bề mặt tối, bóng hoặc kính phản xạ yếu, hoặc tia bị trượt; chưa khẳng định được | Hộp nét đứt chỉ có điểm tập trung ở một mặt, phần còn lại gần như trống dù xung quanh điểm vẫn dày | Giữ cuboid theo phần điểm và ảnh cam, ghi chú giả thuyết, không khẳng định nguyên nhân |
| 4. Điểm lạ | s04f05, #50 / `t4-4-diem-la` | Có thể nhiễu, vật nhỏ hoặc phản xạ từ mặt kính, chưa rõ (giả thuyết) | Vài cụm điểm rời rạc nằm ngoài hộp, không tạo thành hình dạng vật rõ ràng | Không gán thành vật; nếu phân vân thì ghi UNCERTAIN_CLASS hoặc bỏ qua |

## 5. Số từ cuboid của tôi (Bước 5)
Số lấy từ `submission/D13-2A202603113-bangA.csv`; số điểm từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m; z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy = z − h/2 (m) | dist_m (m) | r (m) | yaw_deg (°) | Nhận xét |
|---|---|---|---|---|---|---|---|
| s01f30, 1, Car | −0,873 | 1,400 | −1,573 | 11,33 | 11,36 | 270,5 | lệch 0,13 m so với −1,7 m: chạm đường |
| s03f06, 3, Car | −0,981 | 1,500 | −1,731 | 3,19 | 3,34 | 271,2 | lệch 0,03 m: chạm đường |
| s05f01, 4, Truck | −0,066 | 3,100 | −1,616 | 12,55 | 12,55 | 272,7 | lệch 0,08 m: chạm đường |

(a) Cuboid gần nhất: s03f06#3 (Car) có `dist_m` = 3,19 m, r = 3,34 m. Xa nhất: s01f30#8 (Car) có `dist_m` = 49,46 m, r = 49,48 m (sát ngưỡng 50 m). Cuboid này chỉ có 34 điểm và không đủ điểm mặt đường quanh hộp để tính khoảng hở, nên độ tin cậy thấp; tôi giữ vì tâm vẫn trong ngưỡng 50 m và ghi nhận là UNCERTAIN_SCOPE / LOW_VISIBILITY.

(b) Người đi bộ s01f50#10 có l × w × h = 0,381 × 0,500 × 1,357 m: so với bảng 4.3 thì ngoài khoảng người lớn (dài 0,4–0,9 m; cao 1,50–1,95 m) nhưng nằm trong khoảng trẻ em (0,3–0,6 × 0,3–0,6 × 0,90–1,40 m). Có thể là trẻ em, người đang cúi hoặc hộp chưa ôm hết người vì chỉ có 84 điểm và bị che. Xe hai bánh s01f50#7 (Cyclist) có 1,650 × 0,611 × 1,700 m, nằm trong khoảng xe đạp có người (1,6–1,9 × 0,50–0,70 × 1,60–1,90 m). Xe con s01f30#1 có 4,771 × 1,900 × 1,400 m, nằm trong khoảng sedan (3,8–5,0 × 1,70–1,90 × 1,40–1,60 m), rộng và cao ở sát mép khoảng.

(c) Xe cùng chiều phía trước s01f30#1 có `yaw_deg` = 270,5° (gần 270°); xe ngược chiều s02f21#2 có `yaw_deg` = 89,3° (gần 90°).

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m (m) | Số điểm (`points_in`) |
|---|---|---|
| s03f06, 3, Car | 3,19 | 2061 |
| s01f30, 1, Car | 11,33 | 1060 |
| s02f54, 2, Car | 22,60 | 251 |
| s03f23, 2, Car | 38,91 | 67 |

Quy luật: xa gấp khoảng 2 lần (11,3 m → 22,6 m) thì số điểm còn khoảng 1/4 (1060 → 251), gần với tỉ lệ nghịch bình phương khoảng cách. Đường cong ở slide (xe con, trung vị): khoảng 1968 điểm ở 0–10 m, 718 ở 10–20 m, 214 ở 20–30 m, 72 ở 30–40 m. Số của tôi cùng bậc với đường cong (2061 ở 3,2 m; 251 ở 22,6 m; 67 ở 38,9 m); riêng 1060 điểm ở 11,3 m cao hơn trung vị 718 ở 10–20 m, một lý do hợp lý là cuboid này nằm gần đầu dưới của khoảng (11,3 m); số điểm tỉ lệ nghịch bình phương khoảng cách nên một xe ở giữa khoảng (~15 m) sẽ có khoảng 1060 × (11,3/15)² ≈ 600 điểm, gần trung vị 718. Đây cũng chỉ là một cuboid chứ không phải trung vị.

Tự kiểm: hai cuboid đã sửa sau `preview` / `selfcheck`: (1) s01f50, hộp Car cách khoảng 16 m không có điểm nào bên trong (cờ "hộp rỗng"): đã xóa, nên các `idx` sau nó trong frame dịch xuống 1; (2) s05f01#4 (Truck): chỉnh rộng 2,50 → 2,65 m và cao 3,00 → 3,10 m, z đáy từ −1,566 m thành −1,616 m. Cảnh báo còn lại sau lần `sync` cuối (đều là "quá chật"): s01f50#9 (59 điểm sát ngay ngoài mép), s02f54#3 (233 điểm), s05f27#8 (344 điểm). Cả ba cờ giữ nguyên vì hộp đã sát vật, nới thêm sẽ ôm thừa điểm của vật bên cạnh hoặc mặt đường. Vẫn giữ nguyên vì thấy đã vừa với vật. 


## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
(a) Nhãn 3D đo bằng mét vì model cần biết vị trí và kích thước thật của vật trong không gian để phanh, tránh va chạm hoặc lập kế hoạch đường đi, còn pixel đổi theo ống kính và khoảng cách. Vật thưa điểm cần quy tắc riêng vì nhãn đoán từ vài điểm sẽ dạy model sai; có quy tắc 15–20% và LOW_VISIBILITY thì nhãn đáng tin hơn là đoán cho xong.

(b) Dùng AI: Claude (Anthropic). Hỏi "Giải thích các khái niệm và hướng dẫn các bước làm bài." Sau đó tham khảo gợi ý từ AI để làm bài.

(c) Điều còn chưa chắc: (1) s01f30#8 (49,46 m, 34 điểm) và s05f01#7 (33,59 m, 17 điểm) rất thưa, độ chính xác thấp và có thể không ghép được với nhãn chuẩn; (2) ba cờ "quá chật" (s01f50#9, s02f54#3, s05f27#8): chưa phân biệt được điểm sát mép là của vật bên cạnh hay do hộp nhỏ; (3) s05f27#10 (Cyclist) có z đáy −2,263 m, lệch xa −1,7 m, nhưng khoảng hở mặt đường chỉ 0,12 m nên có thể đường ở đó thấp hơn, tôi chưa chắc; (4) bảng A không có class Bus nên ba cuboid mẫu dùng Truck cho (b).
