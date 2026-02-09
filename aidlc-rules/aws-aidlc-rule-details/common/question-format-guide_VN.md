# Hướng dẫn Định dạng Câu hỏi

## BẮT BUỘC: Tất cả Câu hỏi Phải Sử dụng Định dạng Này

### Quy tắc: Không bao giờ Đặt câu hỏi trong Trò chuyện

**QUAN TRỌNG**: Bạn KHÔNG BAO GIỜ được đặt câu hỏi trực tiếp trong cuộc trò chuyện. TẤT CẢ các câu hỏi phải được đặt trong các tệp câu hỏi chuyên dụng.

### Định dạng Tệp Câu hỏi

#### Quy ước Đặt tên Tệp

- Sử dụng tên mô tả: `{tên-giai-đoạn}-questions.md`
- Ví dụ:
  - `classification-questions.md`
  - `requirements-questions.md`
  - `story-planning-questions.md`
  - `design-questions.md`

#### Cấu trúc Câu hỏi

Mỗi câu hỏi phải bao gồm các tùy chọn có ý nghĩa cộng với "Khác" là tùy chọn cuối cùng:

```markdown
## Câu hỏi [Số]

[Nội dung câu hỏi rõ ràng, cụ thể]

A) [Tùy chọn có ý nghĩa đầu tiên]
B) [Tùy chọn có ý nghĩa thứ hai]
[...các tùy chọn bổ sung khi cần thiết...]
X) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

**QUAN TRỌNG**:

- "Khác" là BẮT BUỘC là tùy chọn CUỐI CÙNG cho mỗi câu hỏi
- Chỉ bao gồm các tùy chọn có ý nghĩa - không bịa ra các tùy chọn để điền vào chỗ trống
- Sử dụng bao nhiêu tùy chọn tùy ý miễn là hợp lý (tối thiểu 2 + Khác)

### Ví dụ Hoàn chỉnh

```markdown
# Các câu hỏi Làm rõ Yêu cầu

Vui lòng trả lời các câu hỏi sau để giúp làm rõ các yêu cầu.

## Câu hỏi 1

Phương thức xác thực người dùng chính là gì?

A) Tên người dùng và mật khẩu
B) Đăng nhập mạng xã hội (Google, Facebook)
C) Đăng nhập một lần (SSO)
D) Xác thực đa yếu tố
E) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:

## Câu hỏi 2

Đây sẽ là ứng dụng web hay di động?

A) Ứng dụng web
B) Ứng dụng di động
C) Cả web và di động
D) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:

## Câu hỏi 3

Đây là dự án mới hay codebase hiện có?

A) Dự án mới (greenfield)
B) Codebase hiện có (brownfield)
C) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

### Định dạng Phản hồi của Người dùng

Người dùng sẽ trả lời bằng cách điền vào lựa chọn chữ cái sau thẻ [Answer]:

```markdown
## Câu hỏi 1

Phương thức xác thực người dùng chính là gì?

A) Tên người dùng và mật khẩu
B) Đăng nhập mạng xã hội (Google, Facebook)
C) Đăng nhập một lần (SSO)
D) Xác thực đa yếu tố

[Answer]: C
```

### Đọc Phản hồi của Người dùng

Sau khi người dùng xác nhận hoàn thành:

1. Đọc tệp câu hỏi
2. Trích xuất câu trả lời sau thẻ [Answer]:
3. Xác thực tất cả các câu hỏi đã được trả lời
4. Tiếp tục phân tích dựa trên phản hồi

### Hướng dẫn Trắc nghiệm

#### Số lượng Tùy chọn

- Tối thiểu: 2 tùy chọn có ý nghĩa + "Khác" (A, B, C)
- Điển hình: 3-4 tùy chọn có ý nghĩa + "Khác" (A, B, C, D, E)
- Tối đa: 5 tùy chọn có ý nghĩa + "Khác" (A, B, C, D, E, F)
- **QUAN TRỌNG**: Đừng bịa ra các tùy chọn chỉ để điền vào chỗ trống - chỉ bao gồm các lựa chọn có ý nghĩa

#### Chất lượng Tùy chọn

- Làm cho các tùy chọn loại trừ lẫn nhau
- Bao gồm các kịch bản phổ biến nhất
- Chỉ bao gồm các tùy chọn có ý nghĩa, thực tế
- **LUÔN bao gồm "Khác" là tùy chọn CUỐI CÙNG** (BẮT BUỘC)
- Cụ thể và rõ ràng
- **Đừng bịa ra các tùy chọn để điền vào các vị trí A, B, C, D**

#### Ví dụ Tốt:

```markdown
## Câu hỏi 5

Công nghệ cơ sở dữ liệu nào sẽ được sử dụng?

A) Quan hệ (PostgreSQL, MySQL)
B) Tài liệu NoSQL (MongoDB, DynamoDB)
C) Key-Value NoSQL (Redis, Memcached)
D) Cơ sở dữ liệu đồ thị (Neo4j, Neptune)
E) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

#### Ví dụ Xấu (Tránh):

```markdown
## Câu hỏi 5

