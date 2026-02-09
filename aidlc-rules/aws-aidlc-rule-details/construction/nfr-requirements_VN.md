# Yêu cầu NFR

## Điều kiện Tiên quyết

- Thiết kế Chức năng phải hoàn thành hoàn thành cho đơn vị
- Artifact thiết kế chức năng đơn vị phải có sẵn
- Kế hoạch thực hiện phải chỉ ra giai đoạn Yêu cầu NFR nên thực thi

## Tổng quan

Xác định các yêu cầu phi chức năng (Non-Functional Requirements) cho đơn vị và thực hiện các lựa chọn ngăn xếp công nghệ.

## Các bước Thực hiện

### Bước 1: Phân tích Thiết kế Chức năng

- Đọc các artifact thiết kế chức năng từ `aidlc-docs/construction/{unit-name}/functional-design/`
- Hiểu độ phức tạp của logic nghiệp vụ và các yêu cầu

### Bước 2: Tạo Kế hoạch Yêu cầu NFR

- Tạo kế hoạch với checkbox [] cho đánh giá NFR
- Tập trung vào khả năng mở rộng, hiệu năng, tính sẵn sàng, bảo mật
- Mỗi bước nên có một checkbox []

### Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích kỹ lưỡng thiết kế chức năng để xác định TẤT CẢ các lĩnh vực mà sự làm rõ NFR sẽ cải thiện chất lượng hệ thống và quyết định kiến trúc. Hãy chủ động trong việc đặt câu hỏi để đảm bảo bao phủ NFR toàn diện.

**QUAN TRỌNG**: Mặc định đặt câu hỏi khi có BẤT KỲ sự mơ hồ hoặc thiếu chi tiết nào có thể ảnh hưởng đến chất lượng hệ thống. Thà hỏi quá nhiều câu hỏi còn hơn là đưa ra các giả định NFR sai lầm.

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào BẤT KỲ sự mơ hồ, thông tin còn thiếu, hoặc các khu vực cần làm rõ nào
- Tạo câu hỏi bất cứ nơi nào đầu vào của người dùng sẽ cải thiện các quyết định về NFR và ngăn xếp công nghệ
- **Khi nghi ngờ, hãy đặt câu hỏi** - sự quá tự tin dẫn đến chất lượng hệ thống kém

**Các danh mục câu hỏi cần đánh giá** (xem xét TẤT CẢ các danh mục):

- **Yêu cầu Khả năng Mở rộng** - Hỏi về tải lượng mong đợi, mô hình tăng trưởng, trình kích hoạt mở rộng, và lập kế hoạch dung lượng
- **Yêu cầu Hiệu năng** - Hỏi về thời gian phản hồi, thông lượng, độ trễ, và điểm chuẩn hiệu năng
- **Yêu cầu Tính Sẵn sàng** - Hỏi về kỳ vọng thời gian hoạt động, khôi phục thảm họa, chuyển đổi dự phòng, và tính liên tục kinh doanh
- **Yêu cầu Bảo mật** - Hỏi về bảo vệ dữ liệu, tuân thủ, xác thực, ủy quyền, và mô hình mối đe dọa
- **Lựa chọn Ngăn xếp Công nghệ** - Hỏi về sở thích công nghệ, ràng buộc, hệ thống hiện có, và yêu cầu tích hợp
- **Yêu cầu Độ tin cậy** - Hỏi về xử lý lỗi, khả năng chịu lỗi, giám sát, và nhu cầu cảnh báo
- **Yêu cầu Khả năng Bảo trì** - Hỏi về chất lượng mã, tài liệu, kiểm thử, và yêu cầu vận hành
- **Yêu cầu Khả năng Sử dụng** - Hỏi về trải nghiệm người dùng, khả năng truy cập, và yêu cầu giao diện

### Bước 4: Lưu trữ Kế hoạch

- Lưu dưới dạng `aidlc-docs/construction/plans/{unit-name}-nfr-requirements-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng

### Bước 5: Thu thập và Phân tích Câu trả lời

- Chờ người dùng hoàn thành tất cả các thẻ [Answer]:
- **BẮT BUỘC**: Xem xét cẩn thận TẤT CẢ các phản hồi cho các câu trả lời mơ hồ hoặc không rõ ràng
- **QUAN TRỌNG**: Thêm các câu hỏi tiếp theo cho BẤT KỲ phản hồi không rõ ràng nào - không tiếp tục với sự mơ hồ
- Tìm kiếm các phản hồi như "phụ thuộc", "có thể", "không chắc", "kết hợp của", "đâu đó giữa", "tiêu chuẩn", "điển hình"
- Tạo tệp câu hỏi làm rõ nếu BẤT KỲ sự mơ hồ nào được phát hiện
- **Không tiếp tục cho đến khi TẤT CẢ các sự mơ hồ được giải quyết**

### Bước 6: Tạo Artifact Yêu cầu NFR

- Tạo `aidlc-docs/construction/{unit-name}/nfr-requirements/nfr-requirements.md`
- Tạo `aidlc-docs/construction/{unit-name}/nfr-requirements/tech-stack-decisions.md`

### Bước 7: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu với điều này:

```markdown
# 📊 NFR Requirements Complete - [unit-name]
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc về yêu cầu NFR
        - Định dạng: "NFR requirements assessment has identified [description]:"
        - Liệt kê các yêu cầu chính về khả năng mở rộng, hiệu năng, tính sẵn sàng (gạch đầu dòng)
        - Liệt kê các yêu cầu bảo mật và tuân thủ được xác định
        - Đề cập đến các quyết định ngăn xếp công nghệ và lý do
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("vui lòng xem lại", "cho tôi biết", "tiếp tục giai đoạn tiếp theo", "trước khi chúng ta tiếp tục")
        - Giữ thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc với định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the NFR requirements at: `aidlc-docs/construction/[unit-name]/nfr-requirements/`
>
> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the NFR requirements based on your review  
> ✅ **Continue to Next Stage** - Approve NFR requirements and proceed to **[next-stage-name]**
>
> ---
```

### Bước 8: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng yêu cầu NFR
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật yêu cầu và lặp lại quy trình phê duyệt

### Bước 9: Ghi lại Phê duyệt và Cập nhật Tiến độ

- Ghi nhật ký phê duyệt trong audit.md với dấu thời gian
- Ghi lại phản hồi phê duyệt của người dùng với dấu thời gian
- Đánh dấu giai đoạn Yêu cầu NFR hoàn thành trong aidlc-state.md
