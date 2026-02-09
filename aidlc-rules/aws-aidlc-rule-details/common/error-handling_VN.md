# Xử lý Lỗi và Quy trình Khôi phục

## Nguyên tắc Xử lý Lỗi Chung

### Khi Lỗi Xảy ra

1. **Xác định lỗi**: Nêu rõ những gì đã sai
2. **Đánh giá tác động**: Xác định xem lỗi có chặn hay có thể khắc phục được không
3. **Giao tiếp**: Thông báo cho người dùng về lỗi và các tùy chọn
4. **Đưa ra giải pháp**: Cung cấp các bước rõ ràng để giải quyết hoặc khắc phục lỗi
5. **Tài liệu**: Ghi lại lỗi và giải pháp trong `audit.md`

### Các mức độ Nghiêm trọng của Lỗi

**Nghiêm trọng (Critical)**: Quy trình làm việc không thể tiếp tục

- Thiếu tệp hoặc artifact bắt buộc
- Đầu vào người dùng không hợp lệ không thể xử lý
- Lỗi hệ thống ngăn chặn thao tác tệp

**Cao (High)**: Giai đoạn không thể hoàn thành theo kế hoạch

- Câu trả lời không đầy đủ cho các câu hỏi bắt buộc
- Phản hồi của người dùng mâu thuẫn
- Thiếu các phụ thuộc từ các giai đoạn trước

**Trung bình (Medium)**: Giai đoạn có thể tiếp tục với các giải pháp thay thế

- Thiếu artifact tùy chọn
- Thất bại xác thực không nghiêm trọng
- Có thể hoàn thành một phần

**Thấp (Low)**: Các vấn đề nhỏ không chặn tiến trình

- Không nhất quán về định dạng
- Thiếu thông tin tùy chọn
- Cảnh báo không chặn

## Xử lý Lỗi Cụ thể theo Giai đoạn

### Lỗi Đánh giá Ngữ cảnh

**Lỗi**: Không thể đọc các tệp workspace

- **Nguyên nhân**: Vấn đề về quyền, thiếu thư mục
- **Giải pháp**: Yêu cầu người dùng xác minh đường dẫn workspace và quyền
- **Giải pháp thay thế**: Tiếp tục chỉ với thông tin do người dùng cung cấp

**Lỗi**: `aidlc-state.md` hiện có bị hỏng

- **Nguyên nhân**: Chỉnh sửa thủ công, lần chạy trước chưa hoàn tất
- **Giải pháp**: Hỏi người dùng xem họ muốn bắt đầu mới hay thử khôi phục
- **Khôi phục**: Tạo bản sao lưu, bắt đầu tệp trạng thái mới

**Lỗi**: Không thể xác định các giai đoạn cần thiết

- **Nguyên nhân**: Không đủ thông tin từ người dùng
- **Giải pháp**: Hỏi các câu hỏi làm rõ về ý định và phạm vi
- **Giải pháp thay thế**: Mặc định là kế hoạch thực hiện toàn diện

### Lỗi Đánh giá Yêu cầu

**Lỗi**: Người dùng cung cấp các yêu cầu mâu thuẫn

- **Nguyên nhân**: Hiểu không rõ, nhu cầu thay đổi
- **Giải pháp**: Tạo các câu hỏi tiếp theo để giải quyết mâu thuẫn
- **Không Tiếp tục**: Cho đến khi mâu thuẫn được giải quyết

**Lỗi**: Tài liệu yêu cầu không thể chuyển đổi

- **Nguyên nhân**: Định dạng không được hỗ trợ, tệp bị hỏng
- **Giải pháp**: Yêu cầu người dùng cung cấp yêu cầu ở định dạng được hỗ trợ
- **Giải pháp thay thế**: Làm việc với mô tả bằng lời của người dùng

**Lỗi**: Câu trả lời không đầy đủ cho các câu hỏi xác minh

- **Nguyên nhân**: Người dùng bỏ qua câu hỏi, không rõ cần trả lời gì
- **Giải pháp**: Làm nổi bật các câu hỏi chưa được trả lời, cung cấp ví dụ
- **Không Tiếp tục**: Cho đến khi tất cả các câu hỏi bắt buộc được trả lời

### Lỗi Phát triển Story

**Lỗi**: Không thể ánh xạ yêu cầu sang các story

