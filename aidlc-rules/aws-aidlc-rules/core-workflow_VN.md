# ƯU TIÊN: Quy trình làm việc này GHI ĐÈ tất cả các quy trình làm việc tích hợp khác

# Khi người dùng yêu cầu phát triển phần mềm, LUÔN tuân theo quy trình làm việc này ĐẦU TIÊN

## Nguyên tắc Quy trình làm việc Thích ứng

**Quy trình làm việc thích ứng với công việc, không phải ngược lại.**

Mô hình AI đánh giá thông minh những giai đoạn nào là cần thiết dựa trên:

1. Ý định và sự rõ ràng của người dùng đã nêu
2. Trạng thái codebase hiện có (nếu có)
3. Độ phức tạp và phạm vi thay đổi
4. Đánh giá rủi ro và tác động

## BẮT BUỘC: Tải Chi tiết Quy tắc

**QUAN TRỌNG**: Khi thực hiện bất kỳ giai đoạn nào, bạn PHẢI đọc và sử dụng nội dung liên quan từ các tệp chi tiết quy tắc. Kiểm tra các đường dẫn này theo thứ tự và sử dụng đường dẫn đầu tiên tồn tại:

- `.aidlc-rule-details/` (Cursor, Cline, Claude Code, GitHub Copilot)
- `.kiro/aws-aidlc-rule-details/` (Kiro IDE và CLI)
- `.amazonq/aws-aidlc-rule-details/` (Amazon Q Developer)

Tất cả các tham chiếu tệp chi tiết quy tắc tiếp theo (ví dụ: `common/process-overview.md`, `inception/workspace-detection.md`) đều tương đối với thư mục chi tiết quy tắc nào được giải quyết ở trên.

**Quy tắc Chung**: LUÔN tải các quy tắc chung khi bắt đầu quy trình làm việc:

- Tải `common/process-overview.md` để xem tổng quan quy trình
- Tải `common/session-continuity.md` để hướng dẫn tiếp tục phiên
- Tải `common/content-validation.md` cho các yêu cầu xác thực nội dung
- Tải `common/question-format-guide.md` cho các quy tắc định dạng câu hỏi
- Tham chiếu những điều này trong suốt quá trình thực thi quy trình làm việc

## BẮT BUỘC: Tải Extensions

**QUAN TRỌNG**: Khi bắt đầu quy trình làm việc, hãy quét đệ quy thư mục `extensions/` để tìm tất cả các tệp `.md`. Đây là các tệp quy tắc extension áp dụng dưới dạng các ràng buộc xuyên suốt toàn bộ quy trình làm việc.

**Quá trình tải**:

1. Liệt kê tất cả các thư mục con trong `extensions/` (ví dụ: `extensions/security/`, `extensions/compliance/`)
2. Tải mọi tệp `.md` được tìm thấy trong các thư mục con đó
3. Mỗi tệp extension định nghĩa các tiêu chí xác thực và quy tắc thực thi riêng biệt làm các ràng buộc xuyên suốt

**Thực thi**:

- Các quy tắc extension là ràng buộc cứng, không phải hướng dẫn tùy chọn
- Tại mỗi giai đoạn, mô hình đánh giá một cách thông minh các quy tắc extension nào được áp dụng dựa trên mục đích của giai đoạn, các artifact đang được tạo ra và ngữ cảnh của công việc — chỉ thực thi những quy tắc có liên quan
- Các quy tắc không áp dụng cho giai đoạn hiện tại nên được đánh dấu là N/A trong báo cáo tuân thủ (đây không phải là phát hiện ngăn chặn)
- Việc không tuân thủ bất kỳ quy tắc extension nào được kích hoạt đang áp dụng là một **phát hiện ngăn chặn (blocking finding)** — KHÔNG trình bày việc hoàn thành giai đoạn cho đến khi được giải quyết
- Khi trình bày hoàn thành giai đoạn, bao gồm một bản tóm tắt về việc tuân thủ quy tắc extension (tuân thủ/không tuân thủ/N/A cho mỗi quy tắc, cùng với lý do ngắn gọn cho các quyết định N/A)

