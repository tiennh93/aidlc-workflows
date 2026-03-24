# Khung Đánh giá & Báo cáo Quy trình AI-DLC - Câu hỏi thường gặp (FAQ)

## Đây là gì?

Một nền tảng khung kiểm thử và tự tạo báo cáo khá đầy đủ đảm chốt các khâu thiết kết xác thực đổi nội ở ngay khu trữ của AI-DLC (workflow). Tự khởi cấu hình phân tính chuẩn của phẩm vật bộ code, chuẩn chỉnh từ đồng tính hệ định nghĩa logic semantic đến biên độ mức chạy hoàn hiệu năng (performance) nhằm đảm chốt không xuất rủi do sự kéo hỏng tới kết cấu bên trong hệ thống.

## Công cụ này dùng cho đối tượng nào?

- Những **người duy trì bản (Maintainers)**, ai là những người đòi quyền kiểm chứng chuẩn mức hợp lý, không hiểm họa, bảo an trước hồi định thông lệnh lấy nhập nhánh hợp lệnh. 
- Những **người cộng hiến (Contributors)** người mà cần để giải trình thực cho biết dải sửa đổi mang hiệu lợi thăng tính (hay chí ít là không cấu thành gây hư hại) cho cả hệ điều tính chung.
- Người **sử dụng (Users)** đòi độ chuẩn nhất định cho thành quả chạy định hình của hệ phần chuẩn hóa phát triển của hệ AI cho luồng code (AI-assisted development workflows) ở bậc tốt và đáng mến dụng vào.

## Các luồng công việc chính là gì?

Framework được tổ chức xung quanh sáu nền tảng trọng tâm (big rocks):

**1. Golden Test Case (Test case Vàng)**
- Các test case cơ sở được tuyển chọn cẩn thận chứa đầy đủ tài liệu AIDLC và mã nguồn đầu ra
- Các đầu vào tham chiếu được phiên bản hóa mà tất cả các bài đánh giá đều chạy theo
- Đảm bảo đánh giá nhất quán và có thể tái tạo qua các thay đổi

**2. Execution Framework (Chủ sở hữu: Jeff)**
- Công cụ điều phối cốt lõi chạy các test case vàng thông qua từng bài đánh giá
- Quản lý quy trình từ đầu vào test case đến đầu ra kết quả có cấu trúc
- Điều phối trên tất cả các khía cạnh đánh giá

**3. Semantic Evaluation (Đánh giá Ngữ nghĩa)**
- Sử dụng AI để đánh giá ngữ nghĩa các đầu ra tại các điểm đánh giá chính của con người
- Chấm điểm đầu ra về độ chính xác, tính đầy đủ và sự phù hợp
- Xác thực rằng nội dung do AI tạo ra đáp ứng các tiêu chuẩn chất lượng
- Tất cả các số liệu ngữ nghĩa được báo cáo **@k** — mỗi bài đánh giá chạy nhiều thử nghiệm để tính đến tính không xác định trong việc chấm điểm dựa trên AI (xem mục "Ý nghĩa của @k là gì?" bên dưới)

**4. Code Evaluation (Đánh giá Mã nguồn)**
- **Linting:** Tính chính xác của phong cách mã
- **Security:** Phân tích Semgrep để tìm lỗ hổng
- **Organization:** Phát hiện trùng lặp mã, các mẫu sử dụng thư viện
- Tạo ra các điểm số bằng số (ví dụ: "3 vấn đề bảo mật mức độ nghiêm trọng cao")

**5. NFR Evaluation (Đánh giá Yêu cầu Phi Chức năng)**
- Tiêu thụ token trên mỗi quy trình
- Các phép đo thời gian thực thi
- Kiểm tra tính nhất quán qua nhiều cấu hình model
- Số liệu sử dụng tài nguyên

**6. GitHub CI/CD Integration & Management (Quản lý & Tích hợp CI/CD GitHub)**
- Các đường dẫn tự động kích hoạt đánh giá trên PR
- Tạo và đính kèm báo cáo con người có thể đọc được
- Lưu trữ các báo cáo đã được phiên bản hóa để so sánh lịch sử

## Nó hoạt động như thế nào?

