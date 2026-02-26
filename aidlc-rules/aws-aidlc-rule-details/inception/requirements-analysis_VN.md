# Phân tích Yêu cầu (Thích ứng) - Requirements Analysis (Adaptive)

**Đóng vai** chủ sở hữu sản phẩm (product owner)

**Giai đoạn Thích ứng**: Luôn thực thi. Mức độ chi tiết thích ứng với độ phức tạp của vấn đề.

**Xem [depth-levels.md](../common/depth-levels.md) để giải thích về độ sâu thích ứng**

## Điều kiện Tiên quyết

- Phát hiện Workspace phải hoàn tất
- Kỹ thuật Đảo ngược phải hoàn tất (nếu brownfield)

## Các Bước Thực thi

### Bước 1: Tải Ngữ cảnh Kỹ thuật Đảo ngược (nếu có sẵn)

**NẾU dự án brownfield**:

- Tải `aidlc-docs/inception/reverse-engineering/architecture.md`
- Tải `aidlc-docs/inception/reverse-engineering/component-inventory.md`
- Tải `aidlc-docs/inception/reverse-engineering/technology-stack.md`
- Sử dụng những thứ này để hiểu hệ thống hiện có khi phân tích yêu cầu

### Bước 2: Phân tích Yêu cầu Người dùng (Phân tích Ý định)

#### 2.1 Sự rõ ràng của Yêu cầu

- **Rõ ràng**: Cụ thể, được xác định rõ, có thể hành động
- **Mơ hồ**: Chung chung, không rõ ràng, cần làm rõ
- **Không đầy đủ**: Thiếu thông tin chính

#### 2.2 Loại Yêu cầu

- **Tính năng Mới**: Thêm chức năng mới
- **Sửa lỗi**: Sửa vấn đề hiện có
- **Refactoring**: Cải thiện cấu trúc mã
- **Nâng cấp**: Cập nhật các phụ thuộc hoặc frameworks
- **Di chuyển**: Chuyển sang công nghệ khác
- **Cải tiến**: Cải thiện tính năng hiện có
- **Dự án Mới**: Bắt đầu từ đầu

#### 2.3 Ước tính Phạm vi Ban đầu

- **Tệp Đơn**: Thay đổi đối với một tệp
- **Thành phần Đơn**: Thay đổi đối với một thành phần/gói
- **Nhiều Thành phần**: Thay đổi trên nhiều thành phần
- **Toàn hệ thống**: Thay đổi ảnh hưởng đến toàn bộ hệ thống
- **Liên hệ thống**: Thay đổi ảnh hưởng đến nhiều hệ thống

#### 2.4 Ước tính Độ phức tạp Ban đầu

- **Tầm thường**: Thay đổi đơn giản, thẳng thắn
- **Đơn giản**: Đường dẫn thực hiện rõ ràng
- **Trung bình**: Một số phức tạp, nhiều cân nhắc
- **Phức tạp**: Độ phức tạp đáng kể, nhiều cân nhắc

### Bước 3: Xác định Độ sâu Yêu cầu

**Dựa trên phân tích yêu cầu, xác định độ sâu:**

**Độ sâu Tối thiểu** - Sử dụng khi:

- Yêu cầu rõ ràng và đơn giản
- Không cần yêu cầu chi tiết
- Chỉ cần ghi lại sự hiểu biết cơ bản

**Độ sâu Tiêu chuẩn** - Sử dụng khi:

- Yêu cầu cần làm rõ
- Cần yêu cầu chức năng và phi chức năng
- Độ phức tạp bình thường

**Độ sâu Toàn diện** - Sử dụng khi:

- Dự án phức tạp với nhiều bên liên quan
- Rủi ro cao hoặc hệ thống quan trọng
- Cần yêu cầu chi tiết với khả năng truy vết

### Bước 4: Đánh giá Yêu cầu Hiện tại

Phân tích bất cứ điều gì người dùng đã cung cấp:

- Các tuyên bố ý định hoặc mô tả (đã đăng nhập trong audit.md)
- Các tài liệu yêu cầu hiện có (tìm kiếm workspace nếu được đề cập)
- Nội dung dán hoặc tham chiếu tệp
- Chuyển đổi bất kỳ tài liệu không phải markdown nào sang định dạng markdown

### Bước 5: Phân tích Tính đầy đủ Kỹ lưỡng

**QUAN TRỌNG**: Sử dụng phân tích toàn diện để đánh giá tính đầy đủ của yêu cầu. Mặc định đặt câu hỏi khi có BẤT KỲ sự mơ hồ hoặc thiếu chi tiết nào.

**BẮT BUỘC**: Đánh giá TẤT CẢ các lĩnh vực này và đặt câu hỏi cho BẤT KỲ lĩnh vực nào không rõ ràng:

- **Yêu cầu Chức năng**: Các tính năng cốt lõi, tương tác người dùng, hành vi hệ thống
- **Yêu cầu Phi Chức năng**: Hiệu năng, bảo mật, khả năng mở rộng, khả năng sử dụng
- **Kịch bản Người dùng**: Use cases, hành trình người dùng, trường hợp biên, kịch bản lỗi
- **Ngữ cảnh Kinh doanh**: Mục tiêu, ràng buộc, tiêu chí thành công, nhu cầu của các bên liên quan
- **Ngữ cảnh Kỹ thuật**: Điểm tích hợp, yêu cầu dữ liệu, ranh giới hệ thống
- **Thuộc tính Chất lượng**: Độ tin cậy, khả năng bảo trì, khả năng kiểm thử, khả năng truy cập

