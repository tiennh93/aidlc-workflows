# AI-DLC (Vòng đời Phát triển Dựa trên AI)

AI-DLC là một quy trình phát triển phần mềm thông minh thích ứng với nhu cầu của bạn, duy trì các tiêu chuẩn chất lượng và giữ cho bạn quyền kiểm soát quy trình. Để tìm hiểu thêm về Phương pháp luận AI-DLC, hãy đọc [bài đăng trên blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) này và [Tài liệu Định nghĩa Phương pháp](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) được đề cập trong đó.

## Bắt đầu Nhanh

1. Tải xuống tệp zip bản phát hành mới nhất từ [Trang phát hành](../../releases/latest) vào một thư mục **bên ngoài** thư mục dự án của bạn (ví dụ: `~/Downloads`).
2. Giải nén tệp zip. Nó chứa một thư mục `aidlc-rules/` với hai thư mục con:
   - `aws-aidlc-rules/` — các quy tắc quy trình làm việc AI-DLC cốt lõi
   - `aws-aidlc-rule-details/` — tài liệu hỗ trợ được tham chiếu bởi các quy tắc
3. Sao chép cả hai thư mục vào dự án của bạn, tuân theo thiết lập cho nền tảng của bạn bên dưới.

> **Lưu ý**: Thư mục được giải nén có thể chứa một thư mục cấp cao nhất (ví dụ: `aidlc-workflows-0.1.0/`). Hãy điều hướng vào đó trước để `aidlc-rules/` có thể truy cập trực tiếp.

## Kiro