Bạn sẽ sử dụng cơ sở dữ liệu nào?

A) Có
B) Không
C) Có thể

[Answer]:
```

### Tích hợp Quy trình làm việc

#### Bước 1: Tạo Tệp Câu hỏi

```markdown
Tạo aidlc-docs/{tên-giai-đoạn}-questions.md với tất cả các câu hỏi
```

#### Bước 2: Thông báo cho Người dùng

```
"Tôi đã tạo {tên-giai-đoạn}-questions.md với [X] câu hỏi.
Vui lòng trả lời từng câu hỏi bằng cách điền vào lựa chọn chữ cái sau thẻ [Answer]:.
Nếu không có tùy chọn nào phù hợp với nhu cầu của bạn, hãy chọn tùy chọn cuối cùng (Khác) và mô tả sở thích của bạn. Hãy cho tôi biết khi bạn hoàn thành."
```

#### Bước 3: Chờ Xác nhận

Chờ người dùng nói "xong", "hoàn thành", "kết thúc" hoặc tương tự.

#### Bước 4: Đọc và Phân tích

```
Đọc aidlc-docs/{tên-giai-đoạn}-questions.md
Trích xuất tất cả câu trả lời
Xác thực tính đầy đủ
Tiếp tục phân tích
```

### Xử lý Lỗi

#### Thiếu Câu trả lời

Nếu bất kỳ thẻ [Answer]: nào trống:

```
"Tôi nhận thấy Câu hỏi [X] chưa được trả lời. Vui lòng cung cấp câu trả lời bằng cách sử dụng một trong các lựa chọn chữ cái
cho tất cả các câu hỏi trước khi tiếp tục."
```

#### Câu trả lời Không hợp lệ

Nếu câu trả lời không phải là lựa chọn chữ cái hợp lệ:

```
"Câu hỏi [X] có câu trả lời không hợp lệ '[câu trả lời]'.
Vui lòng chỉ sử dụng các lựa chọn chữ cái được cung cấp trong câu hỏi."
```

#### Câu trả lời Mơ hồ

Nếu người dùng cung cấp giải thích thay vì chữ cái:

```
"Đối với Câu hỏi [X], vui lòng cung cấp lựa chọn chữ cái phù hợp nhất với câu trả lời của bạn.
Nếu không có cái nào phù hợp, hãy chọn 'Khác' và thêm mô tả của bạn sau thẻ [Answer]:."
```

### Phát hiện Mâu thuẫn và Mơ hồ

**BẮT BUỘC**: Sau khi đọc phản hồi của người dùng, bạn PHẢI kiểm tra các mâu thuẫn và sự mơ hồ.

#### Phát hiện Mâu thuẫn

Tìm kiếm các câu trả lời không nhất quán về mặt logic:

- Sai lệch phạm vi: "Sửa lỗi" nhưng "Toàn bộ codebase bị ảnh hưởng"
- Sai lệch rủi ro: "Rủi ro thấp" nhưng "Thay đổi phá vỡ"
- Sai lệch thời gian: "Sửa nhanh" nhưng "Nhiều hệ thống con"
- Sai lệch tác động: "Thành phần đơn lẻ" nhưng "Thay đổi kiến trúc đáng kể"

#### Phát hiện Sự mơ hồ

Tìm kiếm các phản hồi không rõ ràng hoặc ranh giới:

- Các câu trả lời có thể phù hợp với nhiều phân loại
- Phản hồi thiếu tính cụ thể
- Các chỉ báo xung đột trên nhiều câu hỏi

#### Tạo Câu hỏi Làm rõ

Nếu phát hiện mâu thuẫn hoặc sự mơ hồ:

1. **Tạo tệp làm rõ**: `{tên-giai-đoạn}-clarification-questions.md`
2. **Giải thích vấn đề**: Nêu rõ mâu thuẫn/mơ hồ nào đã được phát hiện
3. **Đặt câu hỏi nhắm mục tiêu**: Sử dụng định dạng trắc nghiệm để giải quyết vấn đề
4. **Tham chiếu câu hỏi gốc**: Chỉ ra câu hỏi nào có câu trả lời xung đột

**Ví dụ**:

```markdown
# Các câu hỏi Làm rõ [Tên Giai đoạn]

Tôi đã phát hiện mâu thuẫn trong các phản hồi của bạn cần làm rõ:

## Mâu thuẫn 1: [Mô tả Ngắn gọn]

Bạn đã chỉ định "[Câu trả lời A]" (Q[X]:[Chữ cái]) nhưng cũng "[Câu trả lời B]" (Q[Y]:[Chữ cái]).
Những phản hồi này mâu thuẫn vì [giải thích].

### Câu hỏi Làm rõ 1

