# Thiết kế Ứng dụng - Các bước Chi tiết

## Mục đích

**Xác định thành phần cấp cao và thiết kế lớp dịch vụ**

Thiết kế Ứng dụng tập trung vào:

- Xác định các thành phần chức năng chính và trách nhiệm của chúng
- Xác định giao diện thành phần (không phải logic nghiệp vụ chi tiết)
- Thiết kế lớp dịch vụ để điều phối
- Thiết lập các phụ thuộc thành phần và mẫu giao tiếp

**Lưu ý**: Thiết kế logic nghiệp vụ chi tiết diễn ra sau đó trong Thiết kế Chức năng (theo đơn vị, Giai đoạn XÂY DỰNG)

## Điều kiện Tiên quyết

- Đánh giá Ngữ cảnh phải hoàn thành
- Đánh giá Yêu cầu được khuyến nghị (cung cấp ngữ cảnh chức năng)
- Phát triển Story được khuyến nghị (user stories hướng dẫn các quyết định thiết kế)
- Kế hoạch thực hiện phải chỉ ra giai đoạn Thiết kế Ứng dụng nên thực thi

## Thực thi Từng bước

### 1. Phân tích Ngữ cảnh

- Đọc `aidlc-docs/inception/requirements/requirements.md` và `aidlc-docs/inception/user-stories/stories.md`
- Xác định các khả năng nghiệp vụ chính và các khu vực chức năng
- Xác định phạm vi và độ phức tạp của thiết kế

### 2. Tạo Kế hoạch Thiết kế Ứng dụng

- Tạo kế hoạch với checkbox [] cho thiết kế ứng dụng
- Tập trung vào các thành phần, trách nhiệm, phương thức, quy tắc nghiệp vụ, và dịch vụ
- Mỗi bước và bước con nên có một checkbox []

### 3. Bao gồm Các artifact Thiết kế Bắt buộc trong Kế hoạch

- **LUÔN LUÔN** bao gồm các artifact bắt buộc này trong kế hoạch thiết kế:
  - [ ] Tạo components.md với các định nghĩa thành phần và trách nhiệm cấp cao
  - [ ] Tạo component-methods.md với chữ ký phương thức (quy tắc nghiệp vụ chi tiết sau này trong Thiết kế Chức năng)
  - [ ] Tạo services.md với các định nghĩa dịch vụ và mẫu điều phối
  - [ ] Tạo component-dependency.md với các mối quan hệ phụ thuộc và mẫu giao tiếp
  - [ ] Xác thực tính đầy đủ và nhất quán của thiết kế

### 4. Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích các yêu cầu và stories để tạo ra CHỈ các câu hỏi liên quan đến thiết kế ứng dụng cụ thể NÀY. Sử dụng các danh mục bên dưới làm cảm hứng, KHÔNG phải là danh sách kiểm tra bắt buộc. Bỏ qua toàn bộ danh mục nếu không áp dụng.

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào sự mơ hồ và thông tin còn thiếu cụ thể cho ngữ cảnh này
- Chỉ tạo câu hỏi khi đầu vào của người dùng là cần thiết cho các quyết định thiết kế

**Các danh mục câu hỏi ví dụ** (điều chỉnh khi cần thiết):

- **Xác định Thành phần** - Chỉ khi ranh giới hoặc tổ chức thành phần không rõ ràng
- **Phương thức Thành phần** - Chỉ khi chữ ký phương thức cần làm rõ (quy tắc nghiệp vụ chi tiết đến sau)
- **Thiết kế Lớp Dịch vụ** - Chỉ khi điều phối dịch vụ hoặc ranh giới bị mơ hồ
- **Phụ thuộc Thành phần** - Chỉ khi mẫu giao tiếp hoặc quản lý phụ thuộc không rõ ràng
- **Mẫu Thiết kế** - Chỉ khi phong cách kiến trúc hoặc lựa chọn mẫu cần đầu vào của người dùng

### 5. Lưu trữ Kế hoạch Thiết kế Ứng dụng

