# Phát hiện Workspace (Workspace Detection)

**Mục đích**: Xác định trạng thái workspace và kiểm tra các dự án AI-DLC hiện có

## Bước 1: Kiểm tra Dự án AI-DLC Hiện có

Kiểm tra nếu `aidlc-docs/aidlc-state.md` tồn tại:

- **Nếu tồn tại**: Tiếp tục từ giai đoạn cuối cùng (tải ngữ cảnh từ các giai đoạn trước)
- **Nếu không tồn tại**: Tiếp tục với đánh giá dự án mới

## Bước 2: Quét Workspace cho Mã Hiện có

**Xác định nếu workspace có mã hiện có:**

- Quét workspace cho các tệp mã nguồn (.java, .py, .js, .ts, .jsx, .tsx, .kt, .kts, .scala, .groovy, .go, .rs, .rb, .php, .c, .h, .cpp, .hpp, .cc, .cs, .fs, v.v.)
- Kiểm tra các tệp xây dựng (pom.xml, package.json, build.gradle, v.v.)
- Tìm kiếm các chỉ báo cấu trúc dự án
- Xác định thư mục gốc của workspace (KHÔNG phải aidlc-docs/)

**Ghi lại kết quả:**

```markdown
## Workspace State

- **Existing Code**: [Yes/No]
- **Programming Languages**: [List if found]
- **Build System**: [Maven/Gradle/npm/etc. if found]
- **Project Structure**: [Monolith/Microservices/Library/Empty]
- **Workspace Root**: [Absolute path]
```

## Bước 3: Xác định Giai đoạn Tiếp theo

**NẾU workspace trống (không có mã hiện có)**:

- Đặt cờ: `brownfield = false`
- Giai đoạn tiếp theo: Phân tích Yêu cầu

**NẾU workspace có mã hiện có**:

- Đặt cờ: `brownfield = true`
- Kiểm tra các artifact kỹ thuật đảo ngược hiện có trong `aidlc-docs/inception/reverse-engineering/`
- **NẾU artifact kỹ thuật đảo ngược tồn tại**: Tải chúng, bỏ qua đến Phân tích Yêu cầu
- **NẾU không có artifact kỹ thuật đảo ngược**: Giai đoạn tiếp theo là Kỹ thuật Đảo ngược

## Bước 4: Tạo Tệp Trạng thái Ban đầu

Tạo `aidlc-docs/aidlc-state.md`:

```markdown
# AI-DLC State Tracking

## Project Information

- **Project Type**: [Greenfield/Brownfield]
- **Start Date**: [ISO timestamp]
- **Current Stage**: INCEPTION - Workspace Detection

## Workspace State

- **Existing Code**: [Yes/No]
- **Reverse Engineering Needed**: [Yes/No]
- **Workspace Root**: [Absolute path]

## Code Location Rules

- **Application Code**: Workspace root (NEVER in aidlc-docs/)
- **Documentation**: aidlc-docs/ only
- **Structure patterns**: See code-generation.md Critical Rules

## Stage Progress

[Will be populated as workflow progresses]
```

## Bước 5: Trình bày Thông điệp Hoàn thành

**Đối với Dự án Brownfield:**

```markdown
# 🔍 Workspace Detection Complete

Workspace analysis findings:
• **Project Type**: Brownfield project
• [AI-generated summary of workspace findings in bullet points]
• **Next Step**: Proceeding to **Reverse Engineering** to analyze existing codebase...
```

**Đối với Dự án Greenfield:**

```markdown
# 🔍 Workspace Detection Complete

Workspace analysis findings:
• **Project Type**: Greenfield project
• **Next Step**: Proceeding to **Requirements Analysis**...
```

## Bước 6: Tự động Tiếp tục

- **Không yêu cầu phê duyệt của người dùng** - đây chỉ là thông tin
- Tự động tiếp tục đến giai đoạn tiếp theo:
  - **Brownfield**: Kỹ thuật Đảo ngược (nếu không có artifact hiện có) hoặc Phân tích Yêu cầu (nếu artifact tồn tại)
  - **Greenfield**: Phân tích Yêu cầu
