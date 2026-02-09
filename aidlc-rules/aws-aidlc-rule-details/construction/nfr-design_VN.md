# Thiết kế NFR

## Điều kiện Tiên quyết

- Yêu cầu NFR phải hoàn thành cho đơn vị
- Artifact yêu cầu NFR phải có sẵn
- Kế hoạch thực hiện phải chỉ ra giai đoạn Thiết kế NFR nên thực thi

## Tổng quan

Kết hợp các yêu cầu NFR vào thiết kế đơn vị sử dụng các mẫu và thành phần logic.

## Các bước Thực hiện

### Bước 1: Phân tích Yêu cầu NFR

- Đọc yêu cầu NFR từ `aidlc-docs/construction/{unit-name}/nfr-requirements/`
- Hiểu các nhu cầu về khả năng mở rộng, hiệu năng, tính sẵn sàng, bảo mật

### Bước 2: Tạo Kế hoạch Thiết kế NFR

- Tạo kế hoạch với checkbox [] cho thiết kế NFR
- Tập trung vào các mẫu thiết kế và thành phần logic
- Mỗi bước nên có một checkbox []

### Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích các yêu cầu NFR để tạo ra CHỈ các câu hỏi liên quan đến thiết kế NFR của đơn vị cụ thể NÀY. Sử dụng các danh mục bên dưới làm cảm hứng, KHÔNG phải là danh sách kiểm tra bắt buộc. Bỏ qua toàn bộ danh mục nếu không áp dụng.

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào sự mơ hồ và thông tin còn thiếu cụ thể cho đơn vị này
- Chỉ tạo câu hỏi khi đầu vào của người dùng là cần thiết cho các quyết định về mẫu và thành phần

**Các danh mục câu hỏi ví dụ** (điều chỉnh khi cần thiết):

- **Mẫu Phục hồi** - Chỉ khi cách tiếp cận chịu lỗi cần làm rõ
- **Mẫu Mở rộng** - Chỉ khi cơ chế mở rộng không rõ ràng
- **Mẫu Hiệu năng** - Chỉ khi chiến lược tối ưu hóa hiệu năng bị mơ hồ
- **Mẫu Bảo mật** - Chỉ khi cách tiếp cận triển khai bảo mật cần đầu vào
- **Thành phần Logic** - Chỉ khi các thành phần cơ sở hạ tầng (hàng đợi, bộ đệm, v.v.) cần làm rõ

### Bước 4: Lưu trữ Kế hoạch

- Lưu dưới dạng `aidlc-docs/construction/plans/{unit-name}-nfr-design-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng

### Bước 5: Thu thập và Phân tích Câu trả lời

- Chờ người dùng hoàn thành tất cả các thẻ [Answer]:
- Xem xét các phản hồi mơ hồ hoặc không rõ ràng
- Thêm các câu hỏi tiếp theo nếu cần thiết

### Bước 6: Tạo Artifact Thiết kế NFR

- Tạo `aidlc-docs/construction/{unit-name}/nfr-design/nfr-design-patterns.md`
- Tạo `aidlc-docs/construction/{unit-name}/nfr-design/logical-components.md`

### Bước 7: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu với điều này:

```markdown
# 🎨 NFR Design Complete - [unit-name]
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc về thiết kế NFR
        - Định dạng: "NFR design has incorporated [description]:"
        - Liệt kê các mẫu thiết kế chính được triển khai (gạch đầu dòng)
        - Liệt kê các thành phần logic và yếu tố cơ sở hạ tầng
        - Đề cập đến các mẫu phục hồi, mở rộng và hiệu năng đã áp dụng
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("vui lòng xem lại", "cho tôi biết", "tiếp tục giai đoạn tiếp theo", "trước khi chúng ta tiếp tục")
        - Giữ thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc với định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the NFR design at: `aidlc-docs/construction/[unit-name]/nfr-design/`
>
> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the NFR design based on your review  
> ✅ **Continue to Next Stage** - Approve NFR design and proceed to **[next-stage-name]**
>
> ---
```

### Bước 8: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng thiết kế NFR
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật thiết kế và lặp lại quy trình phê duyệt

### Bước 9: Ghi lại Phê duyệt và Cập nhật Tiến độ

- Ghi nhật ký phê duyệt trong audit.md với dấu thời gian
- Ghi lại phản hồi phê duyệt của người dùng với dấu thời gian
- Đánh dấu giai đoạn Thiết kế NFR hoàn thành trong aidlc-state.md
