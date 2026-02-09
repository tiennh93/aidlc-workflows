# Thuật ngữ AI-DLC

## Thuật ngữ Cốt lõi

### Phase (Giai đoạn lớn) vs Stage (Giai đoạn nhỏ)

**Phase**: Một trong ba giai đoạn vòng đời cấp cao trong AI-DLC

- 🔵 **GIAI ĐOẠN KHỞI TẠO (INCEPTION)** - Lập kế hoạch & Kiến trúc (CÁI GÌ và TẠI SAO)
- 🟢 **GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION)** - Thiết kế, Triển khai & Kiểm thử (NHƯ THẾ NÀO)
- 🟡 **GIAI ĐOẠN VẬN HÀNH (OPERATIONS)** - Triển khai & Giám sát (mở rộng tương lai)

**Stage**: Một hoạt động quy trình làm việc riêng lẻ trong một phase

- Ví dụ: Stage Đánh giá Ngữ cảnh, Stage Đánh giá Yêu cầu, Stage Lập kế hoạch Mã
- Mỗi stage có các điều kiện tiên quyết, các bước và đầu ra cụ thể
- Các stage có thể là LUÔN-THỰC-THI hoặc CÓ-ĐIỀU-KIỆN

**Ví dụ Sử dụng**:

- ✅ "Phase XÂY DỰNG chứa 7 stage"
- ✅ "Stage Lập kế hoạch Mã luôn được thực thi"
- ✅ "Chúng ta đang trong phase KHỞI TẠO, thực thi stage Đánh giá Yêu cầu"
- ❌ "Phase Đánh giá Yêu cầu" (nên là "stage")
- ❌ "Stage XÂY DỰNG" (nên là "phase")

## Vòng đời Ba Giai đoạn

### GIAI ĐOẠN KHỞI TẠO (INCEPTION PHASE)

**Mục đích**: Lập kế hoạch và các quyết định kiến trúc
**Trọng tâm**: Xác định CÁI GÌ cần xây dựng và TẠI SAO
**Vị trí**: Thư mục `inception/`

**Các Stage**:

- Phát hiện Workspace (LUÔN LUÔN)
- Kỹ thuật Đảo ngược (CÓ ĐIỀU KIỆN - Chỉ Brownfield)
- Phân tích Yêu cầu (LUÔN LUÔN - Độ sâu thích ứng)
- User Stories (CÓ ĐIỀU KIỆN)
- Lập kế hoạch Quy trình làm việc (LUÔN LUÔN)
- Thiết kế Ứng dụng (CÓ ĐIỀU KIỆN)
- Thiết kế - Lập kế hoạch/Tạo Đơn vị (CÓ ĐIỀU KIỆN)

**Đầu ra**: Yêu cầu, user stories, quyết định kiến trúc, định nghĩa đơn vị

### GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION PHASE)

**Mục đích**: Thiết kế chi tiết và triển khai
**Trọng tâm**: Xác định xây dựng NHƯ THẾ NÀO
**Vị trí**: Thư mục `construction/`

**Các Stage**:

- Thiết kế Chức năng (CÓ ĐIỀU KIỆN, theo đơn vị)
- Yêu cầu NFR (CÓ ĐIỀU KIỆN, theo đơn vị)
- Thiết kế NFR (CÓ ĐIỀU KIỆN, theo đơn vị)
- Thiết kế Cơ sở hạ tầng (CÓ ĐIỀU KIỆN, theo đơn vị)
- Lập kế hoạch Mã (LUÔN LUÔN)
- Tạo Mã (LUÔN LUÔN)
- Xây dựng và Kiểm thử (LUÔN LUÔN)

**Đầu ra**: Artifact thiết kế, triển khai NFR, mã, kiểm thử

### GIAI ĐOẠN VẬN HÀNH (OPERATIONS PHASE)

**Mục đích**: Triển khai và sẵn sàng vận hành
**Trọng tâm**: Cách TRIỂN KHAI và CHẠY nó
**Vị trí**: Thư mục `operations/`

**Các Stage**:

- Vận hành (GIỮ CHỖ)

**Đầu ra**: Hướng dẫn xây dựng, hướng dẫn triển khai, thiết lập giám sát, quy trình xác minh

---

## Các Stage Quy trình làm việc

### Các Stage Luôn Thực thi

- **Phát hiện Workspace**: Phân tích ban đầu về trạng thái workspace và loại dự án
- **Phân tích Yêu cầu**: Thu thập yêu cầu (độ sâu thay đổi dựa trên độ phức tạp)
- **Lập kế hoạch Quy trình làm việc**: Tạo kế hoạch thực hiện cho các phase sẽ chạy
- **Lập kế hoạch Mã**: Tạo kế hoạch triển khai chi tiết cho việc tạo mã
- **Tạo Mã**: Tạo mã thực tế dựa trên kế hoạch và các artifact trước đó
- **Xây dựng và Kiểm thử**: Xây dựng tất cả các đơn vị và thực hiện kiểm thử toàn diện

### Các Stage Có điều kiện

- **Kỹ thuật Đảo ngược**: Phân tích codebase hiện có (chỉ dự án brownfield)
- **User Stories**: Tạo user stories và personas (bao gồm Lập kế hoạch Story và Tạo Story)
- **Thiết kế Ứng dụng**: Thiết kế các thành phần ứng dụng, phương thức, quy tắc nghiệp vụ và dịch vụ
- **Thiết kế**: Thiết kế các thành phần hệ thống (bao gồm Lập kế hoạch Đơn vị, Tạo Đơn vị, thiết kế theo đơn vị)
- **Thiết kế Chức năng**: Thiết kế logic nghiệp vụ không phụ thuộc công nghệ (theo đơn vị)
- **Yêu cầu NFR**: Xác định NFR và chọn ngăn xếp công nghệ (theo đơn vị)
- **Thiết kế NFR**: Kết hợp các mẫu NFR và thành phần logic (theo đơn vị)
- **Thiết kế Cơ sở hạ tầng**: Ánh xạ tới các dịch vụ cơ sở hạ tầng thực tế (theo đơn vị)