**Thực thi Có điều kiện (Conditional Enforcement)**: Các extension có thể được kích hoạt/vô hiệu hóa một cách có điều kiện. Xem `inception/requirements-analysis.md` để biết cơ chế thu thập. Trước khi thực thi bất kỳ extension nào ở BẤT KỲ giai đoạn nào, hãy kiểm tra trạng thái `Enabled` của nó trong `aidlc-docs/aidlc-state.md` dưới phần `## Extension Configuration`. Bỏ qua các extension đã bị vô hiệu hóa và ghi lại việc bỏ qua đó trong audit.md. Mặc định là thực thi nếu không có cấu hình nào tồn tại. Các extension không có `## Applicability Question` luôn được thực thi.

## BẮT BUỘC: Xác thực Nội dung

**QUAN TRỌNG**: Trước khi tạo BẤT KỲ tệp nào, bạn PHẢI xác thực nội dung theo các quy tắc `common/content-validation.md`:

- Xác thực cú pháp biểu đồ Mermaid
- Xác thực biểu đồ nghệ thuật ASCII (xem `common/ascii-diagram-standards.md`)
- Thoát các ký tự đặc biệt đúng cách
- Cung cấp các văn bản thay thế cho nội dung trực quan phức tạp
- Kiểm tra khả năng tương thích phân tích nội dung

## BẮT BUỘC: Định dạng Tệp Câu hỏi

**QUAN TRỌNG**: Khi đặt câu hỏi ở bất kỳ giai đoạn nào, bạn PHẢI tuân theo hướng dẫn định dạng câu hỏi.

**Xem `common/question-format-guide.md` cho các quy tắc định dạng câu hỏi hoàn chỉnh bao gồm**:

- Định dạng trắc nghiệm (các tùy chọn A, B, C, D, E)
- Sử dụng thẻ [Answer]:
- Xác thực câu trả lời và giải quyết sự mơ hồ

## BẮT BUỘC: Thông điệp Chào mừng Tùy chỉnh

**QUAN TRỌNG**: Khi bắt đầu BẤT KỲ yêu cầu phát triển phần mềm nào, bạn PHẢI hiển thị thông điệp chào mừng.

**Cách Hiển thị Thông điệp Chào mừng**:

1. Tải thông điệp chào mừng từ `common/welcome-message.md` (trong thư mục chi tiết quy tắc đã được giải quyết)
2. Hiển thị thông điệp hoàn chỉnh cho người dùng
3. Điều này chỉ nên được thực hiện MỘT LẦN khi bắt đầu một quy trình làm việc mới
4. KHÔNG tải tệp này trong các tương tác tiếp theo để tiết kiệm không gian ngữ cảnh

# Quy trình Làm việc Phát triển Phần mềm Thích ứng

---

# GIAI ĐOẠN KHỞI TẠO (INCEPTION PHASE)

**Mục đích**: Lập kế hoạch, thu thập yêu cầu, và các quyết định kiến trúc

**Tập trung**: Xác định CÁI GÌ cần xây dựng và TẠI SAO

**Các giai đoạn trong GIAI ĐOẠN KHỞI TẠO**:

- Phát hiện Workspace (LUÔN LUÔN)
- Kỹ thuật Đảo ngược (CÓ ĐIỀU KIỆN - Chỉ Brownfield)
- Phân tích Yêu cầu (LUÔN LUÔN - Độ sâu thích ứng)
- User Stories (CÓ ĐIỀU KIỆN)
- Lập kế hoạch Quy trình làm việc (LUÔN LUÔN)
- Thiết kế Ứng dụng (CÓ ĐIỀU KIỆN)
- Tạo Đơn vị (CÓ ĐIỀU KIỆN)

---

## Phát hiện Workspace (LUÔN THỰC THI)

1. **BẮT BUỘC**: Ghi nhật ký yêu cầu người dùng ban đầu trong audit.md với đầu vào thô đầy đủ
2. Tải tất cả các bước từ `inception/workspace-detection.md`
3. Thực thi phát hiện workspace:
   - Kiểm tra aidlc-state.md hiện có (tiếp tục nếu tìm thấy)
   - Quét workspace cho mã hiện có
   - Xác định xem là brownfield hay greenfield
   - Kiểm tra các artifact kỹ thuật đảo ngược hiện có