**Khi nghi ngờ, hãy đặt câu hỏi** - yêu cầu không đầy đủ dẫn đến việc triển khai kém.

### Bước 5.1: Các Câu hỏi về Khả năng Áp dụng Extension

**BẮT BUỘC**: Quét tất cả các tệp extension đã tải để tìm phần `## Applicability Question`. Đối với mỗi extension khai báo phần này, hãy đưa câu hỏi đó vào tệp câu hỏi làm rõ được tạo ở Bước 6. Sau khi nhận được câu trả lời, ghi lại trạng thái kích hoạt của từng extension trong `aidlc-docs/aidlc-state.md` dưới phần `## Extension Configuration`:

```markdown
## Extension Configuration

| Extension        | Enabled  | Decided At            |
| ---------------- | -------- | --------------------- |
| [Extension Name] | [Yes/No] | Requirements Analysis |
```

### Bước 6: Tạo Câu hỏi Làm rõ (CÁCH TIẾP CẬN CHỦ ĐỘNG)

- **LUÔN LUÔN** tạo `aidlc-docs/inception/requirements/requirement-verification-questions.md` trừ khi các yêu cầu đặc biệt rõ ràng và đầy đủ
- Đặt câu hỏi về BẤT KỲ lĩnh vực nào thiếu, không rõ ràng hoặc mơ hồ
- Tập trung vào yêu cầu chức năng, yêu cầu phi chức năng, kịch bản người dùng và ngữ cảnh kinh doanh
- Yêu cầu người dùng điền vào tất cả các thẻ [Answer]: trực tiếp trong tài liệu câu hỏi
- Nếu trình bày các tùy chọn trắc nghiệm cho câu trả lời:
  - Dán nhãn các tùy chọn là A, B, C, D v.v.
  - Đảm bảo các tùy chọn loại trừ lẫn nhau và không trùng lặp
  - LUÔN bao gồm tùy chọn cho phản hồi tùy chỉnh: "X) Other (please describe after [Answer]: tag below)"
- Chờ câu trả lời của người dùng trong tài liệu
- **BẮT BUỘC**: Phân tích TẤT CẢ các câu trả lời cho sự mơ hồ và tạo câu hỏi tiếp theo nếu cần
- **BẮT BUỘC**: Tiếp tục đặt câu hỏi cho đến khi TẤT CẢ sự mơ hồ được giải quyết HOẶC người dùng yêu cầu rõ ràng để tiếp tục

### ⛔ CỔNG: Chờ Câu trả lời của Người dùng

KHÔNG tiếp tục đến Bước 7 cho đến khi tất cả các câu hỏi trong requirement-verification-questions.md được trả lời và xác thực.
Trình bày tệp câu hỏi cho người dùng và DỪNG LẠI.

### Bước 7: Tạo Tài liệu Yêu cầu

- **ĐIỀU KIỆN TIÊN QUYẾT**: Cổng Bước 6 phải được thông qua — tất cả câu trả lời đã nhận và phân tích
- Tạo `aidlc-docs/inception/requirements/requirements.md`
- Bao gồm tóm tắt phân tích ý định ở đầu:
  - Yêu cầu người dùng
  - Loại yêu cầu
  - Ước tính phạm vi
  - Ước tính độ phức tạp
- Bao gồm cả yêu cầu chức năng và phi chức năng
- Kết hợp câu trả lời của người dùng cho các câu hỏi làm rõ
- Cung cấp tóm tắt ngắn gọn về các yêu cầu chính

### Bước 8: Cập nhật Theo dõi Trạng thái

Cập nhật `aidlc-docs/aidlc-state.md`:

```markdown
## Stage Progress

### 🔵 INCEPTION PHASE

- [x] Workspace Detection
- [x] Reverse Engineering (if applicable)
- [x] Requirements Analysis
```

### Bước 9: Ghi nhật ký và Tiếp tục

- Ghi nhật ký lời nhắc phê duyệt với dấu thời gian trong `aidlc-docs/audit.md`
- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu bằng dòng này:

```markdown
# 🔍 Requirements Analysis Complete
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc của các yêu cầu
        - Định dạng: "Requirements analysis has identified [project type/complexity]:"
        - Liệt kê các yêu cầu chức năng chính (gạch đầu dòng)
        - Liệt kê các yêu cầu phi chức năng chính (gạch đầu dòng)
        - Đề cập đến các cân nhắc kiến trúc hoặc quyết định kỹ thuật nếu liên quan
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("please review", "let me know", "proceed to next phase", "before we proceed")
        - Giữ tính thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc bằng định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW required:**</u>**  
> Please examine the requirements document at: `aidlc-docs/inception/requirements/requirements.md`
>
> > **🚀 <u>**WHAT'S NEXT?**</u>**
> >
> > **You may:**
> >
> > 🔧 **Request Changes** - Ask for modifications to the requirements if required based on your review
> > [IF User Stories will be skipped, add this option:]
> > 📝 **Add User Stories** - Choose to Include **User Stories** stage (currently skipped based on project simplicity)  
> > ✅ **Approve & Continue** - Approve requirements and proceed to **[User Stories/Workflow Planning]**
>
> ---
```

**Lưu ý**: Chỉ bao gồm tùy chọn "Add User Stories" khi giai đoạn User Stories sẽ bị bỏ qua. Thay thế [User Stories/Workflow Planning] bằng tên giai đoạn tiếp theo thực tế.

- Chờ sự phê duyệt rõ ràng của người dùng trước khi tiếp tục
- Ghi lại phản hồi phê duyệt với dấu thời gian
- Cập nhật giai đoạn Phân tích Yêu cầu hoàn tất trong aidlc-state.md
