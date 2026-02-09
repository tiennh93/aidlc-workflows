# Tạo Mã - Các bước Chi tiết

## Tổng quan

Giai đoạn này tạo mã cho từng đơn vị công việc thông qua hai phần tích hợp:

- **Phần 1 - Lập kế hoạch**: Tạo kế hoạch tạo mã chi tiết với các bước rõ ràng
- **Phần 2 - Tạo**: Thực thi kế hoạch đã được phê duyệt để tạo mã, kiểm thử và artifact

**Lưu ý**: Đối với các dự án brownfield, "tạo" có nghĩa là sửa đổi các tệp hiện có khi thích hợp, không tạo bản sao.

## Điều kiện Tiên quyết

- Tạo Thiết kế Đơn vị phải hoàn thành cho đơn vị
- Triển khai NFR (nếu thực thi) phải hoàn thành cho đơn vị
- Tất cả các artifact thiết kế đơn vị phải có sẵn
- Đơn vị đã sẵn sàng cho việc tạo mã

---

# PHẦN 1: LẬP KẾ HOẠCH

## Bước 1: Phân tích Ngữ cảnh Đơn vị

- [ ] Đọc các artifact thiết kế đơn vị từ Tạo Thiết kế Đơn vị
- [ ] Đọc bản đồ story đơn vị để hiểu các story được giao
- [ ] Xác định các phụ thuộc đơn vị và giao diện
- [ ] Xác thực đơn vị đã sẵn sàng cho việc tạo mã

## Bước 2: Tạo Kế hoạch Tạo Mã Đơn vị Chi tiết

- [ ] Đọc thư mục gốc workspace và loại dự án từ `aidlc-docs/aidlc-state.md`
- [ ] Xác định vị trí mã (xem Quy tắc Quan trọng cho các mẫu cấu trúc)
- [ ] **Chỉ Brownfield**: Xem xét code-structure.md từ kỹ thuật đảo ngược cho các tệp hiện có cần sửa đổi
- [ ] Ghi lại các đường dẫn chính xác (không bao giờ aidlc-docs/)
- [ ] Tạo các bước rõ ràng cho việc tạo đơn vị:
  - Thiết lập Cấu trúc Dự án (chỉ greenfield)
  - Tạo Logic Nghiệp vụ
  - Kiểm thử Đơn vị Logic Nghiệp vụ
  - Tóm tắt Logic Nghiệp vụ
  - Tạo Lớp API
  - Kiểm thử Đơn vị Lớp API
  - Tóm tắt Lớp API
  - Tạo Lớp Repository
  - Kiểm thử Đơn vị Lớp Repository
  - Tóm tắt Lớp Repository
  - Kịch bản Di chuyển Cơ sở dữ liệu (nếu mô hình dữ liệu tồn tại)
  - Tạo Tài liệu (tài liệu API, cập nhật README)
  - Tạo Artifact Triển khai
- [ ] Đánh số tuần tự từng bước
- [ ] Bao gồm các tham chiếu ánh xạ story
- [ ] Thêm checkbox [ ] cho mỗi bước

## Bước 3: Bao gồm Ngữ cảnh Tạo Đơn vị

- [ ] Đối với đơn vị này, bao gồm:
  - Các story được triển khai bởi đơn vị này
  - Sự phụ thuộc vào các đơn vị/dịch vụ khác
  - Các giao diện và hợp đồng mong đợi
  - Các thực thể cơ sở dữ liệu thuộc sở hữu của đơn vị này
  - Ranh giới dịch vụ và trách nhiệm

## Bước 4: Tạo Tài liệu Kế hoạch Đơn vị

- [ ] Lưu kế hoạch hoàn chỉnh dưới dạng `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`
- [ ] Bao gồm đánh số bước (Bước 1, Bước 2, v.v.)
- [ ] Bao gồm ngữ cảnh đơn vị và các phụ thuộc
- [ ] Bao gồm khả năng truy vết story
- [ ] Đảm bảo kế hoạch có thể thực thi từng bước
- [ ] Nhấn mạnh rằng kế hoạch này là nguồn sự thật duy nhất cho việc Tạo Mã

## Bước 5: Tóm tắt Kế hoạch Đơn vị

- [ ] Cung cấp tóm tắt về kế hoạch tạo mã đơn vị cho người dùng
- [ ] Làm nổi bật cách tiếp cận tạo đơn vị
- [ ] Giải thích trình tự bước và độ bao phủ story
- [ ] Ghi chú tổng số bước và phạm vi ước tính

## Bước 6: Ghi nhật ký Nhắc nhở Phê duyệt

