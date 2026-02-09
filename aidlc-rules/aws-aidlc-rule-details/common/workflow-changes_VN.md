# Thay đổi Giữa Quy trình và Quản lý Giai đoạn

## Tổng quan

Người dùng có thể yêu cầu thay đổi kế hoạch thực hiện hoặc việc thực thi giai đoạn trong quá trình làm việc. Tài liệu này cung cấp hướng dẫn về cách xử lý các yêu cầu này một cách an toàn và hiệu quả.

---

## Các loại Thay đổi Giữa Quy trình

### 1. Thêm một Giai đoạn Bị bỏ qua

**Kịch bản**: Người dùng muốn thêm một giai đoạn mà ban đầu đã bị bỏ qua

**Ví dụ**: "Thực ra, tôi muốn thêm giai đoạn user stories mặc dù chúng ta đã bỏ qua giai đoạn đó"

**Xử lý**:

1. **Xác nhận Yêu cầu**: "Bạn muốn thêm giai đoạn User Stories. Điều này sẽ tạo ra các user stories và personas. Xác nhận?"
2. **Kiểm tra Phụ thuộc**: Xác minh tất cả các giai đoạn tiên quyết đã hoàn thành
3. **Cập nhật Kế hoạch Thực hiện**: Thêm giai đoạn vào `execution-plan.md` với lý do
4. **Cập nhật Trạng thái**: Đánh giá giai đoạn là "PENDING" trong `aidlc-state.md`
5. **Thực thi Giai đoạn**: Tuân theo quy trình thực thi giai đoạn bình thường
6. **Ghi nhật ký Thay đổi**: Ghi lại trong `audit.md` với dấu thời gian và lý do

**Cân nhắc**:

- Có thể cần cập nhật các giai đoạn sau có thể hưởng lợi từ các artifact mới
- Các artifact hiện có có thể cần sửa đổi để kết hợp thông tin mới
- Thời gian sẽ bị kéo dài

---

### 2. Bỏ qua một Giai đoạn Đã lên kế hoạch

**Kịch bản**: Người dùng muốn bỏ qua một giai đoạn đã được lên kế hoạch thực thi

**Ví dụ**: "Hãy bỏ qua giai đoạn Thiết kế NFR ngay bây giờ"

**Xử lý**:

1. **Xác nhận Yêu cầu**: "Bạn muốn bỏ qua Thiết kế NFR. Điều này có nghĩa là không có mẫu NFR hoặc thành phần logic nào được kết hợp. Xác nhận?"
2. **Cảnh báo về Tác động**: Giải thích những gì sẽ bị thiếu và hậu quả tiềm ẩn
3. **Nhận Xác nhận Rõ ràng**: Người dùng phải xác nhận rõ ràng sự hiểu biết về tác động
4. **Cập nhật Kế hoạch Thực hiện**: Đánh dấu giai đoạn là "SKIPPED" với lý do
5. **Cập nhật Trạng thái**: Đánh dấu giai đoạn là "SKIPPED" trong `aidlc-state.md`
6. **Điều chỉnh Các giai đoạn Sau**: Lưu ý rằng các giai đoạn sau có thể cần thiết lập thủ công
7. **Ghi nhật ký Thay đổi**: Ghi lại trong `audit.md` với dấu thời gian và lý do

**Cân nhắc**:

- Các giai đoạn sau có thể thất bại hoặc cần sự can thiệp thủ công
- Người dùng chấp nhận trách nhiệm về các artifact bị thiếu
- Có thể thêm lại sau nếu cần

---

### 3. Khởi động lại Giai đoạn Hiện tại

**Kịch bản**: Người dùng không hài lòng với kết quả giai đoạn hiện tại và muốn làm lại

**Ví dụ**: "Tôi không thích những user stories này. Chúng ta có thể bắt đầu lại không?"

**Xử lý**:

1. **Hiểu Mối quan tâm**: "Bạn muốn thay đổi cụ thể điều gì về các stories?"
2. **Đưa ra Tùy chọn**:
   - **Tùy chọn A**: Sửa đổi các artifact hiện có (nhanh hơn, bảo tồn một số công việc)
   - **Tùy chọn B**: Khởi động lại hoàn toàn (bảng sạch, tốn thời gian hơn)
3. **Nếu Chọn Khởi động lại**:
   - Lưu trữ các artifact hiện có: `{artifact}.backup.{timestamp}`
   - Đặt lại các checkbox giai đoạn trong tệp kế hoạch
   - Đánh dấu giai đoạn là "IN PROGRESS" trong `aidlc-state.md`
   - Xóa trạng thái hoàn thành giai đoạn
   - Thực thi lại từ đầu