1. Các **Golden test case** xác định các đầu vào tham chiếu (tài liệu AIDLC + đầu ra mã nguồn dự kiến)
2. **Execution framework** chạy các test case này thông qua từng phương diện đánh giá
3. Đánh giá **ngữ nghĩa, mã nguồn và NFR** tạo ra kết quả có cấu trúc
4. **Báo cáo** được tạo cấu hình tóm tắt tác động qua tất cả các nền diện
5. **GitHub CI/CD** tự động hóa toàn bộ quy trình trên các PR và gắn kết các báo cáo để người xem có thể đánh giá
6. Báo cáo được phiên bản hóa lưu trữ cho các bản khảo soát phân lịch sử để quy hướng dạng đối chiếu

## Môi trường nào được hỗ trợ?

Kiro là một môi trường đi hàng thượng đẳng dành thiết thử quy trình, nhưng cấu trúc rải dài framework cũng mở tiếp nhận kết nối qua rất nhiều cấu công cụ AI / môi trường hệ đa định dạng AI mà kết nối tiếp quản để quy đáp cho định của ứng khách hàng nơi ấy (họ đang đứng chân).

## Cấu báo `@k` trong chỉ trị của dòng chữ nghĩa (semantic metric) giải hàm điều gì?

Có sự thiếu sót bất khả kháng ở dòng định không định nguyên một dạng chuẩn đánh do AI cấu nhận (non-deterministic) vì một mảng giải luồng chạy dẫu trùng thông vào (Input) nhưng trả theo đánh gá cấu xuất cho bảng dòng khác. Quá định lấy đi uy tính thật của trả xuất nên cái cấu báo cáo đánh mác quy cho tiến luồng gọi liên hoàn đục nhiều bài đo đánh giá giải đa diện cấu (ở *k* lượt/ trials), tự trích tính chuẩn 2 bộ quy trình tham số bù khuyết nhau theo định từ biểu đồ ([Kế sách Anthropic: Cách làm giải đáp số liệu cho hệ nhân tố AI](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)):

- **pass@k** — Cái khả dĩ thành thông một bộ trong số *k* nỗ lực thử. Rải cho cái câu hỏi: *"cái quy chế đi của này có ra được cho người dùng một kết trả tốt chuẩn nào chưa?"* Lấy nhiều hơn lượng con số *k* là kích đôn phần trăm tỉ lệ điểm quy vì dẫu sai ra sao với rất rất nhiều phép dò giải thử, tự sinh độ chắc để có ít hay nhiều 1 bài đậu pass thì phần lớn nó có tăng.
- **pass^k** — Xác định là *khoanh hết tất cả k bài test* mới đậu (succeed). Đi tìm cấu trúc phân hỏi: *"Quy cấu có cho tôi bản chắc và cực kỳ cực kỳ vững tay (consistently) trích một kết chuẩn chỉnh nhất chưa?"* Cho *k* càng vọt giá xa vời quá cao sẽ khó đánh cấu độ chạm được cái này vì quy bắt mỗi tiến điểm thử thì phải đậu (must pass) cho hết thì mới chắp vá lại được.

Tại *k*=1 mức bộ cho chỉ tỉ của cái giải trên như như một (Cùng giống hệt đi trả ở một phép). Nếu mà *k* dẫu to sẽ phân mảnh dần phân trích riêng  — pass@k bám tỉ ngưỡng cao tiệm tận của mức quy đậu đạt 100% trong lúc đấy thì điểm số dòng  pass^k rớt tự do lao không phanh thảm đê xuống ở đáy dưới 0%. Lấy kèm ráp nối của cả đôi thông qua cấu thành giải hai tầng báo hiệu cao cực trần chạm tối đỉnh của phần tính giải ở mức tính thông hiểu kỹ năng năng lực (capability ceiling) cũng đi cho trần thềm mức sàn đáp đo lòng điểm cấu tính thành tín nhiệm (reliability floor) ngay ở trên một sự cấu biến cập nhật luồng (workflow change).

Kiểm định đánh số tính báo theo Code (Code evaluation) cùng đo mốc đo NFR là các quy giá chắc chắn đi ở dạng xác định nguyên tĩnh định dạng đo cho 1 hệ chạy tự sinh nên không cần yêu cấu thiết hệ `@k` .

## Cách diễn giải các báo cáo?