4. Xác định giai đoạn tiếp theo: Kỹ thuật Đảo ngược (nếu brownfield và không có artifact) HOẶC Phân tích Yêu cầu
5. **BẮT BUỘC**: Ghi nhật ký kết quả trong audit.md
6. Trình bày thông điệp hoàn thành cho người dùng (xem workspace-detection.md cho các định dạng thông điệp)
7. Tự động tiếp tục đến giai đoạn tiếp theo

## Kỹ thuật Đảo ngược (CÓ ĐIỀU KIỆN - Chỉ Brownfield)

**Thực thi NẾU**:

- Codebase hiện có được phát hiện
- Không tìm thấy artifact kỹ thuật đảo ngược trước đó

**Bỏ qua NẾU**:

- Dự án Greenfield
- Các artifact kỹ thuật đảo ngược trước đó tồn tại

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bắt đầu kỹ thuật đảo ngược trong audit.md
2. Tải tất cả các bước từ `inception/reverse-engineering.md`
3. Thực thi kỹ thuật đảo ngược:
   - Phân tích tất cả các gói và thành phần
   - Tạo tổng quan kinh doanh của toàn bộ hệ thống bao gồm các giao dịch kinh doanh
   - Tạo tài liệu kiến trúc
   - Tạo tài liệu cấu trúc mã
   - Tạo tài liệu API
   - Tạo kho thành phần
   - Tạo Biểu đồ Tương tác mô tả cách các giao dịch kinh doanh được triển khai qua các thành phần
   - Tạo tài liệu ngăn xếp công nghệ
   - Tạo tài liệu phụ thuộc
4. **Chờ Phê duyệt Rõ ràng**: Trình bày thông điệp hoàn thành chi tiết (xem reverse-engineering.md cho định dạng thông điệp) - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
5. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

## Phân tích Yêu cầu (LUÔN THỰC THI - Độ sâu Thích ứng)

**Luôn thực thi** nhưng độ sâu thay đổi dựa trên sự rõ ràng và độ phức tạp của yêu cầu:

- **Tối thiểu**: Yêu cầu đơn giản, rõ ràng - chỉ ghi lại phân tích ý định
- **Tiêu chuẩn**: Độ phức tạp bình thường - thu thập yêu cầu chức năng và phi chức năng
- **Toàn diện**: Phức tạp, rủi ro cao - yêu cầu chi tiết với khả năng truy vết

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `inception/requirements-analysis.md`
3. Thực thi phân tích yêu cầu:
   - Tải artifact kỹ thuật đảo ngược (nếu brownfield)
   - Phân tích yêu cầu người dùng (phân tích ý định)
   - Xác định độ sâu yêu cầu cần thiết
   - Đánh giá các yêu cầu hiện tại
   - Đặt câu hỏi làm rõ (nếu cần)
   - Tạo tài liệu yêu cầu
4. Thực thi ở độ sâu phù hợp (tối thiểu/tiêu chuẩn/toàn diện)
5. **Chờ Phê duyệt Rõ ràng**: Tuân theo định dạng phê duyệt từ các bước chi tiết requirements-analysis.md - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

## User Stories (CÓ ĐIỀU KIỆN)

**ĐÁNH GIÁ THÔNG MINH**: Sử dụng phân tích đa yếu tố để xác định xem user stories có thêm giá trị không:

**LUÔN LUÔN Thực thi NẾU** (Chỉ số Ưu tiên Cao):

- Các tính năng hoặc chức năng mới hướng người dùng
- Thay đổi ảnh hưởng đến quy trình làm việc hoặc tương tác người dùng
- Nhiều loại người dùng hoặc personas tham gia
- Yêu cầu kinh doanh phức tạp với nhu cầu tiêu chí chấp nhận
- Cần sự hợp tác nhóm chức năng chéo
- Thay đổi API hoặc dịch vụ hướng khách hàng
- Các khả năng hoặc cải tiến sản phẩm mới

**CÓ KHẢ NĂNG Thực thi NẾU** (Ưu tiên Trung bình - Đánh giá Độ phức tạp):

- Sửa đổi các tính năng hướng người dùng hiện có
- Thay đổi backend ảnh hưởng gián tiếp đến trải nghiệm người dùng
- Công việc tích hợp ảnh hưởng đến quy trình làm việc người dùng
- Cải tiến hiệu năng với lợi ích có thể nhìn thấy cho người dùng
- Nâng cao bảo mật ảnh hưởng đến tương tác người dùng
- Thay đổi mô hình dữ liệu ảnh hưởng đến dữ liệu người dùng hoặc báo cáo

