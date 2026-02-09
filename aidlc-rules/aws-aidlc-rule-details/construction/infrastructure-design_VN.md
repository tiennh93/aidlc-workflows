# Thiết kế Cơ sở hạ tầng

## Điều kiện Tiên quyết

- Thiết kế Chức năng phải hoàn thành cho đơn vị
- Thiết kế NFR được khuyến nghị (cung cấp các thành phần logic để ánh xạ)
- Kế hoạch thực hiện phải chỉ ra giai đoạn Thiết kế Cơ sở hạ tầng nên thực thi

## Tổng quan

Ánh xạ các thành phần phần mềm logic tới các lựa chọn cơ sở hạ tầng thực tế cho môi trường triển khai.

## Các bước Thực hiện

### Bước 1: Phân tích Artifact Thiết kế

- Đọc thiết kế chức năng từ `aidlc-docs/construction/{unit-name}/functional-design/`
- Đọc thiết kế NFR từ `aidlc-docs/construction/{unit-name}/nfr-design/` (nếu tồn tại)
- Xác định các thành phần logic cần cơ sở hạ tầng

### Bước 2: Tạo Kế hoạch Thiết kế Cơ sở hạ tầng

- Tạo kế hoạch với checkbox [] cho thiết kế cơ sở hạ tầng
- Tập trung vào ánh xạ tới các dịch vụ thực tế (AWS, Azure, GCP, on-premise)
- Mỗi bước nên có một checkbox []

### Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích thiết kế chức năng và NFR để tạo ra CHỈ các câu hỏi liên quan đến nhu cầu cơ sở hạ tầng của đơn vị cụ thể NÀY. Sử dụng các danh mục bên dưới làm cảm hứng, KHÔNG phải là danh sách kiểm tra bắt buộc. Bỏ qua toàn bộ danh mục nếu không áp dụng.

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào sự mơ hồ và thông tin còn thiếu cụ thể cho đơn vị này
- Chỉ tạo câu hỏi khi đầu vào của người dùng là cần thiết cho các quyết định cơ sở hạ tầng

**Các danh mục câu hỏi ví dụ** (điều chỉnh khi cần thiết):

- **Môi trường Triển khai** - Chỉ khi nhà cung cấp đám mây hoặc thiết lập môi trường không rõ ràng
- **Cơ sở hạ tầng Tính toán** - Chỉ khi lựa chọn dịch vụ tính toán cần làm rõ
- **Cơ sở hạ tầng Lưu trữ** - Chỉ khi lựa chọn cơ sở dữ liệu hoặc lưu trữ bị mơ hồ
- **Cơ sở hạ tầng Nhắn tin** - Chỉ khi các dịch vụ nhắn tin/xếp hàng cần chỉ định
- **Cơ sở hạ tầng Mạng** - Chỉ khi cách tiếp cận cân bằng tải hoặc cổng API không rõ ràng
- **Cơ sở hạ tầng Giám sát** - Chỉ khi công cụ quan sát cần làm rõ
- **Cơ sở hạ tầng Chia sẻ** - Chỉ khi chiến lược chia sẻ cơ sở hạ tầng bị mơ hồ

### Bước 4: Lưu trữ Kế hoạch

- Lưu dưới dạng `aidlc-docs/construction/plans/{unit-name}-infrastructure-design-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng

### Bước 5: Thu thập và Phân tích Câu trả lời

- Chờ người dùng hoàn thành tất cả các thẻ [Answer]:
- Xem xét các phản hồi mơ hồ hoặc không rõ ràng
- Thêm các câu hỏi tiếp theo nếu cần thiết

### Bước 6: Tạo Artifact Thiết kế Cơ sở hạ tầng

- Tạo `aidlc-docs/construction/{unit-name}/infrastructure-design/infrastructure-design.md`
- Tạo `aidlc-docs/construction/{unit-name}/infrastructure-design/deployment-architecture.md`
- Nếu cơ sở hạ tầng chia sẻ: Tạo `aidlc-docs/construction/shared-infrastructure.md`

### Bước 7: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu với điều này:

```markdown
# 🏢 Infrastructure Design Complete - [unit-name]
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc về thiết kế cơ sở hạ tầng
        - Định dạng: "Infrastructure design has mapped [description]:"
        - Liệt kê các dịch vụ cơ sở hạ tầng chính và thành phần (gạch đầu dòng)
        - Liệt kê các quyết định kiến trúc triển khai và lý do
        - Đề cập đến các lựa chọn nhà cung cấp đám mây và ánh xạ dịch vụ
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("vui lòng xem lại", "cho tôi biết", "tiếp tục giai đoạn tiếp theo", "trước khi chúng ta tiếp tục")
        - Giữ thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc với định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the infrastructure design at: `aidlc-docs/construction/[unit-name]/infrastructure-design/`
>
> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the infrastructure design based on your review  
> ✅ **Continue to Next Stage** - Approve infrastructure design and proceed to **Code Generation**
>
> ---
```

### Bước 8: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng thiết kế cơ sở hạ tầng
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật thiết kế và lặp lại quy trình phê duyệt

### Bước 9: Ghi lại Phê duyệt và Cập nhật Tiến độ

- Ghi nhật ký phê duyệt trong audit.md với dấu thời gian
- Ghi lại phản hồi phê duyệt của người dùng với dấu thời gian
- Đánh dấu giai đoạn Thiết kế Cơ sở hạ tầng hoàn thành trong aidlc-state.md
