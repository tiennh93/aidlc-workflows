# AI-DLC (Vòng đời Phát triển Dựa trên AI)

AI-DLC là một quy trình phát triển phần mềm thông minh thích ứng với nhu cầu của bạn, duy trì các tiêu chuẩn chất lượng và giữ cho bạn quyền kiểm soát quy trình. Để tìm hiểu thêm về Phương pháp luận AI-DLC, hãy đọc [bài đăng trên blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) này và [Tài liệu Định nghĩa Phương pháp](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) được tham chiếu trong đó.

## Mục lục

- [Nguyên tắc](#nguyên-tắc)
- [Điều kiện Tiên quyết](#điều-kiện-tiên-quyết)
- [Tải AIDLC](#tải-aidlc)
- [Cài đặt theo Nền tảng](#cài-đặt-theo-nền-tảng)
- [Cách sử dụng](#cách-sử-dụng)
- [Quy trình làm việc Thích ứng Ba Giai đoạn](#quy-trình-làm-việc-thích-ứng-ba-giai-đoạn)
- [Các Tính năng Chính](#các-tính-năng-chính)
- [Khắc phục sự cố](#khắc-phục-sự-cố)
- [Tài nguyên Bổ sung](#tài-nguyên-bổ-sung)

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

Cài đặt một trong các nền tảng/công cụ được hỗ trợ của chúng tôi cho Lập trình Hỗ trợ bởi AI (Assisted AI Coding):

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

## Tải AIDLC

### Từ Tệp Zip Đóng gói

1. Tải xuống tệp zip bản phát hành mới nhất (ví dụ: `ai-dlc-rules-v1.0.0.zip`) từ [Trang phát hành](../../releases/latest) vào một thư mục **bên ngoài** thư mục dự án của bạn (ví dụ: `~/Downloads`).
2. Giải nén tệp zip. Nó chứa một thư mục `aidlc-rules/` với hai thư mục con:
   - `aws-aidlc-rules/` — các quy tắc quy trình làm việc AI-DLC cốt lõi
   - `aws-aidlc-rule-details/` — tài liệu hỗ trợ được tham chiếu bởi các quy tắc
3. Lưu ý đường dẫn đến thư mục `aidlc-rules/` đã giải nén — bạn sẽ cần nó trong các lệnh cài đặt theo nền tảng bên dưới.

> **Mẹo**: Tải xuống **artifact phát hành** (có tên `ai-dlc-rules-vX.X.X.zip`), không phải tệp lưu trữ "Source code" tự động tạo. Artifact phát hành chứa trực tiếp `aidlc-rules/`, trong khi tệp lưu trữ nguồn bao bọc mọi thứ trong một thư mục bổ sung.

---

### Clone từ Repository

#### Bước 1: Clone Repository này

```bash
git clone <this-repo>
```

#### Bước 2: Tạo một Thư mục Dự án Mới

**Unix/Linux/macOS:**

```bash
mkdir <my-project>
cd <my-project>
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Name "<my-project>"
Set-Location "<my-project>"
```

**Windows CMD:**

```cmd
mkdir <my-project>
cd <my-project>
```

#### Bước 3: Làm theo Cài đặt theo Nền tảng

Chọn nền tảng của bạn bên dưới và làm theo hướng dẫn cài đặt.

---

## Cài đặt theo Nền tảng

- [Amazon Q Developer IDE Plugin](#amazon-q-developer-ide-pluginextension)
- [Kiro CLI](#kiro-cli-formerly-amazon-q-cli)
- [Cursor IDE](#cursor-ide)
- [Cline](#cline)
- [Claude Code](#claude-code)
- [GitHub Copilot](#github-copilot)

> **Người dùng ZIP**: Các lệnh bên dưới sử dụng `../aidlc-workflows/aidlc-rules` (Unix) và `..\aidlc-workflows\aidlc-rules` (Windows) làm đường dẫn nguồn, giả định bố cục **clone**. Nếu bạn đã tải xuống **ZIP**, hãy thay thế đường dẫn đó bằng vị trí thư mục `aidlc-rules` đã giải nén của bạn (ví dụ: `~/Downloads/aidlc-rules` hoặc `%USERPROFILE%\Downloads\aidlc-rules`).

---

### Amazon Q Developer IDE Plugin/Extension

AI-DLC sử dụng [Amazon Q Rules](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/context-project-rules.html) để triển khai quy trình làm việc thông minh của nó.

**Unix/Linux/macOS:**

```bash
mkdir -p .amazonq/rules
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rules .amazonq/rules/
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".amazonq\rules"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules" ".amazonq\rules\" -Recurse
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .amazonq\rules
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules" ".amazonq\rules\" /E /I
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Trong cửa sổ Amazon Q Chat, xác định vị trí nút `Rules` ở góc dưới cùng bên phải
2. Xác nhận rằng bạn thấy các mục nhập cho `.amazonq/rules/aws-aidlc-rules` trong danh sách hiển thị

![AI-DLC Rules in Q Developer IDE](./assets/images/q-ide-aidlc-rules-loaded.png?raw=true 'AI-DLC Rules in Q Developer')

**Cấu trúc Thư mục:**

```
<my-project>/
├── .amazonq/
│   └── rules/
│       └── aws-aidlc-rules/
│           └── core-workflow.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### Kiro CLI (trước đây là Amazon Q CLI)

AI-DLC sử dụng [Kiro Steering Files](https://kiro.dev/docs/cli/steering/) để triển khai quy trình làm việc thông minh của nó.

**Unix/Linux/macOS:**

```bash
mkdir -p .kiro/steering
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rules .kiro/steering/
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".kiro\steering"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules" ".kiro\steering\" -Recurse
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .kiro\steering
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules" ".kiro\steering\" /E /I
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Khởi động Kiro CLI: `kiro-cli`
2. Kiểm tra nội dung ngữ cảnh của bạn: `/context show`
3. Xác nhận rằng bạn thấy tất cả các mục nhập cho `.kiro/steering/aws-aidlc-rules`

![AI-DLC Rules in Kiro CLI](./assets/images/kiro-cli-aidlc-rules-loaded.png?raw=true 'AI-DLC Rules in Kiro CLI')

**Cấu trúc Thư mục:**

```
<my-project>/
├── .kiro/
│   └── steering/
│       └── aws-aidlc-rules/
│           └── core-workflow.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

---

### Cursor IDE

AI-DLC sử dụng [Cursor Rules](https://cursor.com/docs/context/rules) để triển khai quy trình làm việc thông minh của nó.

#### Tùy chọn 1: Quy tắc Dự án (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
# Create .cursor/rules directory
mkdir -p .cursor/rules

# Create .mdc file with frontmatter and workflow content
cat > .cursor/rules/ai-dlc-workflow.mdc << 'EOF'
---
description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
alwaysApply: true
---

EOF
cat ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md >> .cursor/rules/ai-dlc-workflow.mdc

# Copy rule details to .aidlc-rule-details (loaded on-demand by the workflow)
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
# Create .cursor/rules directory
New-Item -ItemType Directory -Force -Path ".cursor\rules"

# Create frontmatter and write to .mdc file
$frontmatter = @"
---
description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
alwaysApply: true
---

"@
$frontmatter | Out-File -FilePath ".cursor\rules\ai-dlc-workflow.mdc" -Encoding utf8

# Append core workflow content to .mdc file
Get-Content "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" | Add-Content ".cursor\rules\ai-dlc-workflow.mdc"

# Copy rule details to .aidlc-rule-details (loaded on-demand by the workflow)
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
REM Create .cursor/rules directory
mkdir .cursor\rules

REM Create frontmatter in .mdc file
(
echo ---
echo description: "AI-DLC (AI-Driven Development Life Cycle) adaptive workflow for software development"
echo alwaysApply: true
echo ---
echo.
) > .cursor\rules\ai-dlc-workflow.mdc

REM Append core workflow content to .mdc file
type "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" >> .cursor\rules\ai-dlc-workflow.mdc

REM Copy rule details to .aidlc-rule-details (loaded on-demand by the workflow)
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: AGENTS.md (Giải pháp thay thế đơn giản)

**Unix/Linux/macOS:**

```bash
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md ./AGENTS.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Mở **Cursor Settings → Rules, Commands**
2. Dưới **Project Rules**, bạn sẽ thấy `ai-dlc-workflow` được liệt kê
3. Đối với `AGENTS.md`, nó sẽ được tự động phát hiện và áp dụng

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

#### Tùy chọn 1: Thư mục .clinerules (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
mkdir -p .clinerules
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md .clinerules/
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".clinerules"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".clinerules\"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .clinerules
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".clinerules\"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: AGENTS.md (Thay thế)

**Unix/Linux/macOS:**

```bash
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md ./AGENTS.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\AGENTS.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Trong giao diện trò chuyện của Cline, tìm popover Rules bên dưới trường nhập liệu trò chuyện
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

#### Tùy chọn 1: Thư mục gốc Dự án (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md ./CLAUDE.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\CLAUDE.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\CLAUDE.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: Thư mục .claude

**Unix/Linux/macOS:**

```bash
mkdir -p .claude
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md .claude/CLAUDE.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".claude"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".claude\CLAUDE.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .claude
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".claude\CLAUDE.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
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

AI-DLC sử dụng các tệp ngữ cảnh dự án và khả năng Chat của Copilot để triển khai quy trình làm việc thông minh của nó.

#### Tùy chọn 1: Thư mục .copilot (Khuyên dùng)

**Unix/Linux/macOS:**

```bash
mkdir -p .copilot
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md .copilot/instructions.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
New-Item -ItemType Directory -Force -Path ".copilot"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".copilot\instructions.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
mkdir .copilot
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".copilot\instructions.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

#### Tùy chọn 2: Tệp COPILOT.md tại Thư mục gốc

**Unix/Linux/macOS:**

```bash
cp ../aidlc-workflows/aidlc-rules/aws-aidlc-rules/core-workflow.md ./COPILOT.md
mkdir -p .aidlc-rule-details
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details/* .aidlc-rule-details/
```

**Windows PowerShell:**

```powershell
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\COPILOT.md"
New-Item -ItemType Directory -Force -Path ".aidlc-rule-details"
Copy-Item "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details\*" ".aidlc-rule-details\" -Recurse
```

**Windows CMD:**

```cmd
copy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rules\core-workflow.md" ".\COPILOT.md"
mkdir .aidlc-rule-details
xcopy "..\aidlc-workflows\aidlc-rules\aws-aidlc-rule-details" ".aidlc-rule-details\" /E /I
```

**Xác minh Cài đặt:**

1. Mở VS Code với thư mục dự án của bạn
2. Mở bảng Copilot Chat (Cmd/Ctrl+Shift+I)
3. Tham chiếu hướng dẫn bằng cách gõ `#file .copilot/instructions.md` hoặc `#file COPILOT.md` trong cuộc trò chuyện

**Cấu trúc Thư mục (Tùy chọn 1):**

```
<my-project>/
├── .copilot/
│   └── instructions.md
└── .aidlc-rule-details/
    ├── common/
    ├── inception/
    ├── construction/
    └── operations/
```

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

| Tính năng                      | Mô tả                                                                          |
| ------------------------------ | ------------------------------------------------------------------------------ |
| **Trí tuệ Thích ứng**          | Chỉ thực thi các giai đoạn thêm giá trị cho yêu cầu cụ thể của bạn             |
| **Nhận thức Ngữ cảnh**         | Phân tích codebase hiện có và các yêu cầu phức tạp                             |
| **Dựa trên Rủi ro**            | Thay đổi phức tạp nhận được sự xử lý toàn diện, thay đổi đơn giản giữ hiệu quả |
| **Dựa trên Câu hỏi**           | Các câu hỏi trắc nghiệm có cấu trúc trong tệp, không phải trò chuyện           |
| **Luôn trong Quyền kiểm soát** | Xem xét kế hoạch thực thi và phê duyệt từng giai đoạn                          |

---

## Khắc phục sự cố

### Các vấn đề chung

| Vấn đề                            | Giải pháp                                                   |
| --------------------------------- | ----------------------------------------------------------- |
| Quy tắc không tải                 | Kiểm tra tệp tồn tại đúng vị trí cho nền tảng của bạn       |
| Vấn đề mã hóa tệp                 | Đảm bảo các tệp được mã hóa UTF-8                           |
| Quy tắc không áp dụng trong phiên | Bắt đầu một phiên trò chuyện mới sau khi thay đổi tệp       |
| Chi tiết quy tắc không tải        | Xác minh `.aidlc-rule-details/` tồn tại với các thư mục con |

### Các vấn đề cụ thể theo nền tảng

#### Amazon Q Developer / Kiro CLI

- Sử dụng `/context show` để xác minh các quy tắc đã được tải
- Kiểm tra cấu trúc thư mục `.amazonq/rules/` hoặc `.kiro/steering/`

#### Cursor

- Đối với "Apply Intelligently", đảm bảo mô tả được định nghĩa trong frontmatter
- Kiểm tra **Cursor Settings → Rules** để đảm bảo quy tắc được bật
- Nếu quy tắc quá lớn (>500 dòng), chia thành nhiều quy tắc tập trung

#### Cline

- Kiểm tra popover Rules bên dưới trường nhập liệu trò chuyện
- Bật/tắt các tệp quy tắc khi cần thiết bằng giao diện người dùng popover

#### Claude Code

- Sử dụng lệnh `/config` để xem cấu hình hiện tại
- Hỏi "What instructions are currently active in this project?"

#### GitHub Copilot

- Sử dụng cú pháp `#file <path>` để tham chiếu các tệp hướng dẫn
- Đối với các hướng dẫn lớn, tham chiếu các tệp chi tiết quy tắc cụ thể thay vì dán mọi thứ

### Các vấn đề đường dẫn tệp trên Windows

- Sử dụng dấu gạch chéo `/` trong đường dẫn tệp bên trong các tệp markdown
- Đường dẫn Windows với dấu gạch ngược có thể không hoạt động chính xác

---

## Khuyến nghị Kiểm soát Phiên bản

**Commit vào repository:**

```gitignore
# These should be version controlled
CLAUDE.md
COPILOT.md
AGENTS.md
.amazonq/rules/
.kiro/steering/
.cursor/rules/
.clinerules/
.copilot/
.aidlc-rule-details/
```

**Tùy chọn - Thêm vào `.gitignore` (nếu cần):**

```gitignore
# Local-only settings
.claude/settings.local.json
.copilot/context/
```

---

## Tài nguyên Bổ sung

| Tài nguyên                             | Liên kết                                                                          |
| -------------------------------------- | --------------------------------------------------------------------------------- |
| Blog Phương pháp luận AI-DLC           | [AWS Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) |
| Tài liệu Định nghĩa Phương pháp AI-DLC | [Paper](https://prod.d13rzhkk8cj2z0.amplifyapp.com/)                              |
| Tài liệu Amazon Q Developer            | [Docs](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html)    |
| Tài liệu Kiro CLI                      | [Docs](https://kiro.dev/docs/cli/steering/)                                       |
| Tài liệu Cursor Rules                  | [Docs](https://cursor.com/docs/context/rules)                                     |
| Tài liệu Claude Code                   | [GitHub](https://github.com/anthropics/claude-code)                               |
| Tài liệu GitHub Copilot                | [Docs](https://docs.github.com/en/copilot)                                        |
| Hướng dẫn Đóng góp                     | [CONTRIBUTING.md](CONTRIBUTING_VN.md)                                             |
| Quy tắc Ứng xử                         | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT_VN.md)                                       |

---

## Bảo mật

Xem [CONTRIBUTING](CONTRIBUTING_VN.md#security-issue-notifications) để biết thêm thông tin.

## Giấy phép

Thư viện này được cấp phép theo Giấy phép MIT-0. Xem tệp LICENSE.
