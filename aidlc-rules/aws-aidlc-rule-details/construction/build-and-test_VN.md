# Xây dựng và Kiểm thử

**Mục đích**: Xây dựng tất cả các đơn vị và thực hiện chiến lược kiểm thử toàn diện

## Điều kiện Tiên quyết

- Tạo Mã phải hoàn thành cho tất cả các đơn vị
- Tất cả các artifact mã phải được tạo
- Dự án đã sẵn sàng để xây dựng và kiểm thử

---

## Bước 1: Phân tích Yêu cầu Kiểm thử

Phân tích dự án để xác định chiến lược kiểm thử phù hợp:

- **Kiểm thử Đơn vị**: Đã được tạo theo đơn vị trong quá trình tạo mã
- **Kiểm thử Tích hợp**: Kiểm thử tương tác giữa các đơn vị/dịch vụ
- **Kiểm thử Hiệu năng**: Kiểm thử tải, stress, và khả năng mở rộng
- **Kiểm thử End-to-end**: Kiểm thử toàn bộ quy trình làm việc của người dùng
- **Kiểm thử Hợp đồng**: Xác thực hợp đồng API giữa các dịch vụ
- **Kiểm thử Bảo mật**: Quét lỗ hổng, kiểm thử xâm nhập

---

## Bước 2: Tạo Hướng dẫn Xây dựng

Tạo `aidlc-docs/construction/build-and-test/build-instructions.md`:

```markdown
# Hướng dẫn Xây dựng

## Điều kiện Tiên quyết

- **Công cụ Xây dựng**: [Tên công cụ và phiên bản]
- **Phụ thuộc**: [Liệt kê tất cả các phụ thuộc bắt buộc]
- **Biến Môi trường**: [Liệt kê các biến môi trường bắt buộc]
- **Yêu cầu Hệ thống**: [Hệ điều hành, bộ nhớ, dung lượng đĩa]

## Các bước Xây dựng

### 1. Cài đặt Phụ thuộc

\`\`\`bash
[Lệnh cài đặt phụ thuộc]

# Ví dụ: npm install, mvn dependency:resolve, pip install -r requirements.txt

\`\`\`

### 2. Cấu hình Môi trường

\`\`\`bash
[Lệnh thiết lập môi trường]

# Ví dụ: export biến, cấu hình thông tin xác thực

\`\`\`

### 3. Xây dựng Tất cả Các Đơn vị

\`\`\`bash
[Lệnh xây dựng tất cả các đơn vị]

# Ví dụ: mvn clean install, npm run build, brazil-build

\`\`\`

### 4. Xác minh Thành công Xây dựng

- **Đầu ra Mong đợi**: [Mô tả đầu ra xây dựng thành công]
- **Artifact Xây dựng**: [Liệt kê các artifact được tạo và vị trí]
- **Cảnh báo Thông thường**: [Ghi chú bất kỳ cảnh báo chấp nhận được nào]

## Khắc phục Sự cố

### Xây dựng Thất bại với Lỗi Phụ thuộc

- **Nguyên nhân**: [Nguyên nhân phổ biến]
- **Giải pháp**: [Khắc phục từng bước]

### Xây dựng Thất bại với Lỗi Biên dịch

- **Nguyên nhân**: [Nguyên nhân phổ biến]
- **Giải pháp**: [Khắc phục từng bước]
```

---

## Bước 3: Tạo Hướng dẫn Thực thi Kiểm thử Đơn vị

Tạo `aidlc-docs/construction/build-and-test/unit-test-instructions.md`:

```markdown
# Thực thi Kiểm thử Đơn vị

## Chạy Kiểm thử Đơn vị

### 1. Thực thi Tất cả Kiểm thử Đơn vị

\`\`\`bash
[Lệnh chạy tất cả kiểm thử đơn vị]

# Ví dụ: mvn test, npm test, pytest tests/unit

\`\`\`

### 2. Xem xét Kết quả Kiểm thử

- **Mong đợi**: [X] kiểm thử đạt, 0 thất bại
- **Độ bao phủ Kiểm thử**: [Tỷ lệ bao phủ mong đợi]
- **Vị trí Báo cáo Kiểm thử**: [Đường dẫn đến báo cáo kiểm thử]

### 3. Sửa các Kiểm thử Thất bại

Nếu kiểm thử thất bại:

1. Xem lại đầu ra kiểm thử tại [vị trí]
2. Xác định các test case thất bại
3. Sửa các vấn đề mã
4. Chạy lại kiểm thử cho đến khi tất cả đều đạt
```

---

## Bước 4: Tạo Hướng dẫn Kiểm thử Tích hợp

Tạo `aidlc-docs/construction/build-and-test/integration-test-instructions.md`:

```markdown
# Hướng dẫn Kiểm thử Tích hợp

## Mục đích

Kiểm thử tương tác giữa các đơn vị/dịch vụ để đảm bảo chúng hoạt động cùng nhau chính xác.

## Kịch bản Kiểm thử

### Kịch bản 1: [Đơn vị A] → [Đơn vị B] Tích hợp

- **Mô tả**: [Đang kiểm thử cái gì]
- **Thiết lập**: [Thiết lập môi trường kiểm thử yêu cầu]
- **Các bước Kiểm thử**: [Thực thi kiểm thử từng bước]
- **Kết quả Mong đợi**: [Điều gì nên xảy ra]
- **Dọn dẹp**: [Cách dọn dẹp sau khi kiểm thử]

### Kịch bản 2: [Đơn vị B] → [Đơn vị C] Tích hợp

[Cấu trúc tương tự]

## Thiết lập Môi trường Kiểm thử Tích hợp

### 1. Khởi động Các Dịch vụ Yêu cầu

\`\`\`bash
[Lệnh khởi động dịch vụ]

# Ví dụ: docker-compose up, start test database

\`\`\`

### 2. Cấu hình Điểm cuối Dịch vụ

\`\`\`bash
[Lệnh cấu hình điểm cuối]

# Ví dụ: export API_URL=http://localhost:8080

\`\`\`

## Chạy Kiểm thử Tích hợp

### 1. Thực thi Bộ Kiểm thử Tích hợp

\`\`\`bash
[Lệnh chạy kiểm thử tích hợp]

# Ví dụ: mvn integration-test, npm run test:integration

\`\`\`

### 2. Xác minh Tương tác Dịch vụ

- **Kịch bản Kiểm thử**: [Liệt kê các kịch bản kiểm thử tích hợp chính]
- **Kết quả Mong đợi**: [Mô tả kết quả mong đợi]
- **Vị trí Nhật ký**: [Nơi kiểm tra nhật ký]

### 3. Dọn dẹp

\`\`\`bash
[Lệnh dọn dẹp môi trường kiểm thử]

# Ví dụ: docker-compose down, stop test services

\`\`\`
```

---

## Bước 5: Tạo Hướng dẫn Kiểm thử Hiệu năng (Nếu Có)

Tạo `aidlc-docs/construction/build-and-test/performance-test-instructions.md`:

```markdown
# Hướng dẫn Kiểm thử Hiệu năng

## Mục đích

Xác thực hiệu năng hệ thống dưới tải để đảm bảo đáp ứng yêu cầu.

## Yêu cầu Hiệu năng

- **Thời gian Phản hồi**: < [X]ms cho [Y]% yêu cầu
- **Thông lượng**: [X] yêu cầu/giây
- **Người dùng Đồng thời**: Hỗ trợ [X] người dùng đồng thời
- **Tỷ lệ Lỗi**: < [X]%

## Thiết lập Môi trường Kiểm thử Hiệu năng

### 1. Chuẩn bị Môi trường Kiểm thử

\`\`\`bash
[Lệnh thiết lập kiểm thử hiệu năng]

# Ví dụ: scale services, configure load balancers

\`\`\`

### 2. Cấu hình Tham số Kiểm thử

- **Thời gian Kiểm thử**: [X] phút
- **Thời gian Tăng tốc (Ramp-up)**: [X] giây
- **Người dùng Ảo**: [X] người dùng

## Chạy Kiểm thử Hiệu năng

### 1. Thực thi Kiểm thử Tải (Load Tests)

\`\`\`bash
[Lệnh chạy kiểm thử tải]

# Ví dụ: jmeter -n -t test.jmx, k6 run script.js

\`\`\`

### 2. Thực thi Kiểm thử Stress

\`\`\`bash
[Lệnh chạy kiểm thử stress]

# Ví dụ: tăng dần tải cho đến khi thất bại

\`\`\`

### 3. Phân tích Kết quả Hiệu năng

- **Thời gian Phản hồi**: [Thực tế vs Mong đợi]
- **Thông lượng**: [Thực tế vs Mong đợi]
- **Tỷ lệ Lỗi**: [Thực tế vs Mong đợi]
- **Điểm nghẽn**: [Các điểm nghẽn được xác định]
- **Vị trí Kết quả**: [Đường dẫn đến báo cáo hiệu năng]

## Tối ưu hóa Hiệu năng

Nếu hiệu năng không đáp ứng yêu cầu:

1. Xác định điểm nghẽn từ kết quả kiểm thử
2. Tối ưu hóa mã/truy vấn/cấu hình
3. Chạy lại kiểm thử để xác thực cải tiến
```

---

## Bước 6: Tạo Hướng dẫn Kiểm thử Bổ sung (Khi Cần thiết)

Dựa trên yêu cầu dự án, tạo các tệp hướng dẫn kiểm thử bổ sung:

### Kiểm thử Hợp đồng (Cho Vi dịch vụ)

Tạo `aidlc-docs/construction/build-and-test/contract-test-instructions.md`:

- Xác thực hợp đồng API giữa các dịch vụ
- Kiểm thử hợp đồng hướng người tiêu dùng (Consumer-driven)
- Xác thực Schema

### Kiểm thử Bảo mật

Tạo `aidlc-docs/construction/build-and-test/security-test-instructions.md`:

- Quét lỗ hổng
- Kiểm tra bảo mật phụ thuộc
- Kiểm thử xác thực/ủy quyền
- Kiểm thử xác thực đầu vào

### Kiểm thử End-to-End

Tạo `aidlc-docs/construction/build-and-test/e2e-test-instructions.md`:

- Kiểm thử quy trình làm việc người dùng hoàn chỉnh
- Các kịch bản chéo dịch vụ
- Kiểm thử UI (nếu có)

---

## Bước 7: Tạo Tóm tắt Kiểm thử

Tạo `aidlc-docs/construction/build-and-test/build-and-test-summary.md`:

```markdown
# Tóm tắt Xây dựng và Kiểm thử

## Trạng thái Xây dựng

- **Công cụ Xây dựng**: [Tên công cụ]
- **Trạng thái Xây dựng**: [Thành công/Thất bại]
- **Artifact Xây dựng**: [Liệt kê artifact]
- **Thời gian Xây dựng**: [Thời lượng]

## Tóm tắt Thực thi Kiểm thử

### Kiểm thử Đơn vị

- **Tổng số Kiểm thử**: [X]
- **Đạt**: [X]
- **Thất bại**: [X]
- **Độ bao phủ**: [X]%
- **Trạng thái**: [Đạt/Thất bại]

### Kiểm thử Tích hợp

- **Kịch bản Kiểm thử**: [X]
- **Đạt**: [X]
- **Thất bại**: [X]
- **Trạng thái**: [Đạt/Thất bại]

### Kiểm thử Hiệu năng

- **Thời gian Phản hồi**: [Thực tế] (Mục tiêu: [Mong đợi])
- **Thông lượng**: [Thực tế] (Mục tiêu: [Mong đợi])
- **Tỷ lệ Lỗi**: [Thực tế] (Mục tiêu: [Mong đợi])
- **Trạng thái**: [Đạt/Thất bại]

### Kiểm thử Bổ sung

- **Kiểm thử Hợp đồng**: [Đạt/Thất bại/N/A]
- **Kiểm thử Bảo mật**: [Đạt/Thất bại/N/A]
- **Kiểm thử E2E**: [Đạt/Thất bại/N/A]

## Trạng thái Tổng thể

- **Xây dựng**: [Thành công/Thất bại]
- **Tất cả Kiểm thử**: [Đạt/Thất bại]
- **Sẵn sàng cho Vận hành**: [Có/Không]

## Các bước Tiếp theo

[Nếu tất cả đạt]: Sẵn sàng tiến tới giai đoạn Vận hành để lập kế hoạch triển khai
[Nếu thất bại]: Giải quyết các kiểm thử thất bại và xây dựng lại
```

---

## Bước 8: Cập nhật Theo dõi Trạng thái

Cập nhật `aidlc-docs/aidlc-state.md`:

- Đánh dấu giai đoạn Xây dựng và Kiểm thử là hoàn thành
- Cập nhật trạng thái hiện tại

---

## Bước 9: Trình bày Kết quả cho Người dùng

Trình bày thông điệp toàn diện:

```
"🔨 Xây dựng và Kiểm thử Hoàn thành!

**Trạng thái Xây dựng**: [Thành công/Thất bại]

**Kết quả Kiểm thử**:
✅ Kiểm thử Đơn vị: [X] đạt
✅ Kiểm thử Tích hợp: [X] kịch bản đạt
✅ Kiểm thử Hiệu năng: [Trạng thái]
✅ Kiểm thử Bổ sung: [Trạng thái]

**Các Tệp Được tạo**:
1. ✅ build-instructions.md
2. ✅ unit-test-instructions.md
3. ✅ integration-test-instructions.md
4. ✅ performance-test-instructions.md (nếu có)
5. ✅ [các tệp kiểm thử bổ sung khi cần thiết]
6. ✅ build-and-test-summary.md

Xem tóm tắt trong aidlc-docs/construction/build-and-test/build-and-test-summary.md

**Sẵn sàng tiến tới giai đoạn Vận hành để lập kế hoạch triển khai?""
```

---

## Bước 10: Ghi nhật ký Tương tác

**BẮT BUỘC**: Ghi nhật ký hoàn thành giai đoạn trong `aidlc-docs/audit.md`:

```markdown
## Build and Test Stage

**Timestamp**: [ISO timestamp]
**Build Status**: [Success/Failed]
**Test Status**: [Pass/Fail]
**Files Generated**:

- build-instructions.md
- unit-test-instructions.md
- integration-test-instructions.md
- performance-test-instructions.md
- build-and-test-summary.md

---
```
