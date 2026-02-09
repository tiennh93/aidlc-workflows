# User Stories - Các bước Chi tiết

## Mục đích

**Chuyển đổi yêu cầu thành các câu chuyện lấy người dùng làm trung tâm với các tiêu chí chấp nhận**

User Stories tập trung vào:

- Dịch các yêu cầu kinh doanh thành các câu chuyện kể lấy người dùng làm trung tâm
- Xác định các tiêu chí chấp nhận rõ ràng cho mỗi câu chuyện
- Tạo chân dung người dùng (personas) đại diện cho các loại bên liên quan khác nhau
- Thiết lập sự hiểu biết chung giữa các nhóm
- Cung cấp các thông số kỹ thuật có thể kiểm thử cho việc triển khai

## Điều kiện Tiên quyết

- Phát hiện Workspace phải hoàn thành
- Phân tích Yêu cầu được khuyến nghị (có thể tham khảo yêu cầu nếu có)
- Lập kế hoạch Quy trình làm việc phải chỉ ra giai đoạn User Stories nên thực thi

## Hướng dẫn Đánh giá Thông minh

**KHI NÀO THỰC THI USER STORIES**: Sử dụng đánh giá nâng cao này trước khi tiếp tục:

### Thực thi Ưu tiên Cao (LUÔN LUÔN Thực thi)

- **Tính năng Người dùng Mới**: Bất kỳ chức năng mới nào người dùng sẽ tương tác trực tiếp
- **Thay đổi Trải nghiệm Người dùng**: Sửa đổi quy trình làm việc hoặc giao diện người dùng hiện có
- **Hệ thống Đa Persona**: Ứng dụng phục vụ các loại người dùng khác nhau
- **API Hướng Khách hàng**: Dịch vụ mà người dùng hoặc hệ thống bên ngoài sẽ tiêu thụ
- **Logic Nghiệp vụ Phức tạp**: Yêu cầu với nhiều kịch bản hoặc quy tắc nghiệp vụ
- **Dự án Chéo Nhóm**: Công việc đòi hỏi sự hiểu biết chung giữa nhiều nhóm

### Thực thi Ưu tiên Trung bình (Đánh giá Độ phức tạp)

- **Tác động Người dùng Backend**: Thay đổi nội bộ ảnh hưởng gián tiếp đến trải nghiệm người dùng
- **Cải tiến Hiệu năng**: Nâng cấp với lợi ích có thể nhìn thấy cho người dùng
- **Công việc Tích hợp**: Kết nối các hệ thống ảnh hưởng đến quy trình làm việc của người dùng
- **Thay đổi Dữ liệu**: Sửa đổi ảnh hưởng đến dữ liệu người dùng, báo cáo hoặc phân tích
- **Nâng cao Bảo mật**: Thay đổi ảnh hưởng đến xác thực hoặc quyền của người dùng

### Các yếu tố Đánh giá Độ phức tạp

Đối với các trường hợp ưu tiên trung bình, thực thi user stories nếu BẤT KỲ điều nào sau đây áp dụng:

- **Phạm vi**: Thay đổi trải rộng nhiều thành phần hoặc điểm tiếp xúc người dùng
- **Sự mơ hồ**: Yêu cầu có các khía cạnh không rõ ràng mà stories có thể làm rõ
- **Rủi ro**: Tác động kinh doanh cao hoặc tiềm năng hiểu lầm
- **Bên liên quan**: Nhiều bên liên quan kinh doanh tham gia vào yêu cầu
- **Kiểm thử**: Kiểm thử chấp nhận người dùng sẽ được yêu cầu
- **Tùy chọn**: Tồn tại nhiều cách tiếp cận triển khai hợp lệ

### Chỉ Bỏ qua Cho Các trường hợp Đơn giản

- **Refactoring Thuần túy**: Cải tiến mã nội bộ với tác động người dùng bằng không
- **Sửa Lỗi Cô lập**: Các bản sửa lỗi đơn giản, được xác định rõ với phạm vi rõ ràng
- **Chỉ Cơ sở hạ tầng**: Thay đổi không có hiệu ứng hướng người dùng
- **Công cụ Nhà phát triển**: Quy trình xây dựng, CI/CD, hoặc thay đổi môi trường phát triển
- **Tài liệu**: Cập nhật không ảnh hưởng đến chức năng

### Quy tắc Quyết định Mặc định

**Khi nghi ngờ, bao gồm user stories VÀ đặt câu hỏi làm rõ.** Chi phí tạo ra các câu chuyện toàn diện với sự làm rõ thích hợp thường thấp hơn so với lợi ích của:

- Hiểu yêu cầu rõ ràng hơn
- Sự liên kết nhóm tốt hơn
- Tiêu chí kiểm thử được cải thiện
- Giao tiếp với các bên liên quan được nâng cao
- Giảm rủi ro triển khai
- Ít thay đổi tốn kém hơn trong quá trình phát triển
- Kết quả trải nghiệm người dùng tốt hơn

---

# PHẦN 1: LẬP KẾ HOẠCH

## Bước 1: Xác thực Nhu cầu User Stories (BẮT BUỘC)

**QUAN TRỌNG**: Trước khi tiếp tục với user stories, thực hiện đánh giá này:

### Quy trình Đánh giá

1. **Phân tích Ngữ cảnh Yêu cầu**:
   - Xem lại yêu cầu người dùng ban đầu và các yêu cầu
   - Xác định thay đổi hướng người dùng so với chỉ nội bộ
   - Đánh giá độ phức tạp và phạm vi công việc
   - Đánh giá sự tham gia của các bên liên quan kinh doanh

2. **Áp dụng Tiêu chí Đánh giá**:
   - Kiểm tra các chỉ số Ưu tiên Cao (luôn luôn thực thi)
   - Đánh giá các yếu tố Ưu tiên Trung bình (quyết định dựa trên độ phức tạp)
   - Xác nhận đây không phải là trường hợp đơn giản nên bỏ qua

3. **Ghi lại Quyết định Đánh giá**:
   - Tạo `aidlc-docs/inception/plans/user-stories-assessment.md`
   - Bao gồm lý do tại sao user stories có giá trị cho yêu cầu này
   - Tham chiếu các tiêu chí đánh giá cụ thể áp dụng
   - Giải thích lợi ích mong đợi (sự rõ ràng, kiểm thử, sự liên kết các bên liên quan)

4. **Chỉ Tiếp tục Nếu Được Biện minh**:
   - User stories phải thêm giá trị rõ ràng cho dự án
   - Đánh giá phải cho thấy lợi ích cụ thể lớn hơn chi phí
   - Quyết định phải có thể bảo vệ trước các bên liên quan dự án

### Mẫu Tài liệu Đánh giá

```markdown
# User Stories Assessment

## Request Analysis

- **Original Request**: [Brief summary]
- **User Impact**: [Direct/Indirect/None]
- **Complexity Level**: [Simple/Medium/Complex]
- **Stakeholders**: [List involved parties]

## Assessment Criteria Met

- [ ] High Priority: [List applicable criteria]
- [ ] Medium Priority: [List applicable criteria with complexity justification]
- [ ] Benefits: [Expected value from user stories]

## Decision

**Execute User Stories**: [Yes/No]
**Reasoning**: [Detailed justification]

## Expected Outcomes

- [List specific benefits user stories will provide]
- [How stories will improve project success]
```

## Bước 2: Tạo Kế hoạch Story

- Đóng vai trò của một product owner
- Tạo một kế hoạch toàn diện với danh sách kiểm tra thực thi từng bước cho việc phát triển story
- Mỗi bước và bước con nên có một checkbox []
- Tập trung vào phương pháp luận và cách tiếp cận để chuyển đổi yêu cầu thành user stories

## Bước 3: Tạo Câu hỏi Phù hợp Ngữ cảnh

**CHỈ THỊ**: Phân tích kỹ lưỡng các yêu cầu và ngữ cảnh để xác định TẤT CẢ các lĩnh vực mà sự làm rõ sẽ cải thiện chất lượng story và sự hiểu biết của nhóm. Hãy chủ động trong việc đặt câu hỏi để đảm bảo phát triển user story toàn diện.

**QUAN TRỌNG**: Mặc định đặt câu hỏi khi có BẤT KỲ sự mơ hồ hoặc thiếu chi tiết nào có thể ảnh hưởng đến chất lượng story. Thà hỏi quá nhiều câu hỏi còn hơn là tạo ra các stories không đầy đủ hoặc không rõ ràng.

**Xem `common/question-format-guide.md` cho các quy tắc định dạng câu hỏi**

- NHÚNG câu hỏi sử dụng định dạng thẻ [Answer]:
- Tập trung vào BẤT KỲ sự mơ hồ, thông tin còn thiếu, hoặc các khu vực cần làm rõ nào
- Tạo câu hỏi bất cứ nơi nào đầu vào của người dùng sẽ cải thiện các quyết định tạo story
- **Khi nghi ngờ, hãy đặt câu hỏi** - sự quá tự tin dẫn đến các stories kém

**Các danh mục câu hỏi cần đánh giá** (xem xét TẤT CẢ các danh mục):