4. **Ghi nhật ký Thay đổi**: Ghi lại lý do khởi động lại và những gì sẽ thay đổi

**Cân nhắc**:

- Công việc hiện tại sẽ bị mất (nhưng được sao lưu)
- Có thể cần làm lại các giai đoạn phụ thuộc
- Thời gian sẽ bị kéo dài

---

### 4. Khởi động lại Giai đoạn Trước

**Kịch bản**: Người dùng muốn quay lại và làm lại một giai đoạn đã hoàn thành

**Ví dụ**: "Tôi muốn thay đổi quyết định kiến trúc mà chúng ta đã đưa ra trước đó"

**Xử lý**:

1. **Đánh giá Tác động**: Xác định tất cả các giai đoạn phụ thuộc vào giai đoạn cần khởi động lại
2. **Cảnh báo Người dùng**: "Khởi động lại Thiết kế Ứng dụng sẽ yêu cầu làm lại: Lập kế hoạch Đơn vị, Tạo Đơn vị, thiết kế theo đơn vị (tất cả các đơn vị), Lập kế hoạch Mã, Tạo Mã. Xác nhận?"
3. **Nhận Xác nhận Rõ ràng**: Người dùng phải hiểu đầy đủ tác động
4. **Nếu Đã xác nhận**:
   - Lưu trữ tất cả các artifact bị ảnh hưởng
   - Đặt lại tất cả các giai đoạn bị ảnh hưởng trong `aidlc-state.md`
   - Xóa các checkbox trong tất cả các tệp kế hoạch bị ảnh hưởng
   - Quay lại giai đoạn cần khởi động lại
   - Thực thi lại từ điểm đó trở đi
5. **Ghi nhật ký Thay đổi**: Ghi lại toàn bộ tác động và lý do khởi động lại

**Cân nhắc**:

- Yêu cầu làm lại đáng kể
- Tất cả các giai đoạn phụ thuộc phải được làm lại
- Thời gian sẽ bị kéo dài đáng kể
- Xem xét liệu sửa đổi có tốt hơn khởi động lại không

---

### 5. Thay đổi Độ sâu Giai đoạn

**Kịch bản**: Người dùng muốn thay đổi mức độ chi tiết của giai đoạn hiện tại hoặc sắp tới

**Ví dụ**: "Hãy thực hiện phân tích yêu cầu toàn diện thay vì tiêu chuẩn"

**Xử lý**:

1. **Xác nhận Yêu cầu**: "Bạn muốn thay đổi Phân tích Yêu cầu từ Tiêu chuẩn sang Toàn diện. Điều này sẽ kỹ lưỡng hơn nhưng mất nhiều thời gian hơn. Xác nhận?"
2. **Cập nhật Kế hoạch Thực hiện**: Thay đổi mức độ sâu trong `workflow-planning.md`
3. **Điều chỉnh Cách tiếp cận**: Tuân theo các hướng dẫn độ sâu toàn diện cho giai đoạn
4. **Cập nhật Ước tính**: Thông báo cho người dùng về ước tính thời gian mới
5. **Ghi nhật ký Thay đổi**: Ghi lại thay đổi độ sâu và lý do

**Cân nhắc**:

- Độ sâu hơn = nhiều thời gian hơn nhưng chất lượng tốt hơn
- Độ sâu ít hơn = nhanh hơn nhưng có thể bỏ lỡ chi tiết
- Chỉ có thể thay đổi trước hoặc trong giai đoạn, không phải sau khi hoàn thành

---

### 6. Tạm dừng Quy trình làm việc

**Kịch bản**: Người dùng cần tạm dừng và tiếp tục sau

**Ví dụ**: "Tôi cần dừng lại ngay bây giờ và tiếp tục vào ngày mai"

**Xử lý**:

1. **Hoàn thành Bước Hiện tại**: Hoàn thành bước hiện tại đang tiến hành nếu có thể
2. **Cập nhật Checkbox**: Đánh dấu tất cả các bước đã hoàn thành với [x]
3. **Cập nhật Trạng thái**: Đảm bảo `aidlc-state.md` phản ánh trạng thái hiện tại
4. **Ghi nhật ký Tạm dừng**: Ghi lại điểm tạm dừng trong `audit.md`
5. **Cung cấp Hướng dẫn Tiếp tục**: "Khi bạn quay lại, tôi sẽ phát hiện dự án hiện có của bạn và đề nghị tiếp tục từ: [giai đoạn hiện tại, bước hiện tại]"