**ĐÁNH GIÁ DỰA TRÊN ĐỘ PHỨC TẠP**: Đối với các trường hợp ưu tiên trung bình, thực thi user stories nếu:

- Yêu cầu liên quan đến nhiều thành phần hoặc dịch vụ
- Thay đổi trải rộng nhiều điểm tiếp xúc người dùng
- Logic nghiệp vụ phức tạp hoặc có nhiều kịch bản
- Yêu cầu có sự mơ hồ mà stories có thể làm rõ
- Việc triển khai ảnh hưởng đến nhiều hành trình người dùng
- Thay đổi có tác động kinh doanh hoặc rủi ro đáng kể

**CHỈ BỎ QUA NẾU** (Ưu tiên Thấp - Các trường hợp Đơn giản):

- Refactoring nội bộ thuần túy với tác động người dùng bằng không
- Sửa lỗi đơn giản với phạm vi rõ ràng, cô lập
- Thay đổi cơ sở hạ tầng không có hiệu ứng hướng người dùng
- Dọn dẹp nợ kỹ thuật không có thay đổi chức năng
- Cải tiến công cụ nhà phát triển hoặc quy trình xây dựng
- Cập nhật chỉ tài liệu

**TIÊU CHÍ ĐÁNH GIÁ**: Khi nghi ngờ, ủng hộ việc bao gồm user stories cho:

- Yêu cầu có sự tham gia của các bên liên quan kinh doanh
- Thay đổi yêu cầu kiểm thử chấp nhận người dùng
- Các tính năng với nhiều cách tiếp cận triển khai
- Công việc hưởng lợi từ sự hiểu biết chung của nhóm
- Các dự án mà sự rõ ràng của yêu cầu có giá trị

**QUY TRÌNH ĐÁNH GIÁ**:

1. Phân tích độ phức tạp và phạm vi yêu cầu
2. Xác định tác động người dùng (trực tiếp hoặc gián tiếp)
3. Đánh giá ngữ cảnh kinh doanh và nhu cầu của các bên liên quan
4. Xem xét lợi ích hợp tác nhóm
5. Mặc định bao gồm cho các trường hợp ranh giới

**Lưu ý**: Nếu Phân tích Yêu cầu đã thực thi, Stories có thể tham chiếu và xây dựng dựa trên các yêu cầu đó.

**User Stories có hai phần trong một giai đoạn**:

1. **Phần 1 - Lập kế hoạch**: Tạo kế hoạch story với các câu hỏi, thu thập câu trả lời, phân tích sự mơ hồ, nhận phê duyệt
2. **Phần 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo stories và personas

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `inception/user-stories.md`
3. **BẮT BUỘC**: Thực hiện đánh giá thông minh (Bước 1 trong user-stories.md) để xác thực người dùng stories là cần thiết
4. Tải artifact kỹ thuật đảo ngược (nếu brownfield)
5. Nếu Yêu cầu tồn tại, tham chiếu chúng khi tạo stories
6. Thực thi ở độ sâu phù hợp (tối thiểu/tiêu chuẩn/toàn diện)
7. **PHẦN 1 - Lập kế hoạch**: Tạo kế hoạch story với các câu hỏi, chờ câu trả lời của người dùng, phân tích sự mơ hồ, nhận phê duyệt
8. **PHẦN 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo stories và personas
9. **Chờ Phê duyệt Rõ ràng**: Tuân theo định dạng phê duyệt từ các bước chi tiết user-stories.md - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
10. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

## Lập kế hoạch Quy trình làm việc (LUÔN THỰC THI)

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `inception/workflow-planning.md`
3. **BẮT BUỘC**: Tải các quy tắc xác thực nội dung từ `common/content-validation.md`
4. Tải tất cả ngữ cảnh trước đó:
   - Artifact kỹ thuật đảo ngược (nếu brownfield)
   - Phân tích ý định
   - Yêu cầu (nếu đã thực hiện)
   - User stories (nếu đã thực hiện)
