# Đề tài: Đo lường hiệu suất máy móc và tìm nguyên nhân gây lãng phí trong quá trình sản xuất thông qua chỉ số OEE

## Bối cảnh:
Công ty sản xuất vỏ ống nhựa chuyên đựng silicone:
- Sản phẩm: thân ống (Cartridge), Vòi (Nozzle) và Nắp (Plunger).
- 2 công đoạn chính: Ép tạo hình (tạo ra Cartridge, Nozzle, Plunger) và In lên Cartridge.
- Các máy móc có hiệu suất khác nhau (mức độ hư hỏng, tốc độ chạy, chất lượng sản phẩm tạo ra khác nhau). Cần tìm ra những máy chạy tốt, máy chạy kém và phương án để đem lại hiệu quả sản xuất cao nhất.
## Mục tiêu:
- Số hóa hiệu suất hoạt động của máy móc thành các chỉ số có thể đo lường, từ đó tìm ra phương pháp nâng cao hiệu quả sản xuất.
## Dữ liệu:
- Nguồn dữ liệu: file GoogleSheet của công ty (được nhập liệu hàng ngày).
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
## Phương pháp:
- Tổng hợp và làm sạch dữ liệu: xử lí dữ liệu bị thiếu/nhập sai/outliner (GoogleSheet/PowerQuery).
- Tính toán: tính các chỉ số liên quan, phân tích sự tương quan của các yếu tố ảnh hưởng OEE (GoogleSheet/Python/PowerBI-DAX).
- Trực quan hóa dữ liệu: Python/PowerBI.
## Kết quả mong đợi:
- Đánh giá hiệu suất máy móc hiện tại (xấu/trung bình/tốt).
- Tìm ra những yếu tố gây lãng phí trong sản xuất và phương pháp để cải thiện.
 