**Khi Tiếp tục**:

1. **Phát hiện Dự án Hiện có**: Kiểm tra `aidlc-state.md`
2. **Tải Ngữ cảnh**: Đọc tất cả các artifact từ các giai đoạn đã hoàn thành
3. **Hiển thị Trạng thái**: Hiển thị giai đoạn hiện tại và bước tiếp theo
4. **Đưa ra Tùy chọn**: Tiếp tục nơi đã dừng hoặc xem xét công việc trước đó
5. **Ghi nhật ký Tiếp tục**: Ghi lại điểm tiếp tục trong `audit.md`

---

### 7. Thay đổi Quyết định Kiến trúc

**Kịch bản**: Người dùng muốn thay đổi từ nguyên khối sang vi dịch vụ (hoặc ngược lại)

**Ví dụ**: "Thực ra, hãy làm vi dịch vụ thay vì nguyên khối"

**Xử lý**:

1. **Đánh giá Tiến độ Hiện tại**: Xác định đã đi bao xa trong quy trình
2. **Giải thích Tác động**:
   - Nếu trước Lập kế hoạch Đơn vị: Tác động tối thiểu, chỉ cần cập nhật quyết định
   - Nếu sau Lập kế hoạch Đơn vị: Phải làm lại Lập kế hoạch Đơn vị, Tạo Đơn vị, tất cả thiết kế theo đơn vị
   - Nếu sau Tạo Mã: Yêu cầu làm lại đáng kể
3. **Đề xuất Cách tiếp cận**:
   - Sớm trong quy trình: Khởi động lại từ giai đoạn Thiết kế Ứng dụng
   - Muộn trong quy trình: Xem xét liệu sửa đổi có khả thi so với khởi động lại không
4. **Nhận Xác nhận**: Người dùng phải hiểu đầy đủ phạm vi thay đổi
5. **Thực thi Thay đổi**: Tuân theo các thủ tục khởi động lại cho các giai đoạn bị ảnh hưởng

**Cân nhắc**:

- Các thay đổi kiến trúc có hiệu ứng domino
- Sớm hơn trong quy trình = dễ thay đổi hơn
- Muộn hơn trong quy trình = xem xét chi phí so với lợi ích

---

### 8. Thêm/Xóa Đơn vị

**Kịch bản**: Người dùng muốn thêm hoặc xóa các đơn vị sau khi Tạo Đơn vị

**Ví dụ**: "Chúng ta cần tách đơn vị Thanh toán thành Thanh toán và Lập hóa đơn"

**Xử lý**:

1. **Đánh giá Tác động**: Xác định đơn vị nào đã hoàn thành thiết kế/mã
2. **Giải thích Hậu quả**:
   - Thêm đơn vị: Cần thực hiện thiết kế và mã đầy đủ cho đơn vị mới
   - Xóa đơn vị: Cần phân phối lại chức năng cho các đơn vị khác
   - Tách đơn vị: Cần làm lại thiết kế và mã cho cả hai đơn vị kết quả
3. **Cập nhật Artifact Đơn vị**:
   - Sửa đổi `unit-of-work.md`
   - Cập nhật `unit-of-work-dependency.md`
   - Sửa đổi `unit-of-work-story-map.md`
4. **Đặt lại Đơn vị Bị ảnh hưởng**: Đánh dấu các đơn vị bị ảnh hưởng là cần thiết kế lại
5. **Thực thi Thay đổi**: Tuân theo quy trình thiết kế và mã đơn vị bình thường cho các đơn vị bị ảnh hưởng

**Cân nhắc**:

- Ảnh hưởng đến tất cả các giai đoạn hạ lưu cho các đơn vị đó
- Có thể ảnh hưởng đến các đơn vị khác nếu sự phụ thuộc thay đổi
- Tác động thời gian phụ thuộc vào số lượng đơn vị bị ảnh hưởng

---

## Hướng dẫn Chung để Xử lý Thay đổi

### Trước khi Thực hiện Thay đổi

1. **Hiểu Yêu cầu**: Đặt câu hỏi làm rõ về những gì người dùng muốn thay đổi và tại sao
2. **Đánh giá Tác động**: Xác định tất cả các giai đoạn, artifact và sự phụ thuộc bị ảnh hưởng
3. **Giải thích Hậu quả**: Giao tiếp rõ ràng những gì cần phải làm lại và tác động thời gian
4. **Đưa ra Giải pháp thay thế**: Đôi khi sửa đổi tốt hơn là khởi động lại
5. **Nhận Xác nhận Rõ ràng**: Người dùng phải hiểu và chấp nhận tác động