- **Chân dung Người dùng (User Personas)** - Hỏi về loại người dùng, vai trò, đặc điểm, và động lực
- **Độ chi tiết Story** - Hỏi về mức độ chi tiết phù hợp, kích thước story, và cách tiếp cận phân rã
- **Định dạng Story** - Hỏi về sở thích định dạng, sử dụng mẫu, và tiêu chuẩn tài liệu
- **Cách tiếp cận Phân rã** - Hỏi về phương pháp tổ chức, ưu tiên, và chiến lược nhóm
- **Tiêu chí Chấp nhận** - Hỏi về mức độ chi tiết, định dạng, cách tiếp cận kiểm thử, và phương pháp xác thực
- **Hành trình Người dùng** - Hỏi về quy trình làm việc người dùng, mẫu tương tác, và luồng trải nghiệm
- **Ngữ cảnh Kinh doanh** - Hỏi về mục tiêu kinh doanh, chỉ số thành công, và nhu cầu của các bên liên quan
- **Ràng buộc Kỹ thuật** - Hỏi về hạn chế kỹ thuật, yêu cầu tích hợp, và ranh giới hệ thống

## Bước 4: Bao gồm Artifact Story Bắt buộc trong Kế hoạch

- **LUÔN LUÔN** bao gồm các artifact bắt buộc này trong kế hoạch story:
  - [ ] Tạo stories.md với user stories tuân theo tiêu chí INVEST
  - [ ] Tạo personas.md với các nguyên mẫu người dùng và đặc điểm
  - [ ] Đảm bảo stories là Independent, Negotiable, Valuable, Estimable, Small, Testable
  - [ ] Bao gồm tiêu chí chấp nhận cho mỗi story
  - [ ] Ánh xạ personas tới các user stories liên quan

## Bước 5: Trình bày Các Tùy chọn Story

- Bao gồm các cách tiếp cận khác nhau cho phân rã story trong tài liệu kế hoạch:
  - **Dựa trên Hành trình Người dùng**: Stories tuân theo quy trình làm việc và tương tác người dùng
  - **Dựa trên Tính năng**: Stories được tổ chức quanh các tính năng và khả năng hệ thống
  - **Dựa trên Persona**: Stories được nhóm theo các loại người dùng khác nhau và nhu cầu của họ
  - **Dựa trên Miền**: Stories được tổ chức quanh các miền kinh doanh hoặc ngữ cảnh
  - **Dựa trên Epic**: Stories được cấu trúc như các epics phân cấp với các sub-stories
- Giải thích sự đánh đổi và lợi ích của mỗi cách tiếp cận
- Cho phép các cách tiếp cận lai với các tiêu chí quyết định rõ ràng

## Bước 6: Lưu trữ Kế hoạch Story

- Lưu kế hoạch story hoàn chỉnh với các câu hỏi được nhúng trong thư mục `aidlc-docs/inception/plans/`
- Tên tệp: `story-generation-plan.md`
- Bao gồm tất cả các thẻ [Answer]: cho đầu vào của người dùng
- Đảm bảo kế hoạch là toàn diện và bao gồm tất cả các khía cạnh phát triển story

## Bước 7: Yêu cầu Đầu vào Người dùng

- Yêu cầu người dùng điền vào tất cả các thẻ [Answer]: trực tiếp trong tài liệu kế hoạch story
- Nhấn mạnh tầm quan trọng của dấu vết kiểm toán và tài liệu quyết định
- Cung cấp hướng dẫn rõ ràng về cách điền vào các thẻ [Answer]:
- Giải thích rằng tất cả các câu hỏi phải được trả lời trước khi tiếp tục

## Bước 8: Thu thập Câu trả lời

- Chờ người dùng cung cấp câu trả lời cho tất cả các câu hỏi sử dụng thẻ [Answer]: trong tài liệu
- Không tiếp tục cho đến khi TẤT CẢ các thẻ [Answer]: được hoàn thành
- Xem xét tài liệu để đảm bảo không có thẻ [Answer]: nào bị bỏ trống

## Bước 9: PHÂN TÍCH CÂU TRẢ LỜI (BẮT BUỘC)

Trước khi tiếp tục, bạn PHẢI xem xét cẩn thận tất cả các câu trả lời của người dùng cho:

