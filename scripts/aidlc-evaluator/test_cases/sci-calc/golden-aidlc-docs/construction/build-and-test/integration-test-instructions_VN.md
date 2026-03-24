# Hướng Dẫn Kiểm Thử Tích Hợp (Integration Test Instructions)

## Mục Đích (Purpose)
Kiểm tra toàn bộ luồng pipeline từ request → route → engine → response trên tất cả 7 miền API.

## Các Kịch Bản Kiểm Thử (Test Scenarios)

### Kịch Bản 1: Các Phép Tính Số Học (Arithmetic Operations)
- **Các Endpoints**: POST `/api/v1/arithmetic/{add,subtract,multiply,divide,modulo,abs,negate}`
- **Số bài kiểm thử**: 12 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: phép toán phân nhánh nhị phân hai ngôi (binary ops), phép toán một ngôi (unary ops), chia cho 0 (division-by-zero), đầu vào gán lỗi không hợp lệ, từ chối NaN, lỗi 404

### Kịch Bản 2: Lũy Thừa & Căn Bậc (Powers & Roots)
- **Các Endpoints**: POST `/api/v1/powers/{power,sqrt,cbrt,square,nth_root}`
- **Số bài kiểm thử**: 9 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: tất cả các thao tác, lỗi miền giá trị (domain errors), tràn bộ nhớ (overflow), lỗi 404

### Kịch Bản 3: Các Phép Tính Lượng Giác (Trigonometry)
- **Các Endpoints**: POST `/api/v1/trigonometry/{sin,cos,tan,asin,acos,atan,atan2,sinh,cosh,tanh,asinh,acosh,atanh}`
- **Số bài kiểm thử**: 8 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: các đơn vị radian/độ (degrees), lỗi miền giá trị (domain errors), hàm độ cong hyperbolic, lỗi 404

### Kịch Bản 4: Mảng Hàm Logarit (Logarithmic)
- **Các Endpoints**: POST `/api/v1/logarithmic/{ln,log10,log2,log,exp}`
- **Số bài kiểm thử**: 9 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: tất cả các thao tác, lỗi miền giá trị (domain errors), tràn bộ nhớ (overflow), lỗi 404

### Kịch Bản 5: Các Phép Đo Thống Kê (Statistics)
- **Các Endpoints**: POST `/api/v1/statistics/{mean,median,mode,stdev,variance,pstdev,pvariance,min,max,sum,count}`
- **Số bài kiểm thử**: 14 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: tất cả các thao tác logic, lỗi về vùng miền giá trị (domain errors), quy cách xác thực đầu vào danh sách mảng rỗng (empty input validation), lỗi 404

### Kịch Bản 6: Các Hằng Số (Constants)
- **Các Endpoints**: GET `/api/v1/constants/` và GET `/api/v1/constants/{name}`
- **Số bài kiểm thử**: 4 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: thiết lập kết xuất kéo lấy ra một hằng số đơn lẻ, liệt kê duyệt truy tìm tất cả, tham chiếu danh sách hằng số ảo lập lòe không xác định được với (mã cảnh báo cờ 404)

### Kịch Bản 7: Các Lệnh Các Quy Đổi Kèm Quét Dò Tình Trạng Sức Khỏe Mạng Máy (Conversions & Health)
- **Các Endpoints**: POST `/api/v1/conversions/{angle,temperature,length,weight}`, và phương thức GET `/health`
- **Số bài kiểm thử**: Mang vào 7 bài kiểm thử tích hợp
- **Phạm vi bảo đảm kiểm tra (Covers)**: gồng gánh tất cả các loại chuyển đổi (conversion types), rào cản từ chối với nhóm danh mục không xác định, bộ đơn vị không xác thực nắm được, dò trạm kiểm tra quét mức tình trạng định biên sức khỏe server phần cứng mã nguồn rỗng (health check)

## Chạy Các Bài Kiểm Thử Tích Hợp (Run Integration Tests)
```bash
# Tất cả các bài kiểm thử tích hợp đều đã được đính nằm trong cùng một chặng thư mục test tệp đi kẹp chung ngang hàng với các gói mã tệp bài mẫu kiểm thử lớp đơn vị (unit tests)
# Các bản này nhắm thiết ứng trực tiếp gõ mượn công cụ đồ mô phỏng hỗ trợ fixture có tên gọi biến `client` đóng gôm vắt từ khối file tệp conftest.py
set PYTHONPATH=src
python -m pytest tests/ -v -k "API" -p no:anyio -p no:asyncio
```

## Kết Quả Mong Đợi Mức Nhắm Đạt Thấy Kết Tụ (Expected Results)
- **Tổng số bài kiểm thử kiểm duyệt tích hợp toàn vòng (Total Integration Tests)**: 63
- **Tất cả các rào kiểm duyệt test phải đi lướt đạt**: ✅
- **Xác thực định dạng bao cấu trúc thành trả của chuỗi mạng tín hiệu khung lưới phản hồi bóc gỡ ra (Response format validated)**: trúng vào việc test trả trạng thái tình trạng lệnh HTTP status, tính rõ kết mã của định danh thao tác tham biến operation, dò kỹ tút nạp chuỗi mảng nhập gõ inputs đầu ngõ, dò gạn chốt kết mã đáp lời chênh lệch kết quả (hàng chốt result định biên kết số hợp hoặc bung ra cấu trúc tin nhắn vấp chập cờ báo lỗi).
