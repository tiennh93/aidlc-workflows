# Tạo Đơn vị (Units Generation) - Các bước Chi tiết

## Tổng quan

Giai đoạn này phân rã hệ thống thành các đơn vị công việc có thể quản lý thông qua hai phần tích hợp:

- **Phần 1 - Lập kế hoạch**: Tạo kế hoạch phân rã với các câu hỏi, thu thập câu trả lời, phân tích sự mơ hồ, nhận phê duyệt
- **Phần 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo artifact đơn vị

**ĐỊNH NGHĨA**: Một đơn vị công việc là một nhóm logic các stories cho mục đích phát triển. Đối với vi dịch vụ, mỗi đơn vị trở thành một dịch vụ có thể triển khai độc lập. Đối với monoliths, đơn vị đơn lẻ đại diện cho toàn bộ ứng dụng với các mô-đun logic.

**Thuật ngữ**: Sử dụng "Dịch vụ" cho các thành phần có thể triển khai độc lập, "Mô-đun" cho các nhóm logic trong một dịch vụ, "Đơn vị Công việc" cho ngữ cảnh lập kế hoạch.

## Điều kiện Tiên quyết

- Đánh giá Ngữ cảnh phải hoàn thành
- Đánh giá Yêu cầu được khuyến nghị (cung cấp phạm vi chức năng)
- Phát triển Story được khuyến nghị (stories ánh xạ tới các đơn vị)
- Giai đoạn Thiết kế Ứng dụng BẮT BUỘC (xác định thành phần, phương thức và dịch vụ)
- Kế hoạch thực hiện phải chỉ ra giai đoạn Thiết kế nên thực thi

---

# PHẦN 1: LẬP KẾ HOẠCH

## Bước 1: Tạo Kế hoạch Đơn vị Công việc

- Tạo kế hoạch với checkbox [] để phân rã hệ thống thành các đơn vị công việc
- Tập trung vào việc chia nhỏ hệ thống thành các đơn vị phát triển có thể quản lý
- Mỗi bước và bước con nên có một checkbox []

## Bước 2: Bao gồm Artifact Đơn vị Bắt buộc trong Kế hoạch

**LUÔN LUÔN** bao gồm các artifact bắt buộc này trong kế hoạch đơn vị:

- [ ] Tạo `aidlc-docs/inception/application-design/unit-of-work.md` với định nghĩa đơn vị và trách nhiệm
- [ ] Tạo `aidlc-docs/inception/application-design/unit-of-work-dependency.md` với ma trận phụ thuộc
- [ ] Tạo `aidlc-docs/inception/application-design/unit-of-work-story-map.md` ánh xạ stories tới các đơn vị
- [ ] **Chỉ Greenfield**: Ghi lại chiến lược tổ chức mã trong `unit-of-work.md` (xem code-generation.md cho các mẫu cấu trúc)
- [ ] Xác thực ranh giới đơn vị và phụ thuộc
- [ ] Đảm bảo tất cả các stories được gán cho các đơn vị

## Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích các yêu cầu, stories, và thiết kế ứng dụng để tạo ra CHỈ các câu hỏi liên quan đến vấn đề phân rã cụ thể NÀY. Sử dụng các danh mục bên dưới làm cảm hứng, KHÔNG phải là danh sách kiểm tra bắt buộc. Bỏ qua toàn bộ danh mục nếu không áp dụng.

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào sự mơ hồ và thông tin còn thiếu cụ thể cho ngữ cảnh này
- Chỉ tạo câu hỏi khi đầu vào của người dùng là cần thiết cho việc ra quyết định

**Các danh mục câu hỏi ví dụ** (điều chỉnh khi cần thiết):

- **Nhóm Story** - Chỉ khi nhiều stories tồn tại và chiến lược nhóm không rõ ràng
- **Phụ thuộc** - Chỉ khi có nhiều khả năng đơn vị và cách tiếp cận tích hợp bị mơ hồ
- **Sự liên kết Nhóm** - Chỉ khi cấu trúc nhóm hoặc quyền sở hữu không rõ ràng
- **Cân nhắc Kỹ thuật** - Chỉ khi yêu cầu về khả năng mở rộng/triển khai khác nhau giữa các đơn vị
- **Miền Kinh doanh** - Chỉ khi ranh giới miền hoặc ngữ cảnh giới hạn (bounded contexts) không rõ ràng
- **Tổ chức Mã (Chỉ Greenfield đa đơn vị)** - Hỏi mô hình triển khai và sở thích cấu trúc thư mục

