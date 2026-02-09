# Độ sâu Thích ứng

**Mục đích**: Giải thích cách AI-DLC điều chỉnh mức độ chi tiết phù hợp với độ phức tạp của vấn đề

## Nguyên tắc Cốt lõi

**Khi một giai đoạn thực thi, TẤT CẢ các artifact được định nghĩa của nó sẽ được tạo ra. "Độ sâu" đề cập đến mức độ chi tiết và sự chặt chẽ trong các artifact đó, thứ thích ứng với độ phức tạp của vấn đề.**

## Lựa chọn Giai đoạn vs Mức độ Chi tiết

### Lựa chọn Giai đoạn (Nhị phân)

- **Lập kế hoạch Quy trình làm việc** quyết định: THỰC THI hoặc BỎ QUA cho mỗi giai đoạn
- **Nếu THỰC THI**: Giai đoạn chạy và tạo ra TẤT CẢ các artifact được định nghĩa của nó
- **Nếu BỎ QUA**: Giai đoạn không chạy chút nào

### Mức độ Chi tiết (Thích ứng)

- **Vấn đề đơn giản**: Các artifact ngắn gọn với chi tiết thiết yếu
- **Vấn đề phức tạp**: Các artifact toàn diện với chi tiết mở rộng
- **Model quyết định**: Dựa trên đặc điểm của vấn đề, không phải các quy tắc cứng nhắc

## Các yếu tố Ảnh hưởng đến Mức độ Chi tiết

Model xem xét các yếu tố này khi xác định mức độ chi tiết phù hợp:

1. **Sự rõ ràng của Yêu cầu**: Yêu cầu của người dùng rõ ràng và đầy đủ đến mức nào?
2. **Độ phức tạp của Vấn đề**: Không gian giải pháp phức tạp đến mức nào?
3. **Phạm vi**: Một tệp, thành phần, nhiều thành phần hay toàn hệ thống?
4. **Mức độ Rủi ro**: Tác động của các lỗi hoặc thiếu sót là gì?
5. **Ngữ cảnh Sẵn có**: Dự án mới (Greenfield) vs dự án cũ (brownfield), tài liệu hiện có
6. **Sở thích của Người dùng**: Người dùng có bày tỏ sự ưu tiên cho sự ngắn gọn hay chi tiết không?

## Ví dụ: Artifact Phân tích Yêu cầu

**Tất cả các kịch bản đều tạo ra các artifact giống nhau**:

- `requirement-verification-questions.md` (nếu cần)
- `requirements.md`

**Lưu ý**: Yêu cầu ban đầu của người dùng được ghi lại trong `audit.md` (không cần tệp `user-intent.md` riêng biệt)

**Mức độ chi tiết thay đổi theo độ phức tạp**:

### Kịch bản Đơn giản (Sửa lỗi)

- **requirement-verification-questions.md**: các câu hỏi làm rõ cần thiết
- **requirements.md**: Yêu cầu chức năng ngắn gọn, các phần tối thiểu

### Kịch bản Phức tạp (Di chuyển Hệ thống)

- **requirement-verification-questions.md**: Nhiều vòng, hơn 10 câu hỏi
- **requirements.md**: Yêu cầu chức năng + phi chức năng toàn diện, khả năng truy vết, tiêu chí chấp nhận

## Ví dụ: Artifact Thiết kế Ứng dụng

**Tất cả các kịch bản đều tạo ra các artifact giống nhau**:

- `application-design.md`
- `component-diagram.md`

**Mức độ chi tiết thay đổi theo độ phức tạp**:

### Kịch bản Đơn giản (Một Thành phần)

- **application-design.md**: Mô tả thành phần cơ bản, các phương thức chính
- **component-diagram.md**: Biểu đồ đơn giản với các mối quan hệ thiết yếu

### Kịch bản Phức tạp (Hệ thống Nhiều Thành phần)

- **application-design.md**: Trách nhiệm thành phần chi tiết, tất cả các phương thức với chữ ký, mẫu thiết kế, các lựa chọn thay thế được xem xét
- **component-diagram.md**: Biểu đồ toàn diện với tất cả các mối quan hệ, luồng dữ liệu, điểm tích hợp

## Nguyên tắc Hướng dẫn cho Model

**"Tạo chính xác mức độ chi tiết cần thiết cho vấn đề hiện tại - không hơn, không kém."**

- Không thổi phồng các vấn đề đơn giản với các chi tiết không cần thiết
- Không làm qua loa các vấn đề phức tạp bằng cách bỏ qua chi tiết quan trọng
- Để đặc điểm vấn đề thúc đẩy mức độ chi tiết một cách tự nhiên
- Tất cả các artifact bắt buộc luôn được tạo khi giai đoạn thực thi