### Trong khi Thay đổi

1. **Lưu trữ Công việc Hiện có**: Luôn sao lưu trước khi thực hiện các thay đổi phá hủy
2. **Cập nhật Tất cả Theo dõi**: Giữ `aidlc-state.md`, các tệp kế hoạch và `audit.md` đồng bộ
3. **Giao tiếp Tiến độ**: Giữ người dùng được thông báo về những gì đang xảy ra
4. **Xác thực Thay đổi**: Đảm bảo các thay đổi nhất quán trên tất cả các artifact
5. **Kiểm thử Tính liên tục**: Xác minh quy trình làm việc có thể tiếp tục trôi chảy sau các thay đổi

### Sau khi thay đổi

1. **Xác minh Tính nhất quán**: Kiểm tra xem tất cả các artifact có phù hợp với các thay đổi không
2. **Cập nhật Tài liệu**: Đảm bảo tất cả các tham chiếu được cập nhật
3. **Ghi nhật ký Hoàn toàn**: Ghi lại toàn bộ lịch sử thay đổi trong `audit.md`
4. **Xác nhận với Người dùng**: Xác minh các thay đổi đáp ứng mong đợi của người dùng
5. **Tiếp tục Quy trình làm việc**: Tiếp tục với việc thực thi bình thường từ trạng thái mới

---

## Cây Quyết định Yêu cầu Thay đổi

```
Người dùng yêu cầu thay đổi
    |
    ├─ Có phải giai đoạn hiện tại không?
    |   ├─ Có: Có thể sửa đổi hoặc khởi động lại giai đoạn hiện tại
    |   └─ Không: Đi đến câu hỏi tiếp theo
    |
    ├─ Có phải giai đoạn đã hoàn thành không?
    |   ├─ Có: Đánh giá tác động lên các giai đoạn phụ thuộc
    |   |   ├─ Tác động thấp: Sửa đổi và cập nhật phụ thuộc
    |   |   └─ Tác động cao: Đề xuất khởi động lại từ giai đoạn đó
    |   └─ Không: Đi đến câu hỏi tiếp theo
    |
    ├─ Có phải thêm giai đoạn bị bỏ qua không?
    |   ├─ Có: Kiểm tra điều kiện tiên quyết, thêm vào kế hoạch, thực thi
    |   └─ Không: Đi đến câu hỏi tiếp theo
    |
    ├─ Có phải bỏ qua giai đoạn đã lên kế hoạch không?
    |   ├─ Có: Cảnh báo về tác động, nhận xác nhận, bỏ qua
    |   └─ Không: Đi đến câu hỏi tiếp theo
    |
    └─ Có phải thay đổi mức độ độ sâu không?
        ├─ Có: Cập nhật kế hoạch, điều chỉnh cách tiếp cận
        └─ Không: Làm rõ yêu cầu với người dùng
```

---

## Yêu cầu Ghi nhật ký

### Định dạng Nhật ký Yêu cầu Thay đổi

```markdown
## Change Request - [Phase Name]

**Timestamp**: [ISO timestamp]
**Request**: [What user wants to change]
**Current State**: [Where we are in workflow]
**Impact Assessment**: [What will be affected]
**User Confirmation**: [User's explicit confirmation]
**Action Taken**: [What was done]
**Artifacts Affected**: [List of files changed/reset]

---
```

---

## Các thực hành Tốt nhất

1. **Luôn Xác nhận**: Không bao giờ thực hiện các thay đổi phá hủy mà không có xác nhận rõ ràng của người dùng
2. **Giải thích Tác động**: Người dùng cần hiểu hậu quả trước khi quyết định
3. **Đưa ra Tùy chọn**: Đôi khi có nhiều cách để xử lý một thay đổi
4. **Lưu trữ Trước**: Luôn sao lưu trước khi thực hiện các thay đổi phá hủy
5. **Cập nhật Mọi thứ**: Giữ tất cả các tệp theo dõi đồng bộ
6. **Ghi nhật ký Kỹ lưỡng**: Ghi lại tất cả các thay đổi cho hồ sơ kiểm toán
7. **Xác thực Sau đó**: Đảm bảo quy trình làm việc có thể tiếp tục trôi chảy
8. **Linh hoạt**: Quy trình làm việc nên thích ứng với nhu cầu người dùng, không ép buộc quy trình cứng nhắc