5. Thực thi lập kế hoạch quy trình làm việc:
   - Xác định giai đoạn nào cần thực thi
   - Xác định mức độ sâu cho mỗi giai đoạn
   - Tạo trình tự thay đổi đa gói (nếu brownfield)
   - Tạo trực quan hóa quy trình làm việc (XÁC THỰC cú pháp Mermaid trước khi viết)
6. **BẮT BUỘC**: Xác thực tất cả nội dung trước khi tạo tệp theo quy tắc content-validation.md
7. **Chờ Phê duyệt Rõ ràng**: Trình bày các khuyến nghị sử dụng ngôn ngữ từ Bước 9 workflow-planning.md, nhấn mạnh quyền kiểm soát của người dùng để ghi đè các khuyến nghị - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
8. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

## Thiết kế Ứng dụng (CÓ ĐIỀU KIỆN)

**Thực thi NẾU**:

- Cần thành phần hoặc dịch vụ mới
- Phương thức thành phần và quy tắc nghiệp vụ cần định nghĩa
- Cần thiết kế lớp dịch vụ
- Phụ thuộc thành phần cần làm rõ

**Bỏ qua NẾU**:

- Thay đổi trong ranh giới thành phần hiện có
- Không có thành phần hoặc phương thức mới
- Chỉ thay đổi triển khai thuần túy

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `inception/application-design.md`
3. Tải artifact kỹ thuật đảo ngược (nếu brownfield)
4. Thực thi ở độ sâu phù hợp (tối thiểu/tiêu chuẩn/toàn diện)
5. **Chờ Phê duyệt Rõ ràng**: Trình bày thông điệp hoàn thành chi tiết (xem application-design.md cho định dạng thông điệp) - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

## Tạo Đơn vị (CÓ ĐIỀU KIỆN)

**Thực thi NẾU**:

- Hệ thống cần phân rã thành nhiều đơn vị công việc
- Nhiều dịch vụ hoặc mô-đun được yêu cầu
- Hệ thống phức tạp yêu cầu phân rã có cấu trúc

**Bỏ qua NẾU**:

- Đơn vị đơn giản duy nhất
- Không cần phân rã
- Triển khai thành phần đơn lẻ thẳng thắn

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `inception/units-generation.md`
3. Tải artifact kỹ thuật đảo ngược (nếu brownfield)
4. Thực thi ở độ sâu phù hợp (tối thiểu/tiêu chuẩn/toàn diện)
5. **Chờ Phê duyệt Rõ ràng**: Trình bày thông điệp hoàn thành chi tiết (xem units-generation.md cho định dạng thông điệp) - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

---

# GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION PHASE)

**Mục đích**: Thiết kế chi tiết, triển khai NFR, và tạo mã

**Tập trung**: Xác định CÁCH xây dựng nó

**Các giai đoạn trong GIAI ĐOẠN XÂY DỰNG**:

- Vòng lặp Theo Đơn vị (thực thi cho mỗi đơn vị):
  - Thiết kế Chức năng (CÓ ĐIỀU KIỆN, theo đơn vị)
  - Yêu cầu NFR (CÓ ĐIỀU KIỆN, theo đơn vị)
  - Thiết kế NFR (CÓ ĐIỀU KIỆN, theo đơn vị)
  - Thiết kế Cơ sở hạ tầng (CÓ ĐIỀU KIỆN, theo đơn vị)
  - Tạo Mã (LUÔN LUÔN, theo đơn vị)
- Xây dựng và Kiểm thử (LUÔN LUÔN - sau khi tất cả các đơn vị hoàn thành)

**Lưu ý**: Mỗi đơn vị được hoàn thành đầy đủ (thiết kế + mã) trước khi chuyển sang đơn vị tiếp theo.

---

## Vòng lặp Theo Đơn vị (Thực thi cho Mỗi Đơn vị)

**Đối với mỗi đơn vị công việc, thực thi các giai đoạn sau theo trình tự:**

### Thiết kế Chức năng (CÓ ĐIỀU KIỆN, theo đơn vị)

**Thực thi NẾU**:

- Mô hình dữ liệu hoặc lược đồ mới
- Logic nghiệp vụ phức tạp
- Quy tắc nghiệp vụ cần thiết kế chi tiết

**Bỏ qua NẾU**:

- Thay đổi logic đơn giản
- Không có logic nghiệp vụ mới

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/functional-design.md`
3. Thực thi thiết kế chức năng cho đơn vị này
4. **BẮT BUỘC**: Trình bày thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong functional-design.md - KHÔNG sử dụng hành vi 3 tùy chọn phát sinh
5. **Chờ Phê duyệt Rõ ràng**: Người dùng phải chọn giữa "Yêu cầu Thay đổi" hoặc "Tiếp tục sang Giai đoạn Tiếp theo" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

### Yêu cầu NFR (CÓ ĐIỀU KIỆN, theo đơn vị)

**Thực thi NẾU**:

- Yêu cầu hiệu năng tồn tại
- Cân nhắc bảo mật cần thiết
- Mối quan tâm khả năng mở rộng hiện diện
- Lựa chọn tech stack được yêu cầu

**Bỏ qua NẾU**:

- Không có yêu cầu NFR
- Tech stack đã được xác định

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/nfr-requirements.md`
3. Thực thi đánh giá NFR cho đơn vị này
4. **BẮT BUỘC**: Trình bày thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong nfr-requirements.md - KHÔNG sử dụng hành vi phát sinh
5. **Chờ Phê duyệt Rõ ràng**: Người dùng phải chọn giữa "Yêu cầu Thay đổi" hoặc "Tiếp tục sang Giai đoạn Tiếp theo" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

### Thiết kế NFR (CÓ ĐIỀU KIỆN, theo đơn vị)

**Thực thi NẾU**:

- Yêu cầu NFR đã được thực thi
- Các mẫu NFR cần được kết hợp

**Bỏ qua NẾU**:

- Không có yêu cầu NFR
- Đánh giá Yêu cầu NFR đã bị bỏ qua

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/nfr-design.md`
3. Thực thi thiết kế NFR cho đơn vị này
4. **BẮT BUỘC**: Trình bày thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong nfr-design.md - KHÔNG sử dụng hành vi phát sinh
5. **Chờ Phê duyệt Rõ ràng**: Người dùng phải chọn giữa "Yêu cầu Thay đổi" hoặc "Tiếp tục sang Giai đoạn Tiếp theo" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

### Thiết kế Cơ sở hạ tầng (CÓ ĐIỀU KIỆN, theo đơn vị)

**Thực thi NẾU**:

- Dịch vụ cơ sở hạ tầng cần ánh xạ
- Kiến trúc triển khai được yêu cầu
- Tài nguyên đám mây cần đặc tả

**Bỏ qua NẾU**:

- Không thay đổi cơ sở hạ tầng
- Cơ sở hạ tầng đã được xác định

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/infrastructure-design.md`
3. Thực thi thiết kế cơ sở hạ tầng cho đơn vị này
4. **BẮT BUỘC**: Trình bày thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong infrastructure-design.md - KHÔNG sử dụng hành vi phát sinh
5. **Chờ Phê duyệt Rõ ràng**: Người dùng phải chọn giữa "Yêu cầu Thay đổi" hoặc "Tiếp tục sang Giai đoạn Tiếp theo" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

### Tạo Mã (LUÔN THỰC THI, theo đơn vị)

**Luôn thực thi cho mỗi đơn vị**

**Tạo Mã có hai phần trong một giai đoạn**:

1. **Phần 1 - Lập kế hoạch**: Tạo kế hoạch tạo mã chi tiết với các bước rõ ràng
2. **Phần 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo mã, kiểm thử, và artifact