Giới báo cáo đính vào bao gom:
- **Tỉ đánh chất thông giải chữ Semantic scores @k:** Kết toán từ màng dò nhận chất dạng sao hạng rate có tính AI báo chuẩn đi chốt lấy hệ của thông năng khả thụ do `pass@k` (tính bản) đi kèm cái rào nhận uỷ ở `pass^k` (tính củng nhiệm độ vững tin).
- **Hệ chấm độ cho điểm Code (Code scores):** Tính con số phân chèn tính thực tế cho công phu từ quá phân đánh Linting, check lỗi mạn báo an bảo mật (Security), báo sự chia nhịp viết Code dạng lại trùng của dải hệ luân sao lại ở (thể lấy thông dạng tính nhất không đổi, tĩnh - deterministic)
- **Tham khảo báo về biến bộ của dòng báo NFR (NFR metrics):** Thống tiêu độ chi mất dòng mã tokens sinh, giới ranh đo mộc trên độ lằn đi thời điểm định quy, đối đâm định độ của sự cân ổn hệ số (Cái này tĩnh - deterministic)
- **Tách thông theo quy phân đánh chiều phát triển Trends (Trend analysis):** Khảo quy qua cho phép chiếu để ra đối trích về thế hệ lịch bộ version lưu có (Bám dính để đâm đấu thông chéo quy luồng giải test dạng lưu - golden test cases)
- **Cánh cổng qua/chận (Pass/fail gates):** Hệ cấu minh bạch thông rành của định lưu giúp quan xác việc chốt dải các định dòng nâng cấp ấy có lọt vừa không với hệ chốt gán rào theo định ngưỡng quy sẵn.

## Chuyện rủi nếu việc báo đánh có cảnh? (What if my change shows a evaluation?)

Bạn không hẳn lo luồng khóa đứng bộ tự ngắt cho nhập mã nhánh (block merges)—Họ mở luồng bám ngữ (provides context). Chơi bài dán hiệp chung tay những chóp (maintainers) nhằm tạo:
- Dễ am biết có hay không chuyện các mảng dải đó coi bộ được nhận và chấp nếu mà xem phần lợi được hơn (acceptable given the benefits)
- Phân định các phương cách mà giải xoa đi được cái nguy hại kia
- Lập hệ bút lục giải chép những thông về các dạng thế đánh lùi điểm mà phải dính vào những thương đau buộc thoả hiệp lấy nhượng cái khác (trade-offs) 

## Nó liên đới gì đến nhánh mẹ khu dòng mã kho chứa luồng workflow bên AI-DLC?

Nó cho ra bản giải công theo hướng định bám kiểm đôn dò cấu kiểm bộ khóa cho kho luồng gốc kia [AI-DLC workflows](https://github.com/awslabs/aidlc-workflows) nhằm tạo rào bảo hiểm xem mọi quá trình bám đi đổi theo cháp hay làm gia tăng thêm hay tệ đi bản giải độ chất chung. Cấu nó ra định làm cái mạng chắn quy định tầng layer bám chặt thử lửa hệ điều quản ngay trên chính mớ của nó cho bản workflow ngoài bản ngã kia.

## Tôi có thể chạy các thử nghiệm cục bộ trước khi gửi PR không?

Được chứ — phần ứng khung (framework) thì cũng được cho bám làm cho thiết diện gắn với mạng giải CI/CD chạy lưu trên đám nhưng mà chả ai cản nó bung cấu thiết hệ tự động đi chạy tự kiểm bên trong mạng máy nhánh cục (local) để dò trước.

## Báo cáo được phiên bản hóa như thế nào?

Mỗi lần chạy qua test trích tạo đâm thành hệ thiết có phiên bản gắn theo tựa (numbered/named version), đính kèm:
- Sinh cái số chỉ thời lưu và đóng dấu mã thông chạy SHA tự mã sinh của nhánh commit đó.
- Dải trích thông đủ cả bài báo bộ kiểm test results 
- Cân đối với những hệ tính ở tầng ngưỡng quy vàng baseline
- Bảng tóm độ lược lại có người thông vào đọc diễn mượt.

Biểu kho lưu hệ có để giữ cho lưu dấu xem để rà những sự cố đã tính ở nhánh trước hay phân lịch phân nhánh giải trích dòng trend chung (trend tracking).