[Câu hỏi cụ thể để giải quyết mâu thuẫn]

A) [Tùy chọn giải quyết theo hướng câu trả lời đầu tiên]
B) [Tùy chọn giải quyết theo hướng câu trả lời thứ hai]
C) [Tùy chọn cung cấp trung gian]
D) [Tùy chọn định hình lại câu hỏi]

[Answer]:

## Mơ hồ 1: [Mô tả Ngắn gọn]

Phản hồi của bạn cho Q[X] ("[Câu trả lời]") là mơ hồ vì [giải thích].

### Câu hỏi Làm rõ 2

[Câu hỏi cụ thể để làm rõ sự mơ hồ]

A) [Tùy chọn rõ ràng 1]
B) [Tùy chọn rõ ràng 2]
C) [Tùy chọn rõ ràng 3]
D) [Tùy chọn rõ ràng 4]

[Answer]:
```

#### Quy trình làm việc cho Làm rõ

1. **Phát hiện**: Phân tích tất cả các phản hồi cho mâu thuẫn/mơ hồ
2. **Tạo**: Tạo tệp câu hỏi làm rõ nếu tìm thấy vấn đề
3. **Thông báo**: Nói cho người dùng về các vấn đề và tệp làm rõ
4. **Chờ**: Không tiếp tục cho đến khi người dùng cung cấp làm rõ
5. **Xác thực lại**: Sau khi làm rõ, kiểm tra lại tính nhất quán
6. **Tiếp tục**: Chỉ di chuyển về phía trước khi tất cả các mâu thuẫn được giải quyết

#### Ví dụ Tin nhắn Người dùng

```
"Tôi đã phát hiện 2 mâu thuẫn trong các phản hồi của bạn:

1. Phạm vi sửa lỗi vs tác động codebase (Q1 vs Q2)
2. Rủi ro thấp vs thay đổi phá vỡ (Q7 vs Q4)

Tôi đã tạo classification-clarification-questions.md với 2 câu hỏi để giải quyết những vấn đề này.
Vui lòng trả lời những câu hỏi làm rõ này trước khi tôi có thể tiếp tục phân loại."
```

### Các thực hành Tốt nhất

1. **Cụ thể**: Câu hỏi nên rõ ràng và không mơ hồ
2. **Toàn diện**: Bao gồm tất cả thông tin cần thiết
3. **Ngắn gọn**: Giữ câu hỏi tập trung vào một chủ đề
4. **Thực tế**: Các tùy chọn nên thực tế và có thể hành động
5. **Nhất quán**: Sử dụng cùng một định dạng trong suốt tất cả các tệp câu hỏi

### Các ví dụ Cụ thể theo Giai đoạn

#### Ví dụ với 2 tùy chọn có ý nghĩa:

```markdown
## Câu hỏi 1

Đây là dự án mới hay codebase hiện có?

A) Dự án mới (greenfield)
B) Codebase hiện có (brownfield)
C) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

#### Ví dụ với 3 tùy chọn có ý nghĩa:

```markdown
## Câu hỏi 2

Mục tiêu triển khai là gì?

A) Đám mây (AWS, Azure, GCP)
B) Máy chủ tại chỗ (On-premises)
C) Lai (cả đám mây và tại chỗ)
D) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

#### Ví dụ với 4 tùy chọn có ý nghĩa:

```markdown
## Câu hỏi 3

Mẫu kiến trúc nào nên được sử dụng?

A) Kiến trúc nguyên khối (Monolithic)
B) Kiến trúc vi dịch vụ (Microservices)
C) Kiến trúc không máy chủ (Serverless)
D) Kiến trúc hướng sự kiện (Event-driven)
E) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

## Tóm tắt

**Hãy nhớ**:

- ✅ Luôn tạo tệp câu hỏi
- ✅ Luôn sử dụng định dạng trắc nghiệm
- ✅ **Luôn bao gồm "Khác" là tùy chọn CUỐI CÙNG (BẮT BUỘC)**
- ✅ Chỉ bao gồm các tùy chọn có ý nghĩa - không bịa ra các tùy chọn để điền vào chỗ trống
- ✅ Luôn sử dụng thẻ [Answer]:
- ✅ Luôn chờ người dùng hoàn thành
- ✅ Luôn xác thực phản hồi cho mâu thuẫn
- ✅ Luôn tạo tệp làm rõ nếu cần
- ✅ Luôn giải quyết mâu thuẫn trước khi tiếp tục
- ❌ Không bao giờ đặt câu hỏi trong trò chuyện
- ❌ Không bao giờ bịa ra các tùy chọn chỉ để có A, B, C, D
- ❌ Không bao giờ tiếp tục mà không có câu trả lời
- ❌ Không bao giờ tiếp tục với các mâu thuẫn chưa được giải quyết
- ❌ Không bao giờ đưa ra giả định về các phản hồi mơ hồ