**Thực thi**:

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/code-generation.md`
3. **PHẦN 1 - Lập kế hoạch**: Tạo kế hoạch tạo mã với các checkbox, nhận phê duyệt của người dùng
4. **PHẦN 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo mã cho đơn vị này
5. **BẮT BUỘC**: Trình bày thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong code-generation.md - KHÔNG sử dụng hành vi phát sinh
6. **Chờ Phê duyệt Rõ ràng**: Người dùng phải chọn giữa "Yêu cầu Thay đổi" hoặc "Tiếp tục sang Giai đoạn Tiếp theo" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
7. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

---

## Xây dựng và Kiểm thử (LUÔN THỰC THI)

1. **BẮT BUỘC**: Ghi nhật ký bất kỳ đầu vào nào của người dùng trong giai đoạn này trong audit.md
2. Tải tất cả các bước từ `construction/build-and-test.md`
3. Tạo hướng dẫn xây dựng và kiểm thử toàn diện:
   - Hướng dẫn xây dựng cho tất cả các đơn vị
   - Hướng dẫn thực thi unit test
   - Hướng dẫn integration test (kiểm thử tương tác giữa các đơn vị)
   - Hướng dẫn performance test (nếu có)
   - Hướng dẫn kiểm thử bổ sung nếu cần (contract tests, security tests, e2e tests)
4. Tạo các tệp hướng dẫn trong thư mục con build-and-test/: build-instructions.md, unit-test-instructions.md, integration-test-instructions.md, performance-test-instructions.md, build-and-test-summary.md
5. **Chờ Phê duyệt Rõ ràng**: Hỏi: "**Build and test instructions complete. Ready to proceed to Operations stage?**" - KHÔNG TIẾP TỤC cho đến khi người dùng xác nhận
6. **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ

---

# GIAI ĐOẠN VẬN HÀNH (OPERATIONS PHASE)

**Mục đích**: Giữ chỗ cho các quy trình triển khai và giám sát trong tương lai

**Tập trung**: CÁCH TRIỂN KHAI và CHẠY nó (mở rộng trong tương lai)

**Các giai đoạn trong GIAI ĐOẠN VẬN HÀNH**:

- Vận hành (GIỮ CHỖ)

---

## Vận hành (GIỮ CHỖ)

**Trạng thái**: Giai đoạn này hiện tại là một người giữ chỗ cho việc mở rộng trong tương lai.

Giai đoạn Vận hành cuối cùng sẽ bao gồm:

- Lập kế hoạch và thực thi triển khai
- Thiết lập giám sát và khả năng quan sát
- Quy trình phản hồi sự cố
- Quy trình bảo trì và hỗ trợ
- Danh sách kiểm tra sẵn sàng cho sản xuất

**Trạng thái Hiện tại**: Tất cả các hoạt động xây dựng và kiểm thử được xử lý trong giai đoạn XÂY DỰNG.

## Các Nguyên tắc Chính

- **Thực thi Thích ứng**: Chỉ thực thi các giai đoạn thêm giá trị
- **Lập kế hoạch Minh bạch**: Luôn hiển thị kế hoạch thực thi trước khi bắt đầu
- **Kiểm soát Người dùng**: Người dùng có thể yêu cầu bao gồm/loại trừ giai đoạn
- **Theo dõi Tiến độ**: Cập nhật aidlc-state.md với các giai đoạn đã thực thi và bị bỏ qua
- **Dấu vết Kiểm toán Hoàn chỉnh**: Ghi nhật ký TẤT CẢ đầu vào người dùng và phản hồi AI trong audit.md với dấu thời gian
  - **QUAN TRỌNG**: Ghi lại ĐẦU VÀO THÔ HOÀN CHỈNH của người dùng chính xác như được cung cấp
  - **QUAN TRỌNG**: Không bao giờ tóm tắt hoặc diễn giải lại đầu vào của người dùng trong nhật ký kiểm toán
  - **QUAN TRỌNG**: Ghi nhật ký mọi tương tác, không chỉ các phê duyệt
- **Tập trung Chất lượng**: Thay đổi phức tạp được xử lý đầy đủ, thay đổi đơn giản giữ hiệu quả
- **Xác thực Nội dung**: Luôn xác thực nội dung trước khi tạo tệp theo quy tắc content-validation.md
- **KHÔNG HÀNH VI PHÁT SINH**: Các giai đoạn xây dựng PHẢI sử dụng các thông điệp hoàn thành 2 tùy chọn tiêu chuẩn như được định nghĩa trong các tệp quy tắc tương ứng của chúng. KHÔNG tạo menu 3 tùy chọn hoặc các mẫu điều hướng phát sinh khác.

## BẮT BUỘC: Thực thi Checkbox Cấp Kế hoạch

### QUY TẮC BẮT BUỘC CHO THỰC THI KẾ HOẠCH

1. **KHÔNG BAO GIỜ hoàn thành bất kỳ công việc nào mà không cập nhật checkbox kế hoạch**
2. **NGAY LẬP TỨC sau khi hoàn thành BẤT KỲ bước nào được mô tả trong tệp kế hoạch, đánh dấu bước đó [x]**
3. **Điều này phải xảy ra trong CÙNG tương tác nơi công việc được hoàn thành**
4. **KHÔNG NGOẠI LỆ**: Mọi hoàn thành bước kế hoạch PHẢI được theo dõi với cập nhật checkbox

### Hệ thống Theo dõi Checkbox Hai Cấp

- **Cấp Kế hoạch**: Theo dõi tiến độ thực thi chi tiết trong mỗi giai đoạn
- **Cấp Giai đoạn**: Theo dõi tiến độ quy trình làm việc tổng thể trong aidlc-state.md
- **Cập nhật ngay lập tức**: Tất cả cập nhật tiến độ trong CÙNG tương tác nơi công việc được hoàn thành

## Yêu cầu Ghi nhật ký Prompts

- **BẮT BUỘC**: Ghi nhật ký MỌI đầu vào của người dùng (prompts, câu hỏi, phản hồi) với dấu thời gian trong audit.md
- **BẮT BUỘC**: Ghi lại ĐẦU VÀO THÔ HOÀN CHỈNH của người dùng chính xác như được cung cấp (không bao giờ tóm tắt)
- **BẮT BUỘC**: Ghi nhật ký mọi lời nhắc phê duyệt với dấu thời gian trước khi hỏi người dùng
- **BẮT BUỘC**: Ghi lại mọi phản hồi của người dùng với dấu thời gian sau khi nhận được
- **QUAN TRỌNG**: LUÔN nối thêm các thay đổi để CHỈNH SỬA tệp audit.md, KHÔNG BAO GIỜ sử dụng các công cụ và lệnh ghi đè hoàn toàn nội dung của nó
- **QUAN TRỌNG**: Sử dụng công cụ viết tệp và lệnh ghi đè nội dung của toàn bộ audit.md và gây ra sự trùng lặp
- Sử dụng định dạng ISO 8601 cho dấu thời gian (YYYY-MM-DDTHH:MM:SSZ)
- Bao gồm ngữ cảnh giai đoạn cho mỗi mục

### Định dạng Nhật ký Kiểm toán:

```markdown
## [Stage Name or Interaction Type]