## Thuật ngữ Thiết kế Ứng dụng

- **Thành phần (Component)**: Một đơn vị chức năng với các trách nhiệm cụ thể
- **Phương thức (Method)**: Một hàm hoặc hoạt động trong một thành phần với các quy tắc nghiệp vụ được xác định
- **Quy tắc Nghiệp vụ**: Logic chi phối hành vi phương thức và xác thực
- **Dịch vụ (Service)**: Lớp điều phối phối hợp logic nghiệp vụ giữa các thành phần
- **Sự phụ thuộc Thành phần**: Mối quan hệ và mẫu giao tiếp giữa các thành phần

## Thuật ngữ Kiến trúc (Cơ sở hạ tầng)

### Đơn vị Công việc (Unit of Work)

Một nhóm logic các user stories cho mục đích phát triển. Thuật ngữ được sử dụng trong quá trình lập kế hoạch và phân rã.

**Sử dụng**: "Chúng ta cần phân rã hệ thống thành các đơn vị công việc"

### Dịch vụ (Service)

Một thành phần có thể triển khai độc lập trong kiến trúc vi dịch vụ. Mỗi dịch vụ là một đơn vị công việc riêng biệt.

**Sử dụng**: "Dịch vụ Thanh toán xử lý tất cả việc xử lý thanh toán"

### Mô-đun (Module)

Một nhóm chức năng logic trong một dịch vụ hoặc khối. Các mô-đun không thể triển khai độc lập.

**Sử dụng**: "Mô-đun xác thực trong Dịch vụ Người dùng"

### Thành phần (Component)

Một khối xây dựng có thể tái sử dụng trong một dịch vụ hoặc mô-đun. Các thành phần là các lớp, hàm hoặc gói cung cấp chức năng cụ thể.

**Sử dụng**: "Thành phần DatabaseConnection quản lý các kết nối"

## Hướng dẫn Thuật ngữ

### Khi nào Sử dụng Từng Thuật ngữ

**Đơn vị Công việc**:

- Trong các phase Lập kế hoạch Đơn vị và Tạo Đơn vị
- Khi thảo luận về phân rã hệ thống
- Trong tài liệu lập kế hoạch và thảo luận
- Ví dụ: "Chúng ta nên phân rã cái này thành các đơn vị công việc như thế nào?"

**Dịch vụ**:

- Khi đề cập đến các thành phần có thể triển khai độc lập
- Trong ngữ cảnh kiến trúc vi dịch vụ
- Trong các thảo luận về triển khai và cơ sở hạ tầng
- Ví dụ: "Dịch vụ Đơn hàng sẽ được triển khai tới ECS"

**Mô-đun**:

- Khi đề cập đến các nhóm logic trong một dịch vụ
- Trong ngữ cảnh kiến trúc nguyên khối
- Khi thảo luận về tổ chức nội bộ
- Ví dụ: "Mô-đun báo cáo tạo ra tất cả các báo cáo"

**Thành phần**:

- Khi đề cập đến các lớp, hàm hoặc gói cụ thể
- Trong các thảo luận về thiết kế và triển khai
- Khi thảo luận về các khối xây dựng có thể tái sử dụng
- Ví dụ: "Thành phần DatabaseConnection quản lý các kết nối"

## Thuật ngữ Stage

### Lập kế hoạch vs Tạo

- **Lập kế hoạch**: Tạo một kế hoạch với các câu hỏi và checkbox để thực hiện
- **Tạo**: Thực thi kế hoạch để tạo artifact

Ví dụ:

- Lập kế hoạch Story → Tạo Story
- Lập kế hoạch Đơn vị → Tạo Đơn vị
- Lập kế hoạch Thiết kế Đơn vị → Tạo Thiết kế Đơn vị
- Lập kế hoạch NFR → Tạo NFR
- Lập kế hoạch Mã → Tạo Mã

### Các mức độ Độ sâu

- **Tối thiểu**: Thực hiện nhanh, tập trung cho các thay đổi đơn giản
- **Tiêu chuẩn**: Độ sâu bình thường với các artifact tiêu chuẩn cho các dự án điển hình
- **Toàn diện**: Độ sâu đầy đủ với tất cả các artifact cho các dự án phức tạp/rủi ro cao

## Các loại Artifact

### Kế hoạch (Plans)

Các tài liệu với checkbox và câu hỏi hướng dẫn thực hiện.

- Vị trí: `aidlc-docs/plans/`
- Ví dụ: `story-generation-plan.md`, `unit-of-work-plan.md`

### Artifacts (Đầu ra)

Kết quả đầu ra từ việc thực thi các kế hoạch.

- Vị trí: các thư mục con `aidlc-docs/` khác nhau
- Ví dụ: `requirements.md`, `stories.md`, `design.md`

### Tệp Trạng thái (State Files)

Các tệp theo dõi tiến độ và trạng thái quy trình làm việc.

- `aidlc-state.md`: Trạng thái quy trình làm việc tổng thể
- `audit.md`: Nhật ký kiểm toán hoàn chỉnh của tất cả các tương tác

## Các từ viết tắt Phổ biến

- **AI-DLC**: Vòng đời Phát triển Theo hướng AI
- **NFR**: Yêu cầu Phi chức năng
- **UOW**: Đơn vị Công việc
- **API**: Giao diện Lập trình Ứng dụng
- **CDK**: Cloud Development Kit (AWS)
