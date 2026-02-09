# Thiết kế Chức năng (Functional Design)

## Mục đích

**Thiết kế logic nghiệp vụ chi tiết theo đơn vị**

Thiết kế Chức năng tập trung vào:

- Logic nghiệp vụ và thuật toán chi tiết cho đơn vị
- Mô hình miền với các thực thể và mối quan hệ
- Quy tắc nghiệp vụ chi tiết, logic xác thực và ràng buộc
- Thiết kế bất khả tri về công nghệ (không lo ngại về cơ sở hạ tầng)

**Lưu ý**: Điều này xây dựng dựa trên thiết kế thành phần cấp cao từ Thiết kế Ứng dụng (Giai đoạn KHỞI TẠO)

## Điều kiện Tiên quyết

- Tạo Đơn vị phải hoàn tất
- Các artifact đơn vị công việc phải có sẵn
- Khuyến nghị Thiết kế Ứng dụng (cung cấp cấu trúc thành phần cấp cao)
- Kế hoạch thực thi phải chỉ ra giai đoạn Thiết kế Chức năng nên thực thi

## Tổng quan

Thiết kế logic nghiệp vụ chi tiết cho đơn vị, bất khả tri về công nghệ và tập trung hoàn toàn vào các chức năng nghiệp vụ.

## Các Bước Thực thi

### Bước 1: Phân tích Ngữ cảnh Đơn vị

- Đọc định nghĩa đơn vị từ `aidlc-docs/inception/application-design/unit-of-work.md`
- Đọc các story được giao từ `aidlc-docs/inception/application-design/unit-of-work-story-map.md`
- Hiểu trách nhiệm và ranh giới của đơn vị

### Bước 2: Tạo Kế hoạch Thiết kế Chức năng

- Tạo kế hoạch với các hộp kiểm [] cho thiết kế chức năng
- Tập trung vào logic nghiệp vụ, mô hình miền, quy tắc nghiệp vụ
- Mỗi bước nên có một hộp kiểm []

### Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích kỹ lưỡng định nghĩa đơn vị và các artifact thiết kế chức năng để xác định TẤT CẢ các lĩnh vực mà việc làm rõ sẽ cải thiện thiết kế chức năng. Chủ động đặt câu hỏi để đảm bảo hiểu biết toàn diện.

**QUAN TRỌNG**: Mặc định đặt câu hỏi khi có BẤT KỲ sự mơ hồ hoặc thiếu chi tiết nào có thể ảnh hưởng đến chất lượng thiết kế chức năng. Thà hỏi quá nhiều câu hỏi còn hơn đưa ra những giả định sai lầm.

- NHÚNG câu hỏi bằng định dạng thẻ [Answer]:
- Tập trung vào BẤT KỲ sự mơ hồ, thông tin thiếu, hoặc các lĩnh vực cần làm rõ
- Tạo câu hỏi ở bất cứ đâu đầu vào của người dùng sẽ cải thiện các quyết định thiết kế chức năng
- **Khi nghi ngờ, hãy hỏi câu hỏi** - sự quá tự tin dẫn đến các thiết kế kém

**Các danh mục câu hỏi cần xem xét** (đánh giá TẤT CẢ danh mục):

- **Mô hình hóa Logic Nghiệp vụ** - Hỏi về các thực thể cốt lõi, quy trình làm việc, chuyển đổi dữ liệu và quy trình nghiệp vụ
- **Mô hình Miền** - Hỏi về các khái niệm miền, mối quan hệ thực thể, cấu trúc dữ liệu và đối tượng nghiệp vụ
- **Quy tắc Nghiệp vụ** - Hỏi về các quy tắc quyết định, logic xác thực, ràng buộc và chính sách nghiệp vụ
- **Luồng Dữ liệu** - Hỏi về đầu vào dữ liệu, đầu ra, chuyển đổi và yêu cầu bền vững
- **Điểm Tích hợp** - Hỏi về tương tác hệ thống bên ngoài, API và trao đổi dữ liệu
- **Xử lý Lỗi** - Hỏi về các kịch bản lỗi, thất bại xác thực và xử lý ngoại lệ
- **Kịch bản Nghiệp vụ** - Hỏi về các trường hợp biên, luồng thay thế và tình huống nghiệp vụ phức tạp
- **Frontend Components** (nếu áp dụng) - Hỏi về cấu trúc thành phần UI, tương tác người dùng, quản lý trạng thái và xử lý biểu mẫu

### Bước 4: Lưu Kế hoạch

- Lưu dưới dạng `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào người dùng

### Bước 5: Thu thập và Phân tích Câu trả lời

- Chờ người dùng hoàn thành tất cả các thẻ [Answer]:
- **BẮT BUỘC**: Xem xét cẩn thận TẤT CẢ các phản hồi cho các câu trả lời mơ hồ hoặc không rõ ràng
- **QUAN TRỌNG**: Thêm các câu hỏi tiếp theo cho BẤT KỲ phản hồi không rõ ràng nào - không tiếp tục với sự mơ hồ
- Tìm kiếm các phản hồi như "tùy thuộc", "có thể", "không chắc", "pha trộn của", "đâu đó giữa"
- Tạo tệp câu hỏi làm rõ nếu phát hiện BẤT KỲ sự mơ hồ nào
- **Không tiếp tục cho đến khi TẤT CẢ sự mơ hồ được giải quyết**

### Bước 6: Tạo Artifact Thiết kế Chức năng

- Tạo `aidlc-docs/construction/{unit-name}/functional-design/business-logic-model.md`
- Tạo `aidlc-docs/construction/{unit-name}/functional-design/business-rules.md`
- Tạo `aidlc-docs/construction/{unit-name}/functional-design/domain-entities.md`
- Nếu đơn vị bao gồm frontend/UI: Tạo `aidlc-docs/construction/{unit-name}/functional-design/frontend-components.md`
  - Phân cấp và cấu trúc thành phần
  - Định nghĩa Props và state cho mỗi thành phần
  - Luồng tương tác người dùng
  - Quy tắc xác thực biểu mẫu
  - Điểm tích hợp API (mỗi thành phần sử dụng endpoint backend nào)

### Bước 7: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu bằng dòng này:

```markdown
# 🔧 Functional Design Complete - [unit-name]
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc của thiết kế chức năng
        - Định dạng: "Functional design has created [description]:"
        - Liệt kê các mô hình logic nghiệp vụ chính và thực thể (gạch đầu dòng)
        - Liệt kê các quy tắc nghiệp vụ và logic xác thực được xác định
        - Đề cập đến cấu trúc mô hình miền và mối quan hệ
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("please review", "let me know", "proceed to next phase", "before we proceed")
        - Giữ tính thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc bằng định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the functional design artifacts at: `aidlc-docs/construction/[unit-name]/functional-design/`
>
> > **🚀 <u>**WHAT'S NEXT?**</u>**
> >
> > **You may:**
> >
> > 🔧 **Request Changes** - Ask for modifications to the functional design based on your review  
> > ✅ **Continue to Next Stage** - Approve functional design and proceed to **[next-stage-name]**
>
> ---
```

### Bước 8: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt thiết kế chức năng một cách rõ ràng
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật thiết kế và lặp lại quy trình phê duyệt

### Bước 9: Ghi lại Phê duyệt và Cập nhật Tiến độ

- Ghi nhật ký phê duyệt trong audit.md với dấu thời gian
- Ghi lại phản hồi phê duyệt của người dùng với dấu thời gian
- Đánh dấu giai đoạn Thiết kế Chức năng hoàn tất trong aidlc-state.md