- [ ] Trước khi yêu cầu phê duyệt, ghi nhật ký lời nhắc với dấu thời gian trong `aidlc-docs/audit.md`
- [ ] Bao gồm tham chiếu đến kế hoạch tạo mã đơn vị hoàn chỉnh
- [ ] Sử dụng định dạng dấu thời gian ISO 8601

## Bước 7: Chờ Phê duyệt Rõ ràng

- [ ] Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng kế hoạch tạo mã đơn vị
- [ ] Phê duyệt phải bao gồm toàn bộ kế hoạch và trình tự tạo
- [ ] Nếu người dùng yêu cầu thay đổi, cập nhật kế hoạch và lặp lại quy trình phê duyệt

## Bước 8: Ghi lại Phản hồi Phê duyệt

- [ ] Ghi nhật ký phản hồi phê duyệt của người dùng với dấu thời gian trong `aidlc-docs/audit.md`
- [ ] Bao gồm văn bản phản hồi chính xác của người dùng
- [ ] Đánh dấu trạng thái phê duyệt rõ ràng

## Bước 9: Cập nhật Tiến độ

- [ ] Đánh dấu Lập kế hoạch Mã hoàn thành trong `aidlc-state.md`
- [ ] Cập nhật phần "Trạng thái Hiện tại"
- [ ] Chuẩn bị chuyển sang Tạo Mã

---

# PHẦN 2: TẠO

## Bước 10: Tải Kế hoạch Tạo Mã Đơn vị

- [ ] Đọc kế hoạch hoàn chỉnh từ `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`
- [ ] Xác định bước chưa hoàn thành tiếp theo (checkbox [ ] đầu tiên)
- [ ] Tải ngữ cảnh cho bước đó (đơn vị, phụ thuộc, stories)

## Bước 11: Thực thi Bước Hiện tại

- [ ] Xác minh thư mục đích từ kế hoạch (không bao giờ aidlc-docs/)
- [ ] **Chỉ Brownfield**: Kiểm tra xem tệp đích có tồn tại không
- [ ] Tạo chính xác những gì bước hiện tại mô tả:
  - **Nếu tệp tồn tại**: Sửa đổi tại chỗ (không bao giờ tạo `ClassName_modified.java`, `ClassName_new.java`, v.v.)
  - **Nếu tệp không tồn tại**: Tạo tệp mới
- [ ] Ghi vào đúng vị trí:
  - **Mã Ứng dụng**: Thư mục gốc workspace theo cấu trúc dự án
  - **Tài liệu**: `aidlc-docs/construction/{unit-name}/code/` (chỉ markdown)
  - **Tệp Xây dựng/Cấu hình**: Thư mục gốc workspace
- [ ] Tuân theo yêu cầu story đơn vị
- [ ] Tôn trọng các phụ thuộc và giao diện

## Bước 12: Cập nhật Tiến độ

- [ ] Đánh dấu bước đã hoàn thành là [x] trong kế hoạch tạo mã đơn vị
- [ ] Đánh dấu các unit stories liên quan là [x] khi việc tạo chúng hoàn tất
- [ ] Cập nhật trạng thái hiện tại `aidlc-docs/aidlc-state.md`
- [ ] **Chỉ Brownfield**: Xác minh không có tệp trùng lặp nào được tạo (ví dụ: không có `ClassName_modified.java` cùng với `ClassName.java`)
- [ ] Lưu tất cả các artifact đã tạo

## Bước 13: Tiếp tục hoặc Hoàn thành Tạo

- [ ] Nếu còn bước, quay lại Bước 10
- [ ] Nếu tất cả các bước hoàn thành, tiến hành trình bày thông điệp hoàn thành

## Bước 14: Trình bày Thông điệp Hoàn thành

- Trình bày thông điệp hoàn thành theo cấu trúc này:
  1.  **Thông báo Hoàn thành** (bắt buộc): Luôn bắt đầu với điều này:

```markdown
# 💻 Code Generation Complete - [unit-name]
```

     2. **Tóm tắt AI** (tùy chọn): Cung cấp tóm tắt gạch đầu dòng có cấu trúc
        - **Brownfield**: Phân biệt tệp đã sửa đổi vs đã tạo (ví dụ: "• Modified: `src/services/user-service.ts`", "• Created: `src/services/auth-service.ts`")
        - **Greenfield**: Liệt kê các tệp đã tạo với đường dẫn (ví dụ: "• Created: `src/services/user-service.ts`")
        - Liệt kê kiểm thử, tài liệu, artifact triển khai với đường dẫn
        - Giữ thực tế, không hướng dẫn quy trình làm việc
     3. **Thông điệp Quy trình Đã định dạng** (bắt buộc): Luôn kết thúc với định dạng chính xác này:

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the generated code at:
>
> - **Application Code**: `[actual-workspace-path]`
> - **Documentation**: `aidlc-docs/construction/[unit-name]/code/`
>
> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the generated code based on your review  
> ✅ **Continue to Next Stage** - Approve code generation and proceed to **[next-unit/Build & Test]**
>
> ---
```

## Bước 15: Chờ Phê duyệt Rõ ràng

- Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng mã đã tạo
- Phê duyệt phải rõ ràng và không mơ hồ
- Nếu người dùng yêu cầu thay đổi, cập nhật mã và lặp lại quy trình phê duyệt

## Bước 16: Ghi lại Phê duyệt và Cập nhật Tiến độ

- Ghi nhật ký phê duyệt trong audit.md với dấu thời gian
- Ghi lại phản hồi phê duyệt của người dùng với dấu thời gian
- Đánh dấu giai đoạn Tạo Mã hoàn thành cho đơn vị này trong aidlc-state.md

---

## Các Quy tắc Quan trọng

### Quy tắc Vị trí Mã

- **Mã ứng dụng**: Chỉ thư mục gốc workspace (KHÔNG BAO GIỜ aidlc-docs/)
- **Tài liệu**: chỉ aidlc-docs/ (tóm tắt markdown)
- **Đọc thư mục gốc workspace** từ aidlc-state.md trước khi tạo mã

**Mẫu cấu trúc theo loại dự án**:

- **Brownfield**: Sử dụng cấu trúc hiện có (ví dụ: `src/main/java/`, `lib/`, `pkg/`)
- **Greenfield đơn vị đơn lẻ**: `src/`, `tests/`, `config/` trong thư mục gốc workspace
- **Greenfield đa đơn vị (vi dịch vụ)**: `{unit-name}/src/`, `{unit-name}/tests/`
- **Greenfield đa đơn vị (nguyên khối)**: `src/{unit-name}/`, `tests/{unit-name}/`

### Quy tắc Sửa đổi Tệp Brownfield

- Kiểm tra xem tệp có tồn tại không trước khi tạo
- Nếu tồn tại: Sửa đổi tại chỗ (không bao giờ tạo bản sao như `ClassName_modified.java`)
- Nếu không tồn tại: Tạo tệp mới
- Xác minh không có tệp trùng lặp sau khi tạo (Bước 12)

### Quy tắc Giai đoạn Lập kế hoạch

- Tạo các bước rõ ràng, được đánh số cho tất cả các hoạt động tạo
- Bao gồm khả năng truy vết story trong kế hoạch
- Ghi lại ngữ cảnh đơn vị và các phụ thuộc
- Nhận phê duyệt rõ ràng của người dùng trước khi tạo

### Quy tắc Giai đoạn Tạo

- **KHÔNG LOGIC ĐƯỢC MÃ HÓA CỨNG**: Chỉ thực thi những gì được viết trong kế hoạch đơn vị
- **TUÂN THEO KẾ HOẠCH CHÍNH XÁC**: Không đi chệch khỏi trình tự bước
- **CẬP NHẬT CHECKBOX**: Đánh dấu [x] ngay lập tức sau khi hoàn thành mỗi bước
- **TRUY VẾT STORY**: Đánh dấu unit stories [x] khi chức năng được triển khai
- **TÔN TRỌNG PHỤ THUỘC**: Chỉ triển khai khi các phụ thuộc đơn vị được thỏa mãn

### Quy tắc Mã Thân thiện với Tự động hóa

Khi tạo mã UI (web, di động, máy tính để bàn), đảm bảo các phần tử thân thiện với tự động hóa:

- Thêm thuộc tính `data-testid` vào các phần tử tương tác (nút, đầu vào, liên kết, biểu mẫu)
- Sử dụng cách đặt tên nhất quán: `{component}-{element-role}` (ví dụ: `login-form-submit-button`, `user-list-search-input`)
- Tránh ID động hoặc tự động tạo thay đổi giữa các lần render
- Giữ giá trị `data-testid` ổn định qua các thay đổi mã (chỉ thay đổi khi mục đích phần tử thay đổi)

## Tiêu chí Hoàn thành

- Kế hoạch tạo mã đơn vị hoàn chỉnh được tạo và phê duyệt
- Tất cả các bước trong kế hoạch tạo mã đơn vị được đánh dấu [x]
- Tất cả các unit stories được triển khai theo kế hoạch
- Tất cả mã và kiểm thử được tạo (kiểm thử sẽ được thực thi trong giai đoạn Xây dựng & Kiểm thử)
- Artifact triển khai được tạo
- Đơn vị hoàn chỉnh sẵn sàng cho xây dựng và xác minh