AI-DLC sử dụng [Kiro Steering Files](https://kiro.dev/docs/cli/steering/) trong không gian làm việc dự án của bạn. Sao chép các quy tắc vào thư mục `.kiro` của dự án của bạn:

1. Tạo các thư mục `.kiro/steering` và `.kiro/aws-aidlc-rule-details` trong thư mục gốc dự án của bạn.
2. Sao chép `aws-aidlc-rules/` vào `.kiro/steering/`.
3. Sao chép `aws-aidlc-rule-details/` vào `.kiro/`.

Các lệnh dưới đây giả định bạn đã giải nén tệp zip vào thư mục `Downloads` của mình. Nếu bạn đã sử dụng một vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

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

### Kiro IDE

Mở bảng steering files và xác nhận bạn thấy một mục nhập cho `core-workflow` trong phần `Workspace` như hiển thị trong ảnh chụp màn hình bên dưới.

<img src="./assets/images/kiro-ide-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Kiro IDE" width="700" height="450">

Chúng tôi sử dụng Kiro IDE ở chế độ Vibe để chạy quy trình làm việc AI-DLC. Điều này đảm bảo rằng quy trình làm việc AI-DLC hướng dẫn quy trình phát triển trong Kiro. Đôi khi, Kiro có thể nhắc bạn chuyển sang chế độ spec. Chọn `No` cho các lời nhắc như vậy để ở lại chế độ Vibe.

<img src="./assets/images/kiro-sdd-nudge.png" alt="Staying in Kiro Vibe mode" width="500" height="175">

### Kiro CLI

Chạy `kiro-cli`, sau đó `/context show`, và xác nhận các mục nhập cho `.kiro/steering/aws-aidlc-rules`.

<img src="./assets/images/kiro-cli-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Kiro CLI" width="700" height="660">

## Amazon Q Developer IDE Plugin/Extension

AI-DLC sử dụng [Amazon Q Rules](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/context-project-rules.html) trong không gian làm việc dự án của bạn. Sao chép các quy tắc vào thư mục `.amazonq` của dự án của bạn:

1. Tạo các thư mục `.amazonq/rules` và `.amazonq/aws-aidlc-rule-details` trong thư mục gốc dự án của bạn.
2. Sao chép `aws-aidlc-rules/` vào `.amazonq/rules/`.
3. Sao chép `aws-aidlc-rule-details/` vào `.amazonq/`.

Các lệnh dưới đây giả định bạn đã giải nén tệp zip vào thư mục `Downloads` của mình. Nếu bạn đã sử dụng một vị trí khác, hãy thay thế `Downloads` bằng đường dẫn thư mục thực tế của bạn.

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

1. Trong cửa sổ Amazon Q Chat, nhấp vào nút `Rules` ở góc dưới cùng bên phải.
2. Xác nhận bạn thấy các mục nhập cho `.amazonq/rules/aws-aidlc-rules`.

<img src="./assets/images/q-ide-aidlc-rules-loaded.png?raw=true" alt="AI-DLC Rules in Q Developer IDE plugin" width="700" height="400">

### Các Agent Khác

AI-DLC hoạt động với bất kỳ coding agent nào hỗ trợ các quy tắc cấp dự án hoặc tệp steering. Cách tiếp cận chung:

1. Đặt `aws-aidlc-rules/` ở bất cứ nơi nào agent của bạn đọc các quy tắc dự án (tham khảo tài liệu của agent của bạn).
2. Đặt `aws-aidlc-rule-details/` ở cấp độ ngang hàng để các quy tắc có thể tham chiếu đến nó.

Nếu agent của bạn không có quy ước cho các tệp quy tắc, hãy đặt cả hai thư mục tại thư mục gốc dự án của bạn và trỏ agent đến `aws-aidlc-rules/` làm thư mục quy tắc của nó.

### Cách sử dụng

1. Bắt đầu bất kỳ dự án phát triển phần mềm nào bằng cách nêu ý định của bạn bắt đầu bằng cụm từ "Using AI-DLC, ..." trong cuộc trò chuyện.
2. Quy trình làm việc AI-DLC tự động kích hoạt và hướng dẫn bạn từ đó.
3. Trả lời các câu hỏi có cấu trúc mà AI-DLC hỏi bạn.
4. Xem xét cẩn thận mọi kế hoạch mà AI tạo ra. Cung cấp sự giám sát và xác thực của bạn.
5. Xem xét kế hoạch thực thi để xem giai đoạn nào sẽ chạy.
6. Xem xét cẩn thận các artifact và phê duyệt từng giai đoạn để duy trì quyền kiểm soát.
7. Tất cả các artifact sẽ được tạo trong thư mục `aidlc-docs/`.

## Quy trình làm việc Thích ứng Ba Giai đoạn

AI-DLC tuân theo cách tiếp cận ba giai đoạn có cấu trúc thích ứng với độ phức tạp của dự án của bạn:

- **🔵 GIAI ĐOẠN KHỞI TẠO (INCEPTION PHASE)**: Xác định **CÁI GÌ** cần xây dựng và **TẠI SAO**
  - Phân tích và xác thực yêu cầu
  - Tạo user story (khi áp dụng)
  - Thiết kế Ứng dụng và tạo các đơn vị công việc cho phát triển song song
  - Đánh giá rủi ro và đánh giá độ phức tạp

- **🟢 GIAI ĐOẠN XÂY DỰNG (CONSTRUCTION PHASE)**: Xác định **CÁCH** xây dựng nó
  - Thiết kế thành phần chi tiết
  - Tạo mã và triển khai
  - Cấu hình xây dựng và chiến lược kiểm thử
  - Đảm bảo chất lượng và xác thực

- **🟡 GIAI ĐOẠN VẬN HÀNH (OPERATIONS PHASE)**: Triển khai và giám sát (tương lai)
  - Tự động hóa triển khai và cơ sở hạ tầng
  - Thiết lập giám sát và khả năng quan sát
  - Xác thực sẵn sàng cho sản xuất

## Các Tính năng Chính

- **Trí tuệ Thích ứng**: Chỉ thực thi các giai đoạn thêm giá trị cho yêu cầu cụ thể của bạn
- **Nhận thức Ngữ cảnh**: Phân tích codebase hiện có và các yêu cầu phức tạp
- **Dựa trên Rủi ro**: Thay đổi phức tạp nhận được sự xử lý toàn diện, thay đổi đơn giản giữ hiệu quả
- **Dựa trên Câu hỏi**: Các câu hỏi trắc nghiệm có cấu trúc trong tệp, không phải trò chuyện
- **Luôn trong Quyền kiểm soát**: Xem xét kế hoạch thực thi và phê duyệt từng giai đoạn

## Điều kiện Tiên quyết

Cài đặt một trong các nền tảng/công cụ được hỗ trợ của chúng tôi cho Assisted AI Coding:

- [Kiro IDE](https://kiro.dev/)
- [Kiro CLI](https://kiro.dev/cli/)
- [Amazon Q Developer IDE plugin](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html)

## Nguyên tắc

Đây là những nguyên tắc cốt lõi của chúng tôi để hướng dẫn việc ra quyết định.

- **Không trùng lặp**. Nguồn sự thật sống ở một nơi. Nếu chúng tôi hỗ trợ các công cụ hoặc định dạng mới yêu cầu tệp cụ thể, chúng tôi tạo chúng từ nguồn thay vì duy trì các bản sao riêng biệt.

- **Phương pháp luận đầu tiên**. AI-DLC về cơ bản là một phương pháp luận, không phải là một công cụ. Người dùng không cần cài đặt bất cứ thứ gì để bắt đầu. Tuy nhiên, chúng tôi cởi mở với các công cụ tiện lợi (scripts, CLIs) trong tương lai nếu nó giúp người dùng áp dụng hoặc mở rộng phương pháp luận.

- **Có thể tái tạo**. Các quy tắc phải đủ rõ ràng để các mô hình khác nhau tạo ra kết quả tương tự. Chúng tôi biết các mô hình hoạt động khác nhau, nhưng phương pháp luận nên giảm thiểu phương sai thông qua hướng dẫn rõ ràng.

- **Bất khả tri (Agnostic)**. Phương pháp luận hoạt động với bất kỳ IDE, agent, hoặc model nào. Chúng tôi không ràng buộc mình với các công cụ hoặc nhà cung cấp cụ thể.

- **Con người trong vòng lặp**. Các quyết định quan trọng đòi hỏi sự xác nhận rõ ràng của người dùng. Agent đề xuất, con người phê duyệt.

## Bảo mật

Xem [CONTRIBUTING](CONTRIBUTING_VN.md#security-issue-notifications) để biết thêm thông tin.

## Giấy phép

Thư viện này được cấp phép theo Giấy phép MIT-0. Xem tệp LICENSE.
