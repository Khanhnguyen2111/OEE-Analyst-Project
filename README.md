Đề tài: Đo lường hiệu suất máy móc và tìm nguyên nhân gây lãng phí trong quá trình sản xuất thông qua chỉ số OEE

Bối cảnh:
Công ty sản xuất vỏ ống nhựa chuyên đựng silicone:
- Sản phẩm: thân ống (Cartridge), Vòi (Nozzle) và Nắp (Plunger).
- 2 công đoạn chính: Ép tạo hình (tạo ra Cartridge, Nozzle, Plunger) và In lên Cartridge.
- Các máy móc có hiệu suất khác nhau (mức độ hư hỏng, tốc độ chạy, chất lượng sản phẩm tạo ra khác nhau). Cần tìm ra những máy chạy tốt, máy chạy kém và phương án để đem lại hiệu quả sản xuất cao nhất.
Mục tiêu:
- Số hóa hiệu suất hoạt động của máy móc thành các chỉ số có thể đo lường, từ đó tìm ra phương pháp nâng cao hiệu quả sản xuất.
Dữ liệu:
- Nguồn dữ liệu: file GoogleSheet của công ty (được nhập liệu hàng ngày).
- Mô tả dữ liệu:
+ Date: ngày hoạt động (date).
+ MC: tên của máy đang hoạt động (string).
+ WO: lệnh sản xuất-một đơn vị đo lường để thuận tiện kiểm soát/truy xuất thông tin (string).
+ Item_ID: mã của sản phẩm đang sản xuất (string).
+ Item name: tên của sản phẩm (string).
+ Type: phân loại sản phẩm (string).
+ Good_qty: số lượng sản phẩm đạt (int).
+ Error_qty: số lượng sản phẩm lỗi (int).
+ Plan_qty: số lượng sản phẩm mục tiêu (int).
+ Start: giờ bắt đầu (time).
+ End: giờ kết thúc (time).
+ Lost_time: thời gian ngưng máy (loat).
+ Reason: lí do ngưng máy (string).
+ Shift: ca làm việc (ngày/đêm) (string).
- Phạm vi dữ liệu: 10 tháng đầu năm 2025.
Phương pháp:
- Tổng hợp và làm sạch dữ liệu: xử lí dữ liệu bị thiếu/nhập sai/outliner (GoogleSheet/PowerQuery).
- Tính toán: tính các chỉ số liên quan, phân tích sự tương quan của các yếu tố ảnh hưởng OEE (GoogleSheet/Python/PowerBI-DAX).
- Trực quan hóa dữ liệu: Python/PowerBI.
Kết quả mong đợi:
- Đánh giá hiệu suất máy móc hiện tại (xấu/trung bình/tốt).
- Tìm ra những yếu tố gây lãng phí trong sản xuất và phương pháp để cải thiện.
 

