# Đề tài: Đo lường hiệu suất máy móc và tìm nguyên nhân gây lãng phí trong quá trình sản xuất thông qua chỉ số OEE

## 1.Giới thiệu:
### Bối cảnh:
Công ty sản xuất vỏ ống nhựa chuyên đựng silicone:
- 2 công đoạn chính: Ép tạo hình (tạo ra sản phẩm Ống (CT), Vòi (NZ) và Nắp (PG). Sau đó in họa tiết lên thân ống (SC).
- Có nhiều máy móc đang hoạt động và tạo ra các loại sản phẩm khác nhau: việc theo dõi hiệu quả sản xuất đang dựa trên phỏng đoán: “máy A có vẻ đang chạy chậm”, “gần đây thấy máy B hư nhiều” ..v..v..
### Mục tiêu:
Áp dụng các chỉ số tính toán để có thể đo lường hiệu suất hoạt động của máy móc. Từ đó tìm ra phương pháp nâng cao hiệu quả sản xuất.

### Dữ liệu:
- Nguồn và Phạm vi dữ liệu: file GoogleSheet của công ty (10 tháng đầu năm 2025).
- Mô tả dữ liệu:
+ Date: ngày hoạt động
+ MC: tên của máy đang hoạt động
+ WO: lệnh sản xuất
+ item_id: mã sản phẩm
+ mould_id: mã khuôn
+ product_type: nhóm sản phẩm
+ qty_actual: tổng số lượng sản phẩm
+ qty_good: số lượng sản phẩm đạt
+ qty_defect: số lượng sản phẩm lỗi
+ weight_total: khối lượng của sản phẩm đạt
+ weight_defect: khối lượng của sản phẩm lỗi
+ work_hours: thời gian làm việc
+ lost_hours_total: thời gian dừng máy
+ qty_plan: sản lượng kế hoạch
+ capacity: công suất tiêu chuẩn (pcs/h)
+ start: giờ bắt đầu
+ end: giờ kết thúc
+ shift: ca làm việc (ngày/đêm) (string).
+ actual_capacity: công suất thực tế (pcs/h)
- Phạm vi dữ liệu: 10 tháng đầu năm 2025.
### Phương pháp:
- Tổng hợp và làm sạch dữ liệu: xử lí dữ liệu bị nhập thiếu/nhập sai (GoogleSheet/Python)
- Tính toán: tính các chỉ số cần thiết cho quá trình phân tích (GoogleSheet, PowerBI-DAX).
- Trực quan hóa dữ liệu: PowerBI.
### Kết quả mong đợi:
- Đánh giá hiệu suất máy móc hiện tại (xấu/tốt, điểm mạnh/điểm yếu ..v..v..).
- Tìm ra những yếu tố gây lãng phí trong sản xuất và phương pháp để cải thiện.
## 2.Xử lí dữ liệu:
### Làm sạch dữ liệu với Python:
Link Google Collab: https://colab.research.google.com/drive/1Sd5Ytxo4ZZAUzg-nViaIKhYms38vSiN-?usp=sharing

Xử lí ô trống không có dữ liệu:

```Python #máy in không có mould id nên gán ký tự SC:
df['mould_id'] = df['mould_id'].fillna('SC')
#những ngày ngưng máy toàn thời gian, sản lượng = 0:
df['shot'] = df['shot'].fillna('0')
df['qty_actual'] = df['qty_actual'].fillna('0')
df['qty_good'] = df['qty_good'].fillna('0')
df['weight_actual'] = df['weight_actual'].fillna('0')
df['actual_cycle_time'] = df['actual_cycle_time'].fillna('0')
#những ca không có phế:
df['qty_defect'] = df['qty_defect'].fillna('0')
df['weight_defect'] = df['weight_defect'].fillna('0')
#những ca không ngưng máy:
df['lost_hours_total'] = df['lost_hours_total'].fillna('0')
df['lost_hours_plan'] = df['lost_hours_plan'].fillna('0')
df['gram/pcs'] = df['gram/pcs'].fillna('0')
df['qty_plan'] = df['qty_plan'].fillna('0')
```
Chuyển về đúng định dạng:
```#Python chuyển định dạng:
df['qty_actual'] = df['qty_actual'].astype('int')
df['qty_good'] = df['qty_good'].astype('int')
df['qty_defect'] = df['qty_defect'].astype('int')
df['qty_plan']= df['qty_plan'].astype('int')
df['weight_actual'] = df['weight_actual'].astype('float')
df['weight_defect'] = df['weight_defect'].astype('float')
df['product_type'] = df['product_type'].astype('category')
df['mould_id'] = df['mould_id'].astype('category')
df['lost_hours_total'] = df['lost_hours_total'].astype('float')
df['lost_hours_plan']= df['lost_hours_plan'].astype('float')
df['actual_capacity']= df['actual_capacity'].astype('float')
df['actual_cycle_time']= df['actual_cycle_time'].astype('float')
```
### Tính toán các chỉ số bằng DAX:
```DAX Availability = DIVIDE(SUM(cleaned_data[Run_time]),SUM(cleaned_data[work_hours]))
Run_time = cleaned_data[work_hours]-cleaned_data[lost_hours_total]
capacity_h_per_pcs = DIVIDE(1,cleaned_data[capacity])
Availability = DIVIDE(SUM(cleaned_data[Run_time]),SUM(cleaned_data[work_hours]))
Performance = DIVIDE(SUMX(cleaned_data, cleaned_data[capacity_h_per_pcs]*cleaned_data[qty_actual]),SUM(cleaned_data[Run_time]))
Quality = DIVIDE(SUM(cleaned_data[qty_good]),SUM(cleaned_data[qty_actual]))
OEE = [Availability]*[Performance]*[Quality]
```
## 3.Nội dung phân tích:
### 3.1. OEE tổng quan và Đánh giá theo máy:
![OEEtong](./OEEtong/OEEtong.png) 

- OEE đạt 85.94% (tham khảo các công ty trong cùng lĩnh vực, chỉ số OEE trong ngành ép nhựa: đạt từ 80% trở lên là mức tốt).

=> OEE có luôn duy trì mức tốt như vậy không ?
![OEEtheomay](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEtheomay.png)

- OEE by Product type:
   + Có 2 nhóm đạt trên mức trung bình (NZ,PG) và 2 nhóm dưới tb (CT, SC).
- OEE by Machine:
   + Tuy OEE cao nhưng không đồng đều: 10/16 máy có OEE thấp hơn mức trung bình.
   + Hai máy NZ01 và PG03 có OEE cao vượt trội.

=> Hiệu suất nhà máy thực sự tốt hay do chỉ số các máy cao đột biến kéo lên ?

<p align="center">
  <img src="https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEwithout.png" width="400">
  <img src="https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEwithouttable.png" width="400">
</p>

Mức độ tác động đến OEE: CT -> NZ -> SC -> PG:
CT và SC có OEE thấp nhưng tác động nhiều: vì có số lượng máy nhiều và sản xuất nhóm sản phẩm chính.

=> cần cải thiện hiệu suất 2 nhóm máy này.

PG có OEE cao nhưng tác động ít: sản phẩm phụ nên máy ít chạy hơn.

=> tìm thêm đơn hàng hoặc đổi khuôn để máy chạy phụ nhóm sản phẩm chính.
#### 3.1.1. Cách cải thiện nhóm máy SC:
![OEEsc](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEsc.png)

Các máy SC: tốc độ chạy nhanh, ít sản phẩm lỗi nhưng dừng máy nhiều
(từ tháng 6 đến tháng 8 Availability giảm hơn 12%).
![OEEsc2](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEsc2.png)

Máy SC06 ngưng máy để Test 82 giờ và Lack of manpower 89 tiếng (chiếm phần lớn nhất).

=> Ngưng máy do các yếu tố khách quan thường tập trung vào máy SC06: chuyên dùng để Test và ưu tiên tắt máy khi thiếu công nhân => nên OEE SC06 thấp.
![OEEsc3](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEsc3.png)

Tổng thời gian off để sửa máy (mục MC) trong khoảng tháng 7 - tháng 10 là 502 giờ (~64%):

=> Các máy SC đang “xuống cấp” kể từ tháng 7 , cần xem lại công tác bảo trì máy.
#### 3.1.2. Cách cải thiện nhóm máy CT:
![OEEct](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEct.png)

Từ tháng 9, Performance tăng 8% và Availability giảm 3%: chỉnh tốc độ máy chạy nhanh hơn nhưng ngưng máy nhiều hơn.

=> Tiếp tục theo dõi thời gian ngưng máy, tìm điểm cân bằng giữa tốc độ và sự ổn định, có thể thử sử dụng các máy PG.
#### Tổng kết 3.1:
- OEE nhà máy đạt 85.94%:
- OEE cao nhưng không duy trì ổn định qua các tháng:
+ 2 cụm máy SC và CT đang có OEE thấp. Cụm máy PG có OEE cao nhưng chạy các sản phẩm phụ nên máy ít sử dụng và ít tác động đến OEE toàn nhà máy hơn.
+ Cụm máy SC:Các máy SC có tốc độ chạy cao, nhưng ngưng để sửa chữa máy nhiều (đặc biệt là từ tháng 7).

=> Cần xem lại công tác bảo trì các máy SC, có thể xem xét giảm tốc độ để máy ổn định hơn.
+ Cụm máy CT:Tốc độ máy thấp và ngưng máy nhiều.

=> Từ tháng 9 đã tăng tốc độ chạy máy, nhưng cũng làm máy hư hỏng phải ngưng nhiều hơn (cân bằng giữa tốc độ và sự ổn định máy, có thể xem xét dùng các máy PG.
### 3.2. Đánh giá OEE theo tháng:
![OEEthang](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEthang.png)

OEE không phải luôn đạt mức tốt: tháng 8 giảm mạnh (hơn 6%), và vẫn chưa hồi phục lại.
Nguyên nhân chính là do Availability giảm.
#### 3.2.1.Phân tích Availability:
![OEEthang8](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEthang8.png)

Availability tháng 8 của nhóm NZ, PG, SC giảm khoảng 7%.

=> Nguyên nhân ngưng máy chủ yếu là do Thiếu lao động thời vụ (off 351 giờ ~ 35.6%)
#### 3.2.2.Phân tích yếu tố "Thiếu lao động thời vụ:
![ThieuThoiVu](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/thieuthoivu.png)

Thiếu hụt lao động thời vụ làm OEE giảm mạnh (8%) trong tháng 8 và cả tháng 9, 10 (nguyên nhân là do lao động thời vụ đăng ký đi làm cho các hãng bánh trung thu).

=> Cần chuẩn bị nhân lực khi sắp đến các mùa lễ tết: tuyển thêm công nhân chính thức, thuê thêm công ty cung cấp lao động thời vụ hoặc thưởng thêm tiền để giữ chân người lao động.
### 3.3. Đánh giá OEE theo ca:
![OEEtheoca](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/OEEtheoca.png)

=> Chỉ số Quality ca đêm thường xuyên cao hơn ca ngày (ít sản phẩm lỗi hơn).
#### 3.3.1.Phân tích OEE theo ca của từng cụm máy:
![oeecavamay](https://raw.githubusercontent.com/Khanhnguyen2111/OEE-Analyst-Project/main/OEEtong/oeecavamay.png)

- Cụm máy CT, NZ, PG do công nhân thời vụ ngồi máy: ca ngày thường xuyên  có nhiều sản phẩm lỗi hơn: Có thể công nhân thời vụ của ca đêm làm việc tốt hơn ca ngày, cần xem lại nhà cung cấp thời vụ phổ biến nhất của từng ca.
- Các máy SC do công nhân chính thức của công ty ngồi máy: Chất lượng sản phẩm giữa 2 ca khá đồng đều.
## 4.Kết luận:
- OEE nhà máy đạt 85.94% (OEE cao nhưng chưa duy trì ổn định qua các tháng).
- Trong tháng 8, OEE giảm mạnh (6%) và ở mức thấp nhất từ đầu năm tới nay.
   + Do các cụm máy NZ, PG, CT phải ngưng nhiều vì thiếu hụt lao động thời vụ, đồng thời cụm máy SC bị hư hỏng nhiều nhất.

=> Cần chuẩn bị nguồn nhân lực trước các dịp lễ, tết: tìm thêm nhà cung cấp thời vụ, tạo các khoản tiền thưởng thêm vào mùa cao điểm để giữ chân người lao động.
- Về tình trạng máy móc:
   + Cụm máy SC tốc độ cao nhưng có xu hướng hư hỏng nhiều từ tháng 7 tới nay.

=> Xem lại công tác bảo trì máy, có thể giảm tốc độ chạy máy.
   + Cụm máy CT được điều chỉnh tăng tốc độ từ tháng 9, nhưng thời gian ngưng máy cũng nhiều hơn.

=> Xem xét giảm tốc độ chạy lại để máy ổn định, nghiên cứu sử dụng các máy PG để chạy phụ các máy chính.
- Chất lượng sản phẩm ca đêm tốt hơn ca ngày (ở cụm máy có công nhân thời vụ):

=> Xem lại các công ty cung cấp lao động thời vụ, xem công nhân thời vụ thuộc công ty nào có hiệu quả công việc tốt nhất.
## 5.Mở rộng phân tích: 
- Phân tích sự tương quan giữa các yếu tố: Thời gian ngưng máy, Tốc độ chạy máy và Số lượng sản phẩm không đạt (nếu có) => để tìm ra Tốc độ chạy máy lý tưởng (chạy nhanh nhất, ít ngưng máy và ít phế phẩm nhất).











  
 

