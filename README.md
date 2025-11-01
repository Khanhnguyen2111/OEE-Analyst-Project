# Đề tài: Đo lường hiệu suất máy móc và tìm nguyên nhân gây lãng phí trong quá trình sản xuất thông qua chỉ số OEE

## 1.Giới thiệu:
### Bối cảnh:
Công ty sản xuất vỏ ống nhựa chuyên đựng silicone:
- Sản phẩm: thân ống (Cartridge), Vòi (Nozzle) và Nắp (Plunger).
- 2 công đoạn chính: Ép tạo hình (tạo ra Cartridge, Nozzle, Plunger) và In lên Cartridge.
- Các máy móc có hiệu suất khác nhau (mức độ hư hỏng, tốc độ chạy, chất lượng sản phẩm tạo ra khác nhau). Cần tìm ra những máy chạy tốt, máy chạy kém và phương án để đem lại hiệu quả sản xuất cao nhất.
### Mục tiêu:
- Số hóa hiệu suất hoạt động của máy móc thành các chỉ số có thể đo lường, từ đó tìm ra phương pháp nâng cao hiệu quả sản xuất.
### Dữ liệu:
- Nguồn dữ liệu: file GoogleSheet của công ty.
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
+ weight_actual: khối lượng của tổng sản phẩm
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
- Tổng hợp và làm sạch dữ liệu: xử lí dữ liệu bị thiếu/nhập sai (GoogleSheet/Python).
- Tính toán: tính các chỉ số liên quan, phân tích sự tương quan của các yếu tố ảnh hưởng OEE (GoogleSheet/Python/PowerBI-DAX).
- Trực quan hóa dữ liệu: Python/PowerBI.
### Kết quả mong đợi:
- Đánh giá hiệu suất máy móc hiện tại (xấu/trung bình/tốt).
- Tìm ra những yếu tố gây lãng phí trong sản xuất và phương pháp để cải thiện.
## 2.Quy trình triển khai:
### Làm sạch dữ liệu với Python:
Link Google Collab: https://colab.research.google.com/drive/1Sd5Ytxo4ZZAUzg-nViaIKhYms38vSiN-?usp=sharing
Xử lí ô trống:

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
Performance = DIVIDE(SUMX(cleaned_data, cleaned_data[capacity_h_per_pcs]*cleaned_data[qty_actual]),SUM(cleaned_data[Run_time]))
Quality = DIVIDE(SUM(cleaned_data[qty_good]),SUM(cleaned_data[qty_actual]))
OEE = [Availability]*[Performance]*[Quality]
```


  
 