- **Nguyên nhân**: Yêu cầu quá mơ hồ, thiếu chi tiết chức năng
- **Giải pháp**: Quay lại Đánh giá Yêu cầu để làm rõ
- **Giải pháp thay thế**: Tạo các story dựa trên thông tin có sẵn, đánh dấu là chưa hoàn thành

**Lỗi**: Người dùng cung cấp câu trả lời lập kế hoạch story mơ hồ

- **Nguyên nhân**: Các tùy chọn không rõ ràng, quyết định phức tạp
- **Giải pháp**: Thêm các câu hỏi tiếp theo với các ví dụ cụ thể
- **Không Tiếp tục**: Cho đến khi sự mơ hồ được giải quyết

**Lỗi**: Kế hoạch tạo story có các bước chưa hoàn thành

- **Nguyên nhân**: Thực thi bị gián đoạn, các bước bị bỏ qua
- **Giải pháp**: Tiếp tục từ bước chưa hoàn thành đầu tiên
- **Khôi phục**: Xem xét các bước đã hoàn thành, tiếp tục từ điểm kiểm tra (checkpoint)

### Lỗi Thiết kế Ứng dụng

**Lỗi**: Quyết định kiến trúc không rõ ràng hoặc mâu thuẫn

- **Nguyên nhân**: Câu trả lời mơ hồ, yêu cầu xung đột
- **Giải pháp**: Thêm các câu hỏi tiếp theo để làm rõ quyết định
- **Không Tiếp tục**: Cho đến khi quyết định rõ ràng và được ghi lại

**Lỗi**: Không thể xác định số lượng dịch vụ/đơn vị

- **Nguyên nhân**: Không đủ thông tin về ranh giới
- **Giải pháp**: Hỏi các câu hỏi cụ thể về triển khai, cấu trúc nhóm, quy mô
- **Giải pháp thay thế**: Mặc định là nguyên khối (monolith), cho phép thay đổi sau

### Lỗi Thiết kế

**Lỗi**: Các phụ thuộc đơn vị bị vòng tròn

- **Nguyên nhân**: Định nghĩa ranh giới kém, ghép nối chặt chẽ
- **Giải pháp**: Xác định các phụ thuộc vòng tròn, đề xuất tái cấu trúc (refactoring)
- **Khôi phục**: Sửa đổi ranh giới đơn vị để phá vỡ vòng lặp

**Lỗi**: Kế hoạch thiết kế đơn vị thiếu các bước

- **Nguyên nhân**: Tạo kế hoạch chưa hoàn tất, lỗi mẫu
- **Giải pháp**: Tạo lại kế hoạch với tất cả các bước cần thiết
- **Khôi phục**: Thêm các bước còn thiếu vào kế hoạch hiện có

**Lỗi**: Không thể tạo artifact thiết kế

- **Nguyên nhân**: Thiếu thông tin đơn vị, yêu cầu không rõ ràng
- **Giải pháp**: Quay lại Lập kế hoạch Đơn vị để làm rõ định nghĩa đơn vị
- **Giải pháp thay thế**: Tạo thiết kế một phần, đánh dấu các khoảng trống

### Lỗi Triển khai NFR

**Lỗi**: Các lựa chọn ngăn xếp công nghệ không tương thích

- **Nguyên nhân**: Yêu cầu xung quanh, hạn chế nền tảng
- **Giải pháp**: Làm nổi bật sự không tương thích, yêu cầu người dùng chọn
- **Không Tiếp tục**: Cho đến khi các lựa chọn tương thích được thực hiện

**Lỗi**: Các ràng buộc tổ chức không thể đáp ứng

- **Nguyên nhân**: Hạn chế mạng, chính sách bảo mật
- **Giải pháp**: Ghi lại các ràng buộc, yêu cầu người dùng cho giải pháp thay thế
- **Leo thang**: Có thể cần sự can thiệp của con người để thiết lập

**Lỗi**: Bước triển khai NFR yêu cầu hành động của con người

- **Nguyên nhân**: AI không thể thực hiện một số tác vụ nhất định (cấu hình mạng, thông tin xác thực)
- **Giải pháp**: Đánh dấu rõ ràng là **TÁC VỤ CON NGƯỜI**, cung cấp hướng dẫn
- **Chờ**: Người dùng xác nhận trước khi tiếp tục

### Lỗi Lập kế hoạch Mã

**Lỗi**: Kế hoạch tạo mã không đầy đủ