- **Phản hồi mơ hồ hoặc không rõ ràng**: "kết hợp của", "đâu đó giữa", "không chắc", "phụ thuộc", "có thể", "có lẽ"
- **Tiêu chí hoặc thuật ngữ không xác định**: Tham chiếu đến các khái niệm mà không có định nghĩa rõ ràng
- **Câu trả lời mâu thuẫn**: Phản hồi xung đột với nhau
- **Thiếu chi tiết tạo**: Câu trả lời thiếu hướng dẫn cụ thể cho việc triển khai
- **Câu trả lời kết hợp các tùy chọn**: Phản hồi hợp nhất các cách tiếp cận khác nhau mà không có quy tắc quyết định rõ ràng
- **Giải thích không đầy đủ**: Câu trả lời tham chiếu đến các yếu tố bên ngoài mà không định nghĩa chúng
- **Phản hồi dựa trên giả định**: Câu trả lời giả định kiến thức không được nêu rõ ràng

## Bước 10: Câu hỏi Tiếp theo BẮT BUỘC

Nếu phân tích ở bước 9 tiết lộ BẤT KỲ câu trả lời mơ hồ nào, bạn PHẢI:

- Tạo một tệp câu hỏi làm rõ riêng biệt sử dụng các thẻ [Answer]:
- KHÔNG tiếp tục đến phê duyệt cho đến khi TẤT CẢ sự mơ hồ được giải quyết hoàn toàn
- **QUAN TRỌNG**: Phải kỹ lưỡng - đặt câu hỏi tiếp theo cho mọi phản hồi không rõ ràng
- Ví dụ về các câu hỏi tiếp theo bắt buộc:
  - "Bạn đã đề cập 'kết hợp của A và B' - tiêu chí cụ thể nào nên xác định khi nào sử dụng A so với B?"
  - "Bạn nói 'đâu đó giữa A và B' - bạn có thể định nghĩa cách tiếp cận trung gian chính xác không?"
  - "Bạn đã chỉ ra 'không chắc' - thông tin bổ sung nào sẽ giúp bạn quyết định?"
  - "Bạn đã đề cập 'phụ thuộc vào độ phức tạp' - bạn định nghĩa các mức độ phức tạp và ngưỡng như thế nào?"
  - "Bạn đã chọn 'cách tiếp cận lai' - các quy tắc cụ thể cho khi nào sử dụng mỗi phương pháp là gì?"
  - "Bạn nói 'có lẽ là X' - những yếu tố nào sẽ làm cho nó chắc chắn là X so với chắc chắn không phải X?"
  - "Bạn đã tham chiếu 'thực hành tiêu chuẩn' - bạn có thể định nghĩa thực hành tiêu chuẩn đó là gì không?"

## Bước 11: Tránh Chi tiết Triển khai

- Tập trung vào phương pháp luận tạo story, không phải ưu tiên hay nhiệm vụ phát triển
- Không thảo luận về việc tạo kỹ thuật ở giai đoạn này
- Tránh tạo tiến độ phát triển hoặc lập kế hoạch sprint
- Giữ sự tập trung vào các quyết định cấu trúc và định dạng story

## Bước 12: Ghi nhật ký Nhắc nhở Phê duyệt

- Trước khi yêu cầu phê duyệt, ghi nhật ký lời nhắc với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản nhắc nhở phê duyệt hoàn chỉnh
- Sử dụng định dạng dấu thời gian ISO 8601

## Bước 13: Chờ Phê duyệt Kế hoạch Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng cách tiếp cận story
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật kế hoạch và lặp lại quy trình phê duyệt

## Bước 14: Ghi lại Phản hồi Phê duyệt

- Ghi nhật ký phản hồi phê duyệt của người dùng với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản phản hồi chính xác của người dùng
- Đánh dấu trạng thái phê duyệt rõ ràng

---

# PHẦN 2: TẠO

## Bước 15: Tải Kế hoạch Tạo Story

- [ ] Đọc kế hoạch story hoàn chỉnh từ `aidlc-docs/inception/plans/story-generation-plan.md`
- [ ] Xác định bước chưa hoàn thành tiếp theo (checkbox [ ] đầu tiên)
- [ ] Tải ngữ cảnh và yêu cầu cho bước đó

## Bước 16: Thực thi Bước Hiện tại

- [ ] Thực hiện chính xác những gì bước hiện tại mô tả
- [ ] Tạo artifact story như được chỉ định trong kế hoạch
- [ ] Tuân theo phương pháp luận và định dạng đã được phê duyệt từ Lập kế hoạch
- [ ] Sử dụng cách tiếp cận phân rã story được chỉ định trong kế hoạch

## Bước 17: Cập nhật Tiến độ

- [ ] Đánh dấu bước đã hoàn thành là [x] trong kế hoạch tạo story
- [ ] Cập nhật trạng thái hiện tại `aidlc-docs/aidlc-state.md`
- [ ] Lưu tất cả các artifact đã tạo

