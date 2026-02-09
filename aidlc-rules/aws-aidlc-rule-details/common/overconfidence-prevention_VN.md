# Hướng dẫn Ngăn ngừa Sự quá tự tin

## Tuyên bố Vấn đề

AI-DLC đã thể hiện sự quá tự tin bằng cách không đặt đủ câu hỏi làm rõ, ngay cả đối với các tuyên bố ý định dự án phức tạp. Điều này dẫn đến việc đưa ra các giả định thay vì thu thập các yêu cầu thích hợp.

## Phân tích Nguyên nhân Gốc rễ

Vấn đề quá tự tin gây ra bởi các chỉ thị trong nhiều giai đoạn khuyến khích việc bỏ qua các câu hỏi:

1. **Thiết kế Chức năng**: "Bỏ qua toàn bộ danh mục nếu không áp dụng"
2. **User Stories**: "Sử dụng danh mục làm cảm hứng, KHÔNG phải là danh sách kiểm tra bắt buộc"
3. **Phân tích Yêu cầu**: Các mẫu tương tự khuyến khích việc hỏi tối thiểu
4. **Yêu cầu NFR**: Các điều kiện "Chỉ khi" không khuyến khích phân tích kỹ lưỡng

Những chỉ thị này đang bảo AI tránh đặt câu hỏi thay vì khuyến khích thu thập yêu cầu toàn diện.

## Giải pháp Đã triển khai

### Triết lý Tạo Câu hỏi Đã cập nhật

**CÁCH TIẾP CẬN CŨ**: "Chỉ đặt câu hỏi nếu thực sự cần thiết"
**CÁCH TIẾP CẬN MỚI**: "Khi nghi ngờ, hãy đặt câu hỏi - sự quá tự tin dẫn đến kết quả kém"

### Các thay đổi Chính Đã thực hiện

#### 1. Giai đoạn Phân tích Yêu cầu

- Đã thay đổi từ "chỉ khi cần" thành "LUÔN tạo câu hỏi trừ khi đặc biệt rõ ràng"
- Đã thêm các khu vực đánh giá toàn diện (chức năng, phi chức năng, ngữ cảnh kinh doanh, ngữ cảnh kỹ thuật)
- Nhấn mạnh cách tiếp cận đặt câu hỏi chủ động

#### 2. Giai đoạn User Stories

- Đã xóa chỉ thị "bỏ qua toàn bộ danh mục"
- Đã thêm các danh mục câu hỏi toàn diện để đánh giá
- Tăng cường các yêu cầu phân tích câu trả lời
- Củng cố các yêu cầu câu hỏi tiếp theo

#### 3. Giai đoạn Thiết kế Chức năng

- Thay thế các điều kiện "chỉ khi" bằng đánh giá toàn diện
- Đã thêm nhiều danh mục câu hỏi hơn (luồng dữ liệu, điểm tích hợp, xử lý lỗi)
- Tăng cường phát hiện và giải quyết sự mơ hồ

#### 4. Giai đoạn Yêu cầu NFR

- Mở rộng các danh mục câu hỏi vượt ra ngoài các NFR cơ bản
- Đã thêm các cân nhắc về độ tin cậy, khả năng bảo trì và khả năng sử dụng
- Tăng cường phân tích câu trả lời cho các sự mơ hồ kỹ thuật

### Các nguyên tắc Hướng dẫn Mới

1. **Mặc định là Hỏi**: Khi có bất kỳ sự mơ hồ nào, hãy đặt câu hỏi làm rõ
2. **Bao phủ Toàn diện**: Đánh giá TẤT CẢ các danh mục liên quan, không bỏ qua các khu vực
3. **Phân tích Kỹ lưỡng**: Phân tích cẩn thận TẤT CẢ các phản hồi của người dùng cho các sự mơ hồ
4. **Theo dõi Bắt buộc**: Tạo các câu hỏi tiếp theo cho BẤT KỲ phản hồi không rõ ràng nào
5. **Không Tiến hành với Sự mơ hồ**: Không di chuyển về phía trước cho đến khi TẤT CẢ các sự mơ hồ được giải quyết

## Hướng dẫn Triển khai

### Để Tạo Câu hỏi

- Đánh giá TẤT CẢ các danh mục câu hỏi, không bỏ qua bất kỳ cái nào
- Đặt câu hỏi bất cứ nơi nào sự làm rõ sẽ cải thiện chất lượng
- Bao gồm các danh mục câu hỏi toàn diện trong mỗi giai đoạn
- Mặc định là bao gồm thay vì loại trừ các câu hỏi

### Để Phân tích Câu trả lời

- Tìm kiếm các phản hồi mơ hồ: "phụ thuộc", "có thể", "không chắc", "kết hợp của", "đâu đó giữa"
- Phát hiện các thuật ngữ chưa được định nghĩa và tham chiếu đến các khái niệm bên ngoài
- Xác định các câu trả lời mâu thuẫn hoặc không đầy đủ
- Tạo các câu hỏi tiếp theo cho BẤT KỲ sự mơ hồ nào

### Đối với Câu hỏi Tiếp theo

- Tạo các tệp làm rõ riêng biệt khi phát hiện sự mơ hồ
- Đặt các câu hỏi cụ thể để giải quyết từng sự mơ hồ
- Không tiếp tục cho đến khi TẤT CẢ các phản hồi không rõ ràng được làm rõ
- Hãy kỹ lưỡng - thà làm rõ quá mức còn hơn làm rõ thiếu

## Đảm bảo Chất lượng

### Cờ đỏ cần Theo dõi

- Các giai đoạn hoàn thành mà không đặt bất kỳ câu hỏi nào cho các dự án phức tạp
- Tiến hành với các phản hồi mơ hồ hoặc không rõ ràng của người dùng
- Bỏ qua toàn bộ các danh mục câu hỏi mà không có lý do chính đáng
- Đưa ra giả định thay vì yêu cầu làm rõ

### Chỉ báo Thành công

- Số lượng câu hỏi làm rõ phù hợp với độ phức tạp của dự án
- Phân tích kỹ lưỡng các phản hồi của người dùng với việc theo dõi khi cần thiết
- Yêu cầu rõ ràng, không mơ hồ trước khi tiến sang triển khai
- Giảm nhu cầu thay đổi trong các giai đoạn sau do làm rõ tốt hơn ngay từ đầu

## Bảo trì

Hướng dẫn này nên được tham khảo khi:

- Thêm các giai đoạn mới vào AI-DLC
- Cập nhật hướng dẫn giai đoạn hiện có
- Xem xét hiệu suất AI-DLC cho các vấn đề quá tự tin
- Đào tạo thành viên nhóm về các nguyên tắc tạo câu hỏi AI-DLC

## Rút ra Chính

**Thà hỏi quá nhiều câu hỏi còn hơn đưa ra các giả định sai lầm.** Chi phí của việc đặt câu hỏi làm rõ ngay từ đầu ít hơn nhiều so với chi phí triển khai giải pháp sai dựa trên các giả định.