- **Nguyên nhân**: Thiếu artifact thiết kế, yêu cầu không rõ ràng
- **Giải pháp**: Quay lại giai đoạn Thiết kế để hoàn thành các artifact
- **Khôi phục**: Tạo kế hoạch với thông tin có sẵn, đánh dấu các khoảng trống

**Lỗi**: Phụ thuộc đơn vị không được thỏa mãn

- **Nguyên nhân**: Các đơn vị phụ thuộc chưa được tạo
- **Giải pháp**: Sắp xếp lại trình tự tạo để tôn trọng các phụ thuộc
- **Giải pháp thay thế**: Tạo với các phụ thuộc giả (stub), tích hợp sau

### Lỗi Tạo Mã

**Lỗi**: Không thể tạo mã cho một bước

- **Nguyên nhân**: Không đủ thông tin thiết kế, yêu cầu không rõ ràng
- **Giải pháp**: Bỏ qua bước, ghi lại là chưa hoàn thành, tiếp tục
- **Khôi phục**: Quay lại bước sau khi thu thập thêm thông tin

**Lỗi**: Mã được tạo có lỗi cú pháp

- **Nguyên nhân**: Vấn đề mẫu, vấn đề cụ thể của ngôn ngữ
- **Giải pháp**: Sửa lỗi cú pháp, tạo lại nếu cần
- **Xác thực**: Xác minh mã biên dịch được trước khi tiếp tục

**Lỗi**: Tạo kiểm thử thất bại

- **Nguyên nhân**: Logic phức tạp, thiếu thiết lập khung kiểm thử
- **Giải pháp**: Tạo cấu trúc kiểm thử cơ bản, đánh dấu để hoàn thành thủ công
- **Giải pháp thay thế**: Tiếp tục mà không có kiểm thử, thêm vào trong giai đoạn Vận hành

### Lỗi Vận hành

**Lỗi**: Không thể xác định công cụ xây dựng

- **Nguyên nhân**: Cấu trúc dự án bất thường, nhiều hệ thống xây dựng
- **Giải pháp**: Yêu cầu người dùng chỉ định công cụ xây dựng và các lệnh
- **Giải pháp thay thế**: Cung cấp hướng dẫn chung, người dùng tự điều chỉnh

**Lỗi**: Mục tiêu triển khai không rõ ràng

- **Nguyên nhân**: Nhiều môi trường, cơ sở hạ tầng phức tạp
- **Giải pháp**: Yêu cầu người dùng chỉ định mục tiêu triển khai và phương pháp
- **Giải pháp thay thế**: Cung cấp hướng dẫn cho các nền tảng phổ biến

## Quy trình Khôi phục

### Hoàn thành Giai đoạn Một phần

**Kịch bản**: Giai đoạn bị gián đoạn giữa chừng

**Các bước Khôi phục**:

1. Tải tệp kế hoạch giai đoạn
2. Xác định bước hoàn thành cuối cùng (checkbox [x] cuối cùng)
3. Tiếp tục từ bước chưa hoàn thành tiếp theo
4. Xác minh tất cả các bước trước đó thực sự đã hoàn thành
5. Tiếp tục thực thi bình thường

### Tệp Trạng thái Bị hỏng

**Kịch bản**: `aidlc-state.md` bị hỏng hoặc không nhất quán

**Các bước Khôi phục**:

1. Tạo bản sao lưu: `aidlc-state.md.backup`
2. Hỏi người dùng họ thực sự đang ở giai đoạn nào
3. Tạo lại tệp trạng thái từ đầu
4. Đánh dấu các giai đoạn đã hoàn thành dựa trên các artifact hiện có
5. Tiếp tục từ giai đoạn hiện tại

### Thiếu Artifact

**Kịch bản**: Các artifact bắt buộc từ giai đoạn trước bị thiếu

**Các bước Khôi phục**:

1. Xác định artifact nào bị thiếu
2. Xác định xem chúng có thể được tạo lại không
3. Nếu có: Quay lại giai đoạn đó, tạo lại artifact
4. Nếu không: Yêu cầu người dùng cung cấp thông tin thủ công
5. Ghi lại khoảng trống trong `audit.md`

### Người dùng Muốn Khởi động lại Giai đoạn

**Kịch bản**: Người dùng không hài lòng với kết quả giai đoạn và muốn làm lại

**Các bước Khôi phục**:

1. Xác nhận người dùng muốn khởi động lại (dữ liệu sẽ bị mất)
2. Lưu trữ các artifact hiện có: `{artifact}.backup`
3. Đặt lại trạng thái giai đoạn trong `aidlc-state.md`
4. Xóa các checkbox giai đoạn trong tệp kế hoạch
5. Thực thi lại giai đoạn từ đầu

### Người dùng Muốn Bỏ qua Giai đoạn

**Kịch bản**: Người dùng muốn bỏ qua một giai đoạn đã được lên kế hoạch

**Các bước Khôi phục**:

1. Xác nhận người dùng hiểu các tác động
2. Ghi lại lý do bỏ qua trong `audit.md`
3. Đánh dấu giai đoạn là "BỎ QUA" trong `aidlc-state.md`
4. Tiến tới giai đoạn tiếp theo
5. Lưu ý: Có thể gây ra vấn đề trong các giai đoạn sau nếu thiếu phụ thuộc

## Hướng dẫn Leo thang

### Khi nào Cần Sự giúp đỡ của Người dùng

**Ngay lập tức**:

- Đầu vào người dùng mâu thuẫn hoặc mơ hồ
- Thiếu thông tin bắt buộc
- Các ràng buộc kỹ thuật AI không thể giải quyết
- Các quyết định yêu cầu phán đoán kinh doanh

**Sau khi Cố gắng Giải quyết**:

- Lặp lại lỗi trong cùng một bước
- Các vấn đề kỹ thuật phức tạp
- Cấu trúc dự án bất thường
- Tích hợp với các hệ thống bên ngoài

### Khi nào Đề xuất Bắt đầu lại

**Xem xét Bắt đầu Mới Nếu**:

- Nhiều giai đoạn có lỗi
- Tệp trạng thái bị hỏng nghiêm trọng
- Người dùng không thể cung cấp thông tin còn thiếu
- Các artifact không nhất quán giữa các giai đoạn
- Yêu cầu người dùng thay đổi đáng kể
- Quyết định kiến trúc cần được đảo ngược

**Trước khi Bắt đầu lại**:

1. Lưu trữ tất cả công việc hiện có
2. Ghi lại các bài học kinh nghiệm
3. Xác định những gì cần bảo tồn
4. Nhận xác nhận của người dùng
5. Tạo kế hoạch thực hiện mới

## Lỗi Tiếp tục Phiên làm việc

### Thiếu Artifact Trong khi Tiếp tục

**Lỗi**: Các artifact bắt buộc từ các giai đoạn trước bị thiếu

- **Nguyên nhân**: Tệp bị xóa, di chuyển hoặc không bao giờ được tạo
- **Giải pháp**:
  1. Xác định giai đoạn nào đã tạo ra các artifact bị thiếu
  2. Kiểm tra xem giai đoạn có được đánh dấu hoàn thành trong aidlc-state.md không
  3. Nếu đánh dấu hoàn thành nhưng artifact bị thiếu: Tạo lại giai đoạn đó
  4. Nếu chưa được đánh dấu hoàn thành: Tiếp tục từ giai đoạn đó
- **Khôi phục**: Quay lại giai đoạn tạo ra các artifact bị thiếu và thực thi lại

**Lỗi**: Tệp artifact tồn tại nhưng trống hoặc bị hỏng

- **Nguyên nhân**: Ghi bị gián đoạn, chỉnh sửa thủ công, vấn đề hệ thống tệp
- **Giải pháp**:
  1. Tạo bản sao lưu của tệp bị hỏng
  2. Cố gắng tạo lại từ giai đoạn tạo ra nó
  3. Nếu không thể tạo lại: Yêu cầu người dùng cung cấp thông tin để tạo lại
- **Khôi phục**: Thực thi lại giai đoạn tạo ra artifact

### Trạng thái Không nhất quán Trong khi Tiếp tục

**Lỗi**: aidlc-state.md hiển thị giai đoạn hoàn thành nhưng artifact không tồn tại

- **Nguyên nhân**: Tệp trạng thái được cập nhật nhưng tạo artifact thất bại
- **Giải pháp**:
  1. Đánh dấu giai đoạn là chưa hoàn thành trong aidlc-state.md
  2. Thực thi lại giai đoạn để tạo artifact
  3. Xác minh artifact tồn tại trước khi đánh dấu hoàn thành
- **Khôi phục**: Đặt lại trạng thái giai đoạn và thực thi lại

**Lỗi**: Artifact tồn tại nhưng aidlc-state.md hiển thị giai đoạn chưa hoàn thành