## Bước 4: Lưu trữ Kế hoạch UOW

- Lưu dưới dạng `aidlc-docs/inception/plans/unit-of-work-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng
- Đảm bảo kế hoạch bao gồm tất cả các khía cạnh của phân rã hệ thống

## Bước 5: Yêu cầu Đầu vào Người dùng

- Yêu cầu người dùng điền vào các thẻ [Answer]: trực tiếp trong tài liệu kế hoạch
- Nhấn mạnh tầm quan trọng của các quyết định phân rã
- Cung cấp hướng dẫn rõ ràng về việc hoàn thành các thẻ [Answer]:

## Bước 6: Thu thập Câu trả lời

- Chờ người dùng cung cấp câu trả lời cho tất cả các câu hỏi sử dụng thẻ [Answer]: trong tài liệu
- Không tiếp tục cho đến khi TẤT CẢ các thẻ [Answer]: được hoàn thành
- Xem xét tài liệu để đảm bảo không có thẻ [Answer]: nào bị bỏ trống

## Bước 7: PHÂN TÍCH CÂU TRẢ LỜI (BẮT BUỘC)

Trước khi tiếp tục, bạn PHẢI xem xét cẩn thận tất cả các câu trả lời của người dùng cho:

- **Phản hồi mơ hồ hoặc không rõ ràng**: "kết hợp của", "đâu đó giữa", "không chắc", "phụ thuộc"
- **Tiêu chí hoặc thuật ngữ không xác định**: Tham chiếu đến các khái niệm mà không có định nghĩa rõ ràng
- **Câu trả lời mâu thuẫn**: Phản hồi xung đột với nhau
- **Thiếu chi tiết tạo**: Câu trả lời thiếu hướng dẫn cụ thể
- **Câu trả lời kết hợp các tùy chọn**: Phản hồi hợp nhất các cách tiếp cận khác nhau mà không có quy tắc quyết định rõ ràng

## Bước 8: Câu hỏi Tiếp theo BẮT BUỘC

Nếu phân tích ở bước 7 tiết lộ BẤT KỲ câu trả lời mơ hồ nào, bạn PHẢI:

- Thêm các câu hỏi tiếp theo cụ thể vào tài liệu kế hoạch sử dụng thẻ [Answer]:
- KHÔNG tiếp tục đến phê duyệt cho đến khi tất cả các sự mơ hồ được giải quyết
- Ví dụ về các câu hỏi tiếp theo bắt buộc:
  - "Bạn đã đề cập 'kết hợp của A và B' - tiêu chí cụ thể nào nên xác định khi nào sử dụng A so với B?"
  - "Bạn nói 'đâu đó giữa A và B' - bạn có thể định nghĩa cách tiếp cận trung gian chính xác không?"
  - "Bạn đã chỉ ra 'không chắc' - thông tin bổ sung nào sẽ giúp bạn quyết định?"
  - "Bạn đã đề cập 'phụ thuộc vào độ phức tạp' - bạn định nghĩa các mức độ phức tạp như thế nào?"

## Bước 9: Yêu cầu Phê duyệt

- Hỏi: "**Unit of work plan complete. Review the plan in aidlc-docs/inception/plans/unit-of-work-plan.md. Ready to proceed to generation?**"
- KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận

## Bước 10: Ghi nhật ký Phê duyệt

- Ghi nhật ký nhắc nhở và phản hồi trong audit.md với dấu thời gian
- Sử dụng định dạng dấu thời gian ISO 8601
- Bao gồm văn bản nhắc nhở phê duyệt hoàn chỉnh

## Bước 11: Cập nhật Tiến độ

- Đánh dấu Lập kế hoạch Đơn vị hoàn thành trong aidlc-state.md
- Cập nhật phần "Trạng thái Hiện tại"
- Chuẩn bị chuyển sang Tạo Đơn vị

---

# PHẦN 2: TẠO

## Bước 12: Tải Kế hoạch Đơn vị Công việc

- [ ] Đọc kế hoạch hoàn chỉnh từ `aidlc-docs/inception/plans/unit-of-work-plan.md`
- [ ] Xác định bước chưa hoàn thành tiếp theo (checkbox [ ] đầu tiên)
- [ ] Tải ngữ cảnh và yêu cầu cho bước đó

## Bước 13: Thực thi Bước Hiện tại

- [ ] Thực hiện chính xác những gì bước hiện tại mô tả
- [ ] Tạo artifact đơn vị như được chỉ định trong kế hoạch
- [ ] Tuân theo cách tiếp cận phân rã đã được phê duyệt từ Lập kế hoạch
- [ ] Sử dụng các tiêu chí và ranh giới được chỉ định trong kế hoạch

## Bước 14: Cập nhật Tiến độ

- [ ] Đánh dấu bước đã hoàn thành là [x] trong kế hoạch đơn vị công việc
- [ ] Cập nhật trạng thái hiện tại `aidlc-docs/aidlc-state.md`
- [ ] Lưu tất cả các artifact đã tạo

## Bước 15: Tiếp tục hoặc Hoàn thành

- [ ] Nếu còn bước, quay lại Bước 12
- [ ] Nếu tất cả các bước hoàn thành, xác minh các đơn vị đã sẵn sàng cho các giai đoạn thiết kế
- [ ] Đánh dấu giai đoạn Tạo Đơn vị là hoàn thành

## Bước 16: Trình bày Thông điệp Hoàn thành

```markdown
# 🔧 Units Generation Complete

