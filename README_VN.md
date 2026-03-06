# AI-DLC (Vòng đời Phát triển Dựa trên AI)

AI-DLC là một quy trình phát triển phần mềm thông minh thích ứng với nhu cầu của bạn, duy trì các tiêu chuẩn chất lượng và giữ cho bạn quyền kiểm soát quy trình. Để tìm hiểu thêm về Phương pháp luận AI-DLC, hãy đọc [bài đăng trên blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) này và [Tài liệu Định nghĩa Phương pháp](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) được tham chiếu trong đó.

## Mục lục

- [Bắt đầu Nhanh](#bắt-đầu-nhanh)
- [Cài đặt theo Nền tảng](#cài-đặt-theo-nền-tảng)
- [Cách sử dụng](#cách-sử-dụng)
- [Quy trình làm việc Thích ứng Ba Giai đoạn](#quy-trình-làm-việc-thích-ứng-ba-giai-đoạn)
- [Các Tính năng Chính](#các-tính-năng-chính)
- [Phần mở rộng](#phần-mở-rộng)
- [Nguyên tắc](#nguyên-tắc)
- [Điều kiện Tiên quyết](#điều-kiện-tiên-quyết)
- [Khắc phục sự cố](#khắc-phục-sự-cố)
- [Tài nguyên Bổ sung](#tài-nguyên-bổ-sung)

---

## Bắt đầu Nhanh

1. Tải xuống tệp zip bản phát hành mới nhất từ [Trang Releases](../../releases/latest) vào một thư mục **bên ngoài** thư mục dự án của bạn (ví dụ: `~/Downloads`).
2. Giải nén tệp zip. Nó chứa một thư mục `aidlc-rules/` với hai thư mục con:
   - `aws-aidlc-rules/` — các quy tắc quy trình làm việc AI-DLC cốt lõi
   - `aws-aidlc-rule-details/` — các quy tắc chi tiết được tham chiếu có điều kiện bởi các quy tắc cốt lõi
3. Làm theo hướng dẫn cài đặt cho agent lập trình và nền tảng của bạn bên dưới.

---

## Cài đặt theo Nền tảng

- [Kiro](#kiro)
- [Amazon Q Developer IDE Plugin](#amazon-q-developer-ide-pluginextension)
- [Cursor IDE](#cursor-ide)
- [Cline](#cline)
- [Claude Code](#claude-code)
- [GitHub Copilot](#github-copilot)

---

### Kiro

AI-DLC sử dụng [Kiro Steering Files](https://kiro.dev/docs/cli/steering/) bên trong workspace của dự án của bạn.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

Trên macOS/Linux:

```bash
mkdir -p .kiro/steering
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rules .kiro/steering/
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details .kiro/
```

Trên Windows (CMD):

```cmd
mkdir .kiro\steering
xcopy %USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules .kiro\steering\aws-aidlc-rules\ /E /I
xcopy %USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details .kiro\aws-aidlc-rule-details\ /E /I
```

Dự án của bạn sẽ trông như sau:

```
<project-root>/
    ├── .kiro/
    │     ├── steering/
    │     │      ├── aws-aidlc-rules/
    │     ├── aws-aidlc-rule-details/
```

Để xác minh các quy tắc đã được tải:

#### Xác minh trong Kiro IDE

Mở bảng điều khiển tệp steering và xác nhận bạn thấy một mục nhập cho `core-workflow` trong phần `Workspace` như hình hiển thị bên dưới.

<img src="./assets/images/kiro-ide-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Kiro IDE" width="700" height="450">

Chúng tôi sử dụng Kiro IDE ở chế độ Vibe để chạy quy trình làm việc AI-DLC. Điều này đảm bảo rằng quy trình làm việc AI-DLC hướng dẫn quy trình phát triển trong Kiro. Đôi khi, Kiro có thể gợi ý bạn chuyển sang chế độ spec. Chọn `No` với các lời nhắc như vậy để tiếp tục ở chế độ Vibe.

<img src="./assets/images/kiro-sdd-nudge.png" alt="Staying in Kiro Vibe mode" width="500" height="175">

#### Xác minh trong Kiro CLI

Chạy `kiro-cli`, sau đó gõ `/context show`, và xác nhận các mục nhập cho `.kiro/steering/aws-aidlc-rules`.

<img src="./assets/images/kiro-cli-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Kiro CLI" width="700" height="660">

---

### Amazon Q Developer IDE Plugin/Extension

AI-DLC sử dụng [Amazon Q Rules](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/context-project-rules.html) bên trong workspace của dự án của bạn.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

Trên macOS/Linux:

```bash
mkdir -p .amazonq/rules
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rules .amazonq/rules/
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details .amazonq/
```

Trên Windows (CMD):

```cmd
mkdir .amazonq\rules
xcopy %USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules .amazonq\rules\aws-aidlc-rules\ /E /I
xcopy %USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details .amazonq\aws-aidlc-rule-details\ /E /I
```

Dự án của bạn sẽ trông như sau:

```
<project-root>/
    ├── .amazonq/
    │     ├── rules/
    │     │     ├── aws-aidlc-rules/
    │     ├── aws-aidlc-rule-details/
```

Để xác minh các quy tắc đã được tải:

1. Trong cửa sổ Amazon Q Chat, nhấp vào nút `Rules` ở góc dưới bên phải.
2. Xác nhận bạn thấy các mục nhập cho `.amazonq/rules/aws-aidlc-rules`.

<img src="./assets/images/q-ide-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Q Developer IDE plugin" width="700" height="400">

---

### Cursor IDE

AI-DLC sử dụng [Cursor Rules](https://cursor.com/docs/context/rules) để triển khai quy trình làm việc thông minh của nó.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

#### Tùy chọn 1: Quy tắc Dự án (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
mkdir -p .cursor/rules

cat > .cursor/rules/ai-dlc-workflow.mdc << 'EOF'
---
description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
alwaysApply: true
---

EOF
cat ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md >> .cursor/rules/ai-dlc-workflow.mdc

mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".cursor\rules"

$frontmatter = @"
---
description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
alwaysApply: true
---

"@
$frontmatter | Out-File -FilePath ".cursor\rules\ai-dlc-workflow.mdc" -Encoding utf8

Get-Content "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" | Add-Content ".cursor\rules\ai-dlc-workflow.mdc"

New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .cursor\rules

(
echo ---
echo description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
echo alwaysApply: true
echo ---
echo.
) > .cursor\rules\ai-dlc-workflow.mdc

type "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" >> .cursor\rules\ai-dlc-workflow.mdc

mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: AGENTS.md (Thay thế đơn giản)

**Unix/Linux/macOS:**

```bash
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md ./AGENTS.md
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Mở **Cursor Settings → Rules, Commands**
2. Dưới phần **Project Rules**, bạn sẽ thấy `ai-dlc-workflow` được liệt kê
3. Đối với `AGENTS.md`, nó sẽ tự động được phát hiện và áp dụng

![AI-DLC Rules in Cursor](./assets/images/cursor-ide-aidlc-rules-loaded.png?raw=true 'AI-DLC Rules in Cursor')

**Cấu trúc Thư mục (Tùy chọn 1):**

```
<my-project>/
├── .cursor/
│   └── rules/
│       └── ai-dlc-workflow.mdc
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### Cline

AI-DLC sử dụng Cline Rules để triển khai quy trình làm việc thông minh của nó.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

#### Tùy chọn 1: Thư mục .clinerules (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
mkdir -p .clinerules
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md .clinerules/
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".clinerules"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".clinerules\"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .clinerules
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".clinerules\"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: AGENTS.md (Thay thế)

**Unix/Linux/macOS:**

```bash
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md ./AGENTS.md
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Trong giao diện trò chuyện của Cline, hãy tìm popover Rules bên dưới trường nhập văn bản trò chuyện
2. Xác nhận rằng `core-workflow.md` được liệt kê và đang hoạt động
3. Bạn có thể bật/tắt tệp quy tắc khi cần thiết

![AI-DLC Rules in Cline](./assets/images/cline-ide-aidlc-rules-loaded.png?raw=true 'AI-DLC Rules in Cline')

**Cấu trúc Thư mục (Tùy chọn 1):**

```
<my-project>/
├── .clinerules/
│   └── core-workflow.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### Claude Code

AI-DLC sử dụng tệp bộ nhớ dự án của Claude Code (`CLAUDE.md`) để triển khai quy trình làm việc thông minh của nó.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

#### Tùy chọn 1: Thư mục gốc Dự án (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md ./CLAUDE.md
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\CLAUDE.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\CLAUDE.md"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: Thư mục .claude

**Unix/Linux/macOS:**

```bash
mkdir -p .claude
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md .claude/CLAUDE.md
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".claude"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".claude\CLAUDE.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .claude
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".claude\CLAUDE.md"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Khởi động Claude Code trong thư mục dự án của bạn (CLI: `claude` hoặc VS Code extension)
2. Sử dụng lệnh `/config` để xem cấu hình hiện tại
3. Hỏi Claude: "What instructions are currently active in this project?"

**Cấu trúc Thư mục (Tùy chọn 1):**

```
<my-project>/
├── CLAUDE.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### GitHub Copilot

AI-DLC sử dụng [GitHub Copilot custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions) để triển khai quy trình làm việc thông minh của nó. Tệp `.github/copilot-instructions.md` được tự động phát hiện và áp dụng cho tất cả các yêu cầu chat trong workspace.

Các lệnh bên dưới giả định bạn đã giải nén tệp zip vào thư mục `Downloads`. Nếu bạn sử dụng vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

**Unix/Linux/macOS:**

```bash
mkdir -p .github
cp ~/Downloads/aidlc-rules/aws-aidlc-rules/core-workflow.md .github/copilot-instructions.md
mkdir -p .aidlc-rule-details
cp -R ~/Downloads/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".github"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".github\copilot-instructions.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "$env:USERPROFILE\Downloads\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .github
copy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".github\copilot-instructions.md"
mkdir .aidlc-rule-details
xcopy "%USERPROFILE%\Downloads\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Mở VS Code với thư mục dự án của bạn
2. Mở bảng điều khiển Copilot Chat (Cmd/Ctrl+Shift+I)
3. Chọn **Configure Chat** (biểu tượng bánh răng) > **Chat Instructions** và xác nhận rằng `copilot-instructions` được liệt kê
4. Ngoài ra, gõ `/instructions` trong đầu vào chat để xem các hướng dẫn hiện đang hoạt động

**Cấu trúc Thư mục:**

```
<my-project>/
├── .github/
│   └── copilot-instructions.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### Các Agent Khác

AI-DLC hoạt động với bất kỳ công cụ lập trình AI nào hỗ trợ các quy tắc mức độ dự án hoặc tệp steering. Cách tiếp cận chung là:

1. Đặt `aws-aidlc-rules/` ở bất cứ đâu agent của bạn đọc các quy tắc dự án từ đó (tham khảo tài liệu của agent của bạn).
2. Đặt `aws-aidlc-rule-details/` ở cùng cấp độ ngang hàng để các quy tắc có thể tham chiếu nó.

Nếu agent của bạn không có quy ước cho các tệp quy tắc, hãy đặt cả hai thư mục ở thư mục gốc của dự án và trỏ agent của bạn đến `aws-aidlc-rules/` làm thư mục quy tắc của nó.

---

## Cách sử dụng

1. Bắt đầu bất kỳ dự án phát triển phần mềm nào bằng cách nêu ý định của bạn bắt đầu bằng cụm từ **"Using AI-DLC, ..."** trong cuộc trò chuyện
2. Quy trình làm việc AI-DLC tự động kích hoạt và hướng dẫn bạn từ đó
3. Trả lời các câu hỏi có cấu trúc mà AI-DLC hỏi bạn
4. Xem xét cẩn thận mọi kế hoạch mà AI tạo ra. Cung cấp sự giám sát và xác thực của bạn
5. Xem xét kế hoạch thực thi để xem giai đoạn nào sẽ chạy
6. Xem xét cẩn thận các artifact và phê duyệt từng giai đoạn để duy trì quyền kiểm soát
7. Tất cả các artifact sẽ được tạo trong thư mục `aidlc-docs/`

---

## Quy trình làm việc Thích ứng Ba Giai đoạn

AI-DLC tuân theo cách tiếp cận ba giai đoạn có cấu trúc thích ứng với độ phức tạp của dự án của bạn:

### 🔵 GIAI ĐOẠN KHỞI TẠO (INCEPTION PHASE)

Xác định **CÁI GÌ** cần xây dựng và **TẠI SAO**

- Phân tích và xác thực yêu cầu
- Tạo user story (khi áp dụng)
- Thiết kế Ứng dụng và tạo các đơn vị công việc cho phát triển song song
- Đánh giá rủi ro và đánh giá độ phức tạp

### 🟢 GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION PHASE)

Xác định **CÁCH** xây dựng nó

- Thiết kế thành phần chi tiết
- Tạo mã và triển khai
- Cấu hình xây dựng và chiến lược kiểm thử
- Đảm bảo chất lượng và xác thực

### 🟡 GIAI ĐOẠN VẬN HÀNH (OPERATIONS PHASE)

Triển khai và giám sát (tương lai)

- Tự động hóa triển khai và cơ sở hạ tầng
- Thiết lập giám sát và khả năng quan sát
- Xác thực sẵn sàng cho sản xuất

---

## Các Tính năng Chính

| Tính năng                      | Mô tả                                                                                                                   |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| **Trí tuệ Thích ứng**          | Chỉ thực thi các giai đoạn thêm giá trị cho yêu cầu cụ thể của bạn                                                      |
| **Nhận thức Ngữ cảnh**         | Phân tích codebase hiện có và các yêu cầu phức tạp                                                                      |
| **Dựa trên Rủi ro**            | Thay đổi phức tạp nhận được sự xử lý toàn diện, thay đổi đơn giản giữ hiệu quả                                          |
| **Dựa trên Câu hỏi**           | Các câu hỏi trắc nghiệm có cấu trúc trong tệp, không phải trò chuyện                                                    |
| **Luôn trong Quyền kiểm soát** | Xem xét kế hoạch thực thi và phê duyệt từng giai đoạn                                                                   |
| **Có thể mở rộng**             | Thêm các quy tắc tùy chỉnh (ví dụ: bảo mật, tuân thủ và quy tắc cụ thể của tổ chức) lên trên quy trình làm việc cốt lõi |

---

## Phần mở rộng

AI-DLC hỗ trợ hệ thống phần mở rộng cho phép bạn thêm các quy tắc bổ sung lên trên quy trình làm việc cốt lõi. Các phần mở rộng là các tệp markdown được tổ chức trong thư mục `aws-aidlc-rule-details/extensions/` và được tự động tải cũng như áp dụng khi được kích hoạt trong giai đoạn Phân tích Yêu cầu (Requirements Analysis).

### Cách Phần mở rộng Hoạt động

Các phần mở rộng được phân loại theo nhóm (ví dụ: `security/`, `scalability/`, `accessibility/`). Mỗi nhóm có thể chứa các quy tắc riêng và bất kỳ số lượng thư mục con nào bạn xác định.

Mỗi phần mở rộng nên bao gồm một **Câu hỏi Khả năng áp dụng (Applicability Question)** — một câu hỏi trắc nghiệm có cấu trúc mà AI-DLC tự động hiển thị trong giai đoạn Phân tích Yêu cầu. Điều này cho phép người dùng quyết định xem có kích hoạt hay bỏ qua phần mở rộng đó cho dự án hiện tại hay không. Ví dụ, phần mở rộng bảo mật có sẵn bao gồm:

```markdown
## Question: Security Extensions

Should security extension rules be enforced for this project?

A) Yes — enforce all SECURITY rules as blocking constraints
B) No — skip all SECURITY rules
X) Other (please describe)

[Answer]:
```

Khi bạn tạo các phần mở rộng của riêng mình, hãy bao gồm một câu hỏi khả năng áp dụng tương tự để người dùng có thể chọn tham gia hoặc không tham gia trên từng dự án.

Dưới đây là luồng hoạt động chung sau khi một phần mở rộng được kích hoạt:

1. Trong giai đoạn Khởi tạo (Inception), AI-DLC hiển thị câu hỏi khả năng áp dụng của phần mở rộng.
2. Nếu được kích hoạt, các quy tắc của phần mở rộng được tải dưới dạng các ràng buộc bắt buộc áp dụng trên tất cả các giai đoạn của AI-DLC.
3. Ở mỗi giai đoạn, AI model sẽ xác minh việc tuân thủ tất cả các quy tắc mở rộng đã tải trước khi cho phép tiến hành giai đoạn đó.

### Cấu trúc Thư mục Phần mở rộng

Quy trình làm việc hiện được cung cấp kèm theo một phần mở rộng bảo mật cơ bản.

```text
aws-aidlc-rule-details/
└── extensions/
    └── security/                      # Danh mục phần mở rộng
        └── baseline/
        │   └── security-baseline.md   # Các quy tắc bảo mật cơ bản
        ├── compliance/                # Cấu trúc thư mục được đề xuất
        │   ├── hipaa/                 # Quy tắc tuân thủ HIPAA
        │   ├── pci-dss/               # Quy tắc tuân thủ PCI-DSS
        │   └── soc2/                  # Quy tắc tuân thủ SOC 2
        └── internal-policies/         # Các quy tắc tùy chỉnh của tổ chức bạn
```

### Thêm Phần mở rộng Của riêng Bạn

Bạn có thể mở rộng một danh mục hiện có hoặc tạo một danh mục hoàn toàn mới.

Để thêm các quy tắc vào một danh mục hiện có (ví dụ: `security/`):

1. Tạo một thư mục mới dưới `extensions/security/` (ví dụ: `compliance/hipaa/`).
2. Thêm một hoặc nhiều tệp markdown với các quy tắc của bạn. Tuân theo cấu trúc giống như `security-baseline.md`:
   - Cung cấp cho mỗi quy tắc một ID duy nhất.
   - Bao gồm một **Câu hỏi Khả năng áp dụng** được mô tả ở trên.
   - Bao gồm phần **Quy tắc** mô tả các yêu cầu.
   - Bao gồm phần **Xác minh** với các tiêu chí cụ thể mà model cần đánh giá.
3. Các quy tắc mặc định là chặn (blocking) — nếu tiêu chí xác minh không được đáp ứng, giai đoạn không thể tiếp tục cho đến khi phát hiện được giải quyết.

Để tạo một danh mục phần mở rộng mới, hãy thêm một thư mục mới dưới `extensions/` (ví dụ: `extensions/performance/`) và đặt các tệp markdown quy tắc của bạn bên trong đó theo cùng một định dạng.

---

## Nguyên tắc

Đây là những nguyên tắc cốt lõi của chúng tôi để hướng dẫn việc ra quyết định.

- **Không trùng lặp**. Nguồn sự thật (source of truth) nằm ở một nơi. Nếu chúng tôi hỗ trợ các công cụ hoặc định dạng mới yêu cầu tệp cụ thể, chúng tôi tạo chúng từ nguồn thay vì duy trì các bản sao riêng biệt.

- **Phương pháp luận là ưu tiên hàng đầu**. AI-DLC về cơ bản là một phương pháp luận, không phải là một công cụ. Người dùng không cần cài đặt bất cứ thứ gì để bắt đầu. Tuy nhiên, chúng tôi cởi mở với các công cụ tiện lợi (scripts, CLIs) trong tương lai nếu nó giúp người dùng áp dụng hoặc mở rộng phương pháp luận.

- **Có thể tái tạo**. Các quy tắc phải đủ rõ ràng để các mô hình khác nhau tạo ra kết quả tương tự. Chúng tôi biết các mô hình hoạt động khác nhau, nhưng phương pháp luận nên giảm thiểu sự sai lệch thông qua hướng dẫn rõ ràng.

- **Bất khả tri (Agnostic)**. Phương pháp luận hoạt động với bất kỳ IDE, agent, hoặc model nào. Chúng tôi không ràng buộc mình với các công cụ hoặc nhà cung cấp cụ thể.

- **Con người trong vòng lặp**. Các quyết định quan trọng đòi hỏi sự xác nhận rõ ràng của người dùng. Agent đề xuất, con người phê duyệt.

---

## Điều kiện Tiên quyết

Có sẵn một trong các nền tảng/công cụ được hỗ trợ của chúng tôi cho Lập trình Hỗ trợ bởi AI (Assisted AI Coding) được cài đặt:

| Nền tảng                      | Liên kết Cài đặt                                                                                                                                                |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kiro                          | [Cài đặt](https://kiro.dev/)                                                                                                                                    |
| Kiro CLI                      | [Cài đặt](https://kiro.dev/cli/)                                                                                                                                |
| Amazon Q Developer IDE Plugin | [Cài đặt](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html)                                                                               |
| Cursor IDE                    | [Cài đặt](https://cursor.com/)                                                                                                                                  |
| Cline VS Code Extension       | [Cài đặt](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev)                                                                           |
| Claude Code CLI               | [Cài đặt](https://github.com/anthropics/claude-code)                                                                                                            |
| GitHub Copilot                | [Cài đặt](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) + [Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) |

---

## Khắc phục sự cố

### Các Vấn đề Chung

| Vấn đề                            | Giải pháp                                                   |
| --------------------------------- | ----------------------------------------------------------- |
| Quy tắc không tải                 | Kiểm tra tệp tồn tại đúng vị trí cho nền tảng của bạn       |
| Vấn đề mã hóa tệp                 | Đảm bảo các tệp được mã hóa UTF-8                           |
| Quy tắc không áp dụng trong phiên | Bắt đầu một phiên trò chuyện mới sau khi thay đổi tệp       |
| Chi tiết quy tắc không tải        | Xác minh `.aidlc-rule-details/` tồn tại với các thư mục con |

### Các Vấn đề Cụ thể theo Nền tảng

#### Amazon Q Developer / Kiro

- Sử dụng `/context show` để xác minh các quy tắc đã được tải
- Kiểm tra cấu trúc thư mục `.amazonq/rules/` hoặc `.kiro/steering/`

#### Cursor

- Đối với "Apply Intelligently", đảm bảo mô tả được định nghĩa trong frontmatter
- Kiểm tra **Cursor Settings → Rules** để đảm bảo quy tắc được bật
- Nếu quy tắc quá lớn (>500 dòng), hãy chia thành nhiều quy tắc tập trung

#### Cline

- Kiểm tra popover Rules bên dưới trường nhập văn bản trò chuyện
- Bật/tắt tệp quy tắc khi cần thiết bằng giao diện người dùng popover

#### Claude Code

- Sử dụng lệnh `/config` để xem cấu hình hiện tại
- Hỏi "What instructions are currently active in this project?"

#### GitHub Copilot

- Chọn **Configure Chat** (biểu tượng bánh răng) > **Chat Instructions** để xác minh các hướng dẫn đã được tải
- Gõ `/instructions` trong vùng nhập trò chuyện để xem các tệp hướng dẫn đang hoạt động
- Kiểm tra rằng `.github/copilot-instructions.md` tồn tại trong thư mục gốc workspace của bạn

### Các Vấn đề Đường dẫn Tệp trên Windows

- Sử dụng dấu gạch chéo `/` trong đường dẫn tệp bên trong các tệp markdown
- Đường dẫn Windows với dấu gạch ngược có thể không hoạt động chính xác

---

## Khuyến nghị Kiểm soát Phiên bản

**Commit vào repository:**

```gitignore
# These should be version controlled
CLAUDE.md
AGENTS.md
.amazonq/rules/
.kiro/steering/
.cursor/rules/
.clinerules/
.github/copilot-instructions.md
.aidlc-rule-details/
```

**Tùy chọn - Thêm vào `.gitignore` (nếu cần):**

```gitignore
# Local-only settings
.claude/settings.local.json
```

---

## Tài nguyên Bổ sung

| Tài nguyên                             | Liên kết                                                                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Tài liệu Định nghĩa Phương pháp AI-DLC | [Paper](https://prod.d13rzhkk8cj2z0.amplifyapp.com/)                                                                          |
| Blog Phương pháp luận AI-DLC           | [AWS Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)                                             |
| Blog Ra mắt Mã nguồn mở AI-DLC         | [AWS Blog](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/) |
| Blog Hướng dẫn Ví dụ AI-DLC            | [AWS Blog](https://aws.amazon.com/blogs/devops/building-with-ai-dlc-using-amazon-q-developer/)                                |
| Tài liệu Amazon Q Developer            | [Docs](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html)                                                |
| Tài liệu Kiro CLI                      | [Docs](https://kiro.dev/docs/cli/steering/)                                                                                   |
| Tài liệu Cursor Rules                  | [Docs](https://cursor.com/docs/context/rules)                                                                                 |
| Tài liệu Claude Code                   | [GitHub](https://github.com/anthropics/claude-code)                                                                           |
| Tài liệu GitHub Copilot                | [Docs](https://docs.github.com/en/copilot)                                                                                    |
| Hướng dẫn Đóng góp                     | [CONTRIBUTING.md](CONTRIBUTING_VN.md)                                                                                         |
| Quy tắc Ứng xử                         | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT_VN.md)                                                                                   |

---

## Bảo mật

Xem [CONTRIBUTING](CONTRIBUTING_VN.md#security-issue-notifications) để biết thêm thông tin.

## Giấy phép

Thư viện này được cấp phép theo Giấy phép MIT-0. Xem tệp LICENSE.