- **Nguyên nhân**: Tạo artifact thành công nhưng cập nhật trạng thái thất bại
- **Giải pháp**:
  1. Xác minh artifact đã hoàn thành và hợp lệ
  2. Cập nhật aidlc-state.md để đánh dấu giai đoạn hoàn thành
  3. Tiến tới giai đoạn tiếp theo
- **Khôi phục**: Cập nhật tệp trạng thái để phản ánh việc hoàn thành thực tế

**Lỗi**: Nhiều giai đoạn được đánh dấu là "hiện tại" trong aidlc-state.md

- **Nguyên nhân**: Hỏng tệp trạng thái, chỉnh sửa thủ công
- **Giải pháp**:
  1. Xem xét các artifact để xác định tiến độ thực tế
  2. Hỏi người dùng họ thực sự đang ở giai đoạn nào
  3. Sửa aidlc-state.md để hiển thị một giai đoạn hiện tại duy nhất
- **Khôi phục**: Xây dựng lại tệp trạng thái dựa trên các artifact hiện có

### Lỗi Tải Ngữ cảnh

**Lỗi**: Không thể tải ngữ cảnh bắt buộc từ các giai đoạn trước

- **Nguyên nhân**: Thiếu tệp, nội dung bị hỏng, sai đường dẫn tệp
- **Giải pháp**:
  1. Liệt kê các artifact nào cần thiết cho giai đoạn hiện tại
  2. Kiểm tra xem cái nào bị thiếu hoặc bị hỏng
  3. Tạo lại các artifact bị thiếu hoặc yêu cầu người dùng cung cấp thông tin
- **Khôi phục**: Hoàn thành các giai đoạn tiên quyết trước khi tiếp tục giai đoạn hiện tại

**Lỗi**: Các artifact đã tải chứa thông tin mâu thuẫn

- **Nguyên nhân**: Chỉnh sửa thủ công, nhiều người làm việc, cập nhật không đầy đủ
- **Giải pháp**:
  1. Xác định mâu thuẫn và trình bày cho người dùng
  2. Hỏi người dùng thông tin nào là chính xác
  3. Cập nhật artifact để giải quyết mâu thuẫn
- **Khôi phục**: Hòa giải mâu thuẫn trước khi tiếp tục

### Các thực hành Tốt nhất khi Tiếp tục

1. **Luôn xác thực trạng thái**: Kiểm tra aidlc-state.md khớp với các artifact thực tế
2. **Tải tăng dần**: Tải artifact theo từng giai đoạn, xác thực từng cái
3. **Thất bại nhanh**: Dừng ngay lập tức nếu các artifact quan trọng bị thiếu
4. **Giao tiếp rõ ràng**: Nói cho người dùng biết chính xác những gì bị thiếu và tại sao nó cần thiết
5. **Đưa ra tùy chọn**: Tạo lại, cung cấp thủ công hoặc bắt đầu mới
6. **Ghi lại việc khôi phục**: Ghi nhật ký tất cả các hành động khôi phục trong audit.md

## Yêu cầu Ghi nhật ký

### Định dạng Ghi nhật ký Lỗi

```markdown
## Error - [Phase Name]

**Timestamp**: [ISO timestamp]
**Error Type**: [Critical/High/Medium/Low]
**Description**: [What went wrong]
**Cause**: [Why it happened]
**Resolution**: [How it was resolved]
**Impact**: [Effect on workflow]

---
```

### Định dạng Ghi nhật ký Khôi phục

```markdown
## Recovery - [Phase Name]

**Timestamp**: [ISO timestamp]
**Issue**: [What needed recovery]
**Recovery Steps**: [What was done]
**Outcome**: [Result of recovery]
**Artifacts Affected**: [List of files]

---
```

## Các thực hành Tốt nhất về Ngăn ngừa

1. **Xác thực Sớm**: Kiểm tra đầu vào và các phụ thuộc trước khi bắt đầu công việc
2. **Điểm kiểm tra Thường xuyên**: Cập nhật checkbox ngay sau khi hoàn thành các bước
3. **Giao tiếp Rõ ràng**: Giải thích những gì bạn đang làm và tại sao
4. **Đặt câu hỏi**: Đừng giả định - làm rõ sự mơ hồ ngay lập tức
5. **Tài liệu hóa Mọi thứ**: Ghi nhật ký tất cả các quyết định và thay đổi trong `audit.md`