- Lưu dưới dạng `aidlc-docs/inception/plans/application-design-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng
- Đảm bảo kế hoạch bao gồm tất cả các khía cạnh thiết kế

### 6. Yêu cầu Đầu vào Người dùng

- Yêu cầu người dùng điền vào các thẻ [Answer]: trực tiếp trong tài liệu kế hoạch
- Nhấn mạnh tầm quan trọng của các quyết định thiết kế
- Cung cấp hướng dẫn rõ ràng về việc hoàn thành các thẻ [Answer]:

### 7. Thu thập Câu trả lời

- Chờ người dùng cung cấp câu trả lời cho tất cả các câu hỏi sử dụng thẻ [Answer]: trong tài liệu
- Không tiếp tục cho đến khi TẤT CẢ các thẻ [Answer]: được hoàn thành
- Xem xét tài liệu để đảm bảo không có thẻ [Answer]: nào bị bỏ trống

### 8. PHÂN TÍCH CÂU TRẢ LỜI (BẮT BUỘC)

Trước khi tiếp tục, bạn PHẢI xem xét cẩn thận tất cả các câu trả lời của người dùng cho:

- **Phản hồi mơ hồ hoặc không rõ ràng**: "kết hợp của", "đâu đó giữa", "không chắc", "phụ thuộc"
- **Tiêu chí hoặc thuật ngữ không xác định**: Tham chiếu đến các khái niệm mà không có định nghĩa rõ ràng
- **Câu trả lời mâu thuẫn**: Phản hồi xung đột với nhau
- **Thiếu chi tiết thiết kế**: Câu trả lời thiếu hướng dẫn cụ thể
- **Câu trả lời kết hợp các tùy chọn**: Phản hồi hợp nhất các cách tiếp cận khác nhau mà không có quy tắc quyết định rõ ràng

### 9. Câu hỏi Tiếp theo BẮT BUỘC

Nếu phân tích ở bước 8 tiết lộ BẤT KỲ câu trả lời mơ hồ nào, bạn PHẢI:

- Thêm các câu hỏi tiếp theo cụ thể vào tài liệu kế hoạch sử dụng thẻ [Answer]:
- KHÔNG tiếp tục đến phê duyệt cho đến khi tất cả các sự mơ hồ được giải quyết
- Ví dụ về các câu hỏi tiếp theo bắt buộc:
  - "Bạn đã đề cập 'kết hợp của A và B' - tiêu chí cụ thể nào nên xác định khi nào sử dụng A so với B?"
  - "Bạn nói 'đâu đó giữa A và B' - bạn có thể định nghĩa cách tiếp cận trung gian chính xác không?"
  - "Bạn đã chỉ ra 'không chắc' - thông tin bổ sung nào sẽ giúp bạn quyết định?"
  - "Bạn đã đề cập 'phụ thuộc vào độ phức tạp' - bạn định nghĩa các mức độ phức tạp như thế nào?"

### 10. Tạo Artifact Thiết kế Ứng dụng

- Thực thi kế hoạch đã được phê duyệt để tạo artifact thiết kế
- Tạo `aidlc-docs/inception/application-design/components.md` với:
  - Tên và mục đích thành phần
  - Trách nhiệm của thành phần
  - Giao diện của thành phần
- Tạo `aidlc-docs/inception/application-design/component-methods.md` với:
  - Chữ ký phương thức cho mỗi thành phần
  - Mục đích cấp cao của mỗi phương thức
  - Loại đầu vào/đầu ra
  - Lưu ý: Các quy tắc nghiệp vụ chi tiết sẽ được xác định trong Thiết kế Chức năng (theo đơn vị, Giai đoạn XÂY DỰNG)
- Tạo `aidlc-docs/inception/application-design/services.md` với:
  - Định nghĩa dịch vụ
  - Trách nhiệm của dịch vụ
  - Tương tác và điều phối dịch vụ
- Tạo `aidlc-docs/inception/application-design/component-dependency.md` với:
  - Ma trận phụ thuộc hiển thị các mối quan hệ
  - Mẫu giao tiếp giữa các thành phần
  - Biểu đồ luồng dữ liệu

### 11. Ghi nhật ký Phê duyệt

- Ghi nhật ký nhắc nhở phê duyệt với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản nhắc nhở phê duyệt hoàn chỉnh
- Sử dụng định dạng dấu thời gian ISO 8601

### 12. Trình bày Thông điệp Hoàn thành

```markdown
# 🏗️ Application Design Complete

[AI-generated summary of application design artifacts created in bullet points]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the application design artifacts at: `aidlc-docs/inception/application-design/`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the application design if required
> [IF Units Generation is skipped:]
> 📝 **Add Units Generation** - Choose to include **Units Generation** stage (currently skipped)
> ✅ **Approve & Continue** - Approve design and proceed to **[Units Generation/CONSTRUCTION PHASE]**
```

### 13. Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng thiết kế ứng dụng
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật thiết kế và lặp lại quy trình phê duyệt

### 14. Ghi lại Phản hồi Phê duyệt

- Ghi nhật ký phản hồi phê duyệt của người dùng với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản phản hồi chính xác của người dùng
- Đánh dấu trạng thái phê duyệt rõ ràng

### 15. Cập nhật Tiến độ

- Đánh dấu giai đoạn Thiết kế Ứng dụng hoàn thành trong `aidlc-docs/aidlc-state.md`
- Cập nhật phần "Trạng thái Hiện tại"
- Chuẩn bị chuyển sang giai đoạn tiếp theo