[AI-generated summary of units and decomposition created in bullet points]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the units generation artifacts at: `aidlc-docs/inception/application-design/`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the units generation if required
> ✅ **Approve & Continue** - Approve units and proceed to **CONSTRUCTION PHASE**
```

## Bước 17: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng việc tạo đơn vị
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật các đơn vị và lặp lại quy trình phê duyệt

## Bước 18: Ghi lại Phản hồi Phê duyệt

- Ghi nhật ký phản hồi phê duyệt của người dùng với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản phản hồi chính xác của người dùng
- Đánh dấu trạng thái phê duyệt rõ ràng

## Bước 19: Cập nhật Tiến độ

- Đánh dấu giai đoạn Tạo Đơn vị hoàn thành trong `aidlc-docs/aidlc-state.md`
- Cập nhật phần "Trạng thái Hiện tại"
- Chuẩn bị chuyển sang GIAI ĐOẠN XÂY DỰNG

---

## Các Quy tắc Quan trọng

### Quy tắc Giai đoạn Lập kế hoạch

- Tạo CHỈ các câu hỏi phù hợp với ngữ cảnh
- Sử dụng định dạng thẻ [Answer]: cho tất cả các câu hỏi
- Phân tích tất cả các câu trả lời cho sự mơ hồ trước khi tiếp tục
- Giải quyết TẤT CẢ sự mơ hồ với các câu hỏi tiếp theo
- Nhận phê duyệt rõ ràng của người dùng trước khi bắt đầu tạo

### Quy tắc Giai đoạn Tạo

- **KHÔNG LOGIC ĐƯỢC MÃ HÓA CỨNG**: Chỉ thực thi những gì được viết trong kế hoạch đơn vị công việc
- **TUÂN THEO KẾ HOẠCH CHÍNH XÁC**: Không đi chệch khỏi trình tự bước
- **CẬP NHẬT CHECKBOX**: Đánh dấu [x] ngay lập tức sau khi hoàn thành mỗi bước
- **SỬ DỤNG CÁCH TIẾP CẬN ĐƯỢC PHÊ DUYỆT**: Tuân theo phương pháp phân rã từ Lập kế hoạch
- **XÁC MINH HOÀN THÀNH**: Đảm bảo tất cả các artifact đơn vị hoàn thành trước khi tiếp tục

## Tiêu chí Hoàn thành

- Tất cả các câu hỏi lập kế hoạch đã được trả lời và sự mơ hồ đã được giải quyết
- Phê duyệt của người dùng đã đạt được cho kế hoạch
- Tất cả các bước trong kế hoạch đơn vị công việc được đánh dấu [x]
- Tất cả các artifact đơn vị được tạo theo kế hoạch:
  - `unit-of-work.md` với các định nghĩa đơn vị
  - `unit-of-work-dependency.md` với ma trận phụ thuộc
  - `unit-of-work-story-map.md` với các ánh xạ story
- Các đơn vị được xác minh và sẵn sàng cho các giai đoạn thiết kế theo đơn vị