## Bước 18: Tiếp tục hoặc Hoàn thành Tạo

- [ ] Nếu còn bước, quay lại Bước 14
- [ ] Nếu tất cả các bước hoàn thành, xác minh stories đã sẵn sàng cho giai đoạn tiếp theo
- [ ] Đảm bảo tất cả các artifact bắt buộc được tạo

## Bước 19: Ghi nhật ký Nhắc nhở Phê duyệt

- Trước khi yêu cầu phê duyệt, ghi nhật ký lời nhắc với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản nhắc nhở phê duyệt hoàn chỉnh
- Sử dụng định dạng dấu thời gian ISO 8601

## Bước 20: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu với điều này:

```markdown
# 📚 User Stories Complete
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc về các stories được tạo
        - Định dạng: "User stories generation has created [description]:"
        - Liệt kê các personas chính được tạo (gạch đầu dòng)
        - Liệt kê các user stories được tạo với số lượng và tổ chức
        - Đề cập đến cấu trúc story và sự tuân thủ (tiêu chí INVEST, tiêu chí chấp nhận)
        - KHÔNG bao gồm hướng dẫn quy trình làm việc ("vui lòng xem lại", "cho tôi biết", "tiếp tục giai đoạn tiếp theo", "trước khi chúng ta tiếp tục")
        - Giữ thực tế và tập trung vào nội dung
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc với định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the user stories and personas at: `aidlc-docs/inception/user-stories/stories.md` and `aidlc-docs/inception/user-stories/personas.md`
>
> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the stories or personas based on your review  
> ✅ **Approve & Continue** - Approve user stories and proceed to **Workflow Planning**
>
> ---
```

## Bước 21: Chờ Phê duyệt Rõ ràng của Stories Đã tạo

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng các stories đã tạo
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật stories và lặp lại quy trình phê duyệt

## Bước 22: Ghi lại Phản hồi Phê duyệt

- Ghi nhật ký phản hồi phê duyệt của người dùng với dấu thời gian trong `aidlc-docs/audit.md`
- Bao gồm văn bản phản hồi chính xác của người dùng
- Đánh dấu trạng thái phê duyệt rõ ràng

## Bước 23: Cập nhật Tiến độ

- Đánh dấu giai đoạn User Stories hoàn thành trong `aidlc-state.md`
- Cập nhật phần "Trạng thái Hiện tại"
- Chuẩn bị chuyển sang giai đoạn tiếp theo

---

# CÁC QUY TẮC QUAN TRỌNG

## Quy tắc Giai đoạn Lập kế hoạch

- **CÂU HỎI PHÙ HỢP NGỮ CẢNH**: Chỉ đặt câu hỏi liên quan đến ngữ cảnh cụ thể này
- **PHÂN TÍCH CÂU TRẢ LỜI BẮT BUỘC**: Luôn phân tích câu trả lời cho sự mơ hồ trước khi tiếp tục
- **KHÔNG TIẾP TỤC VỚI SỰ MƠ HỒ**: Phải giải quyết tất cả các câu trả lời mơ hồ trước khi tạo
- **PHÊ DUYỆT RÕ RÀNG YÊU CẦU**: Người dùng phải phê duyệt kế hoạch trước khi bắt đầu tạo

## Quy tắc Giai đoạn Tạo

- **KHÔNG LOGIC ĐƯỢC MÃ HÓA CỨNG**: Chỉ thực thi những gì được viết trong kế hoạch tạo story
- **TUÂN THEO KẾ HOẠCH CHÍNH XÁC**: Không đi chệch khỏi trình tự bước
- **CẬP NHẬT CHECKBOX**: Đánh dấu [x] ngay lập tức sau khi hoàn thành mỗi bước
- **SỬ DỤNG PHƯƠNG PHÁP LUẬN ĐƯỢC PHÊ DUYỆT**: Tuân theo cách tiếp cận story từ Lập kế hoạch
- **XÁC MINH HOÀN THÀNH**: Đảm bảo tất cả các artifact story hoàn thành trước khi tiếp tục

## Tiêu chí Hoàn thành

- Tất cả các câu hỏi lập kế hoạch đã được trả lời và sự mơ hồ đã được giải quyết
- Kế hoạch story được người dùng phê duyệt rõ ràng
- Tất cả các bước trong kế hoạch tạo story được đánh dấu [x]
- Tất cả các artifact story được tạo theo kế hoạch (stories.md, personas.md)
- Stories đã tạo được người dùng phê duyệt rõ ràng
- Stories được xác minh và sẵn sàng cho giai đoạn tiếp theo