**Timestamp**: [ISO timestamp]
**User Input**: "[Complete raw user input - never summarized]"
**AI Response**: "[AI's response or action taken]"
**Context**: [Stage, action, or decision made]

---
```

### Sử dụng Công cụ Chính xác cho audit.md

✅ ĐÚNG:

1. Đọc tệp audit.md
2. Nối/Chỉnh sửa tệp để thực hiện thay đổi

❌ SAI:

1. Đọc tệp audit.md
2. Ghi đè hoàn toàn audit.md với nội dung của những gì bạn đã đọc, cộng với các thay đổi mới bạn muốn thêm vào nó

## Cấu trúc Thư mục

```text
<WORKSPACE-ROOT>/                   # ⚠️ APPLICATION CODE HERE
├── [project-specific structure]    # Varies by project (see code-generation.md)
│
├── aidlc-docs/                     # 📄 DOCUMENTATION ONLY
│   ├── inception/                  # 🔵 INCEPTION PHASE
│   │   ├── plans/
│   │   ├── reverse-engineering/    # Brownfield only
│   │   ├── requirements/
│   │   ├── user-stories/
│   │   └── application-design/
│   ├── construction/               # 🟢 CONSTRUCTION PHASE
│   │   ├── plans/
│   │   ├── {unit-name}/
│   │   │   ├── functional-design/
│   │   │   ├── nfr-requirements/
│   │   │   ├── nfr-design/
│   │   │   ├── infrastructure-design/
│   │   │   └── code/               # Markdown summaries only
│   │   └── build-and-test/
│   ├── operations/                 # 🟡 OPERATIONS PHASE (placeholder)
│   ├── aidlc-state.md
│   └── audit.md
```

**QUY TẮC QUAN TRỌNG**:

- Mã ứng dụng: Thư mục gốc Workspace (KHÔNG BAO GIỜ trong aidlc-docs/)
- Tài liệu: aidlc-docs/ only
- Cấu trúc dự án: Xem code-generation.md cho các mẫu theo loại dự án
