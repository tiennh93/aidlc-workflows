# Các Câu Hỏi Xác Minh Yêu Cầu (Requirements Verification Questions)

Tài liệu định hướng (vision document) và tài liệu môi trường kỹ thuật (tech-env) đã rất chi tiết. Các câu hỏi dưới đây nhằm giải quyết những điểm mơ hồ còn sót lại để đảm bảo tính hoàn chỉnh đầy đủ trước khi xuất ra bản yêu cầu (requirements) chính thức.

Vui lòng trả lời từng câu hỏi bằng cách điền chọn mẫu chữ cái phù hợp phía sau khối thẻ `[Answer]:`.

---

## Câu Hỏi 1
Tài liệu định hướng quy định thiết kế cấu trúc phản hồi kèm các mã lỗi (error codes) cụ thể. Vậy liệu khối bộ xử lý tĩnh 422 từ biến sửa (custom 422 handler) có nên đánh bọc chung ôm luôn các cờ báo vi phạm xác thực Pydantic (Pydantic validation errors) vào bên trong dập theo cùng một cấu trúc phễu mẫu vỏ bì y xì giống vậy không (`{"status": "error", "operation": "...", "inputs": {...}, "error": {"code": "INVALID_INPUT", "message": "..."}}`), hay làm một bảng định dạng đơn giản hóa là đủ thỏa lấp chấp nhận được đối với loại báo lỗi báo dạng vấp xác minh validation này?

A) Sử dụng rập khuôn bọc rành mạch bộ vỏ báo hiệu (envelope structure) dùng áp chung trọn gói cho toàn thảy tất cả các sự cố lỗi, bào gồm bao chứa kẹp đủ bộ lỗi bắt xác nhận lỗi Pydantic validation errors
B) Gỡ sử dụng bộ cấu trúc đơn giản giản chế bớt rào biểu chừa ra (bỏ khuyết 2 trường rập `operation` và trường thông tin `inputs`) khi gán trói các báo điểm vấp vạch lưới lọc Pydantic
C) Ý kiến Khác (xin điền vui lòng biên mô tả lời vàng ngọc vào liền ngay theo sau thẻ gạch đầu dòng [Answer]: ở đoạn dưới trễ)

[Answer]: A

## Câu Hỏi 2
Đối chiếu vào bên thao tác khối thống kê số học `mode` (số yếu vị mang mốc điểm tần số phủ dày nhất), hệ sớ tư tưởng vision định chỉ định "buộc trả về hệ mode cấu bậc nhỏ nhất nếu mang vấp dải trường hợp bắt trùng giá trị đồng bậc móc ties". Vậy liệu tính năng hàm hệ cấu `mode` nên báo giao kết toán chỉ bằng duy nhất một móc trị số duy độc nứt (single numeric value), hay là sổ hẳn một băng danh sách (với quy móc trả luôn trị định số thấp nhỏ nhất khi mắc kẹt chập dính đồng hạng gá chót dầm trượt ties)?

A) Luôn trả về duy nhất nhõn một giá trị định số duy độc trịch (chính là mốc mode báo dạng nhỏ thu nhỏ nhất nếu gặp vướng dây dính giáp trường hợp chung mâm đồng hạng số ties)
B) Thả rã băng trọn danh sách xổ toàn bộ các mode đạt chốt báo tóm đằng bảng báo sắp dần lên theo thang thứ tự độ
C) Cấu Khác (vui lòng mô chép thêm phần định thuật vào dấu mốc sau tem cờ bứt nút [Answer]: dậu vạch dưới đây)

[Answer]: A

## Câu Hỏi 3
Sớ tài liệu có điểm ghi chú mã thẻ điểm cờ gá báo danh lỗi `OVERFLOW` lật trỏ làm bộ định dạng mã lỗi cờ quạt. Hệ cấu module `math` Python hay phóng nhả bung thông đáp số `inf` phả hồi báo điểm chóp cho các chốt dội hất văng cảnh vỡ độ trồi giới hạn tràn khung bến (e.g., gọi nháp test móc lệnh `math.exp(1000)`). Vậy hệ cổng liên vận API có nên nhả đùn thông cái báo tín mốc ghi chữ `inf` lộ mặt vào tận thẳng bên phía trường trả báo đáp quả bóc lấy gán vô trường báo kết quả (result field) hay không, hay bộ cớ API nên phái bắn ra cái mảng quăng đập thiệp báo sự cố đáp dội hộp tin error OVERFLOW?

A) Phái bắn cẩn xuất đùn hộp tin rập đáp dội bọc error OVERFLOW vào hễ thời lỡ mỗi khi quả kết thông mảng điểm rơi toán học giáng điểm dính rập là văng kết dạng `inf` hay chạng ngầm `-inf`
B) Cứ tự nhả tung mảng báo `inf`/`-inf` mặc định như các quả chốt đóng đạt kết trị chuẩn thông hành valid results (và thi thoảng đi lính báo dội lụi ném đáp quăng error với chỉ thuần tuý dăm dạng chỉ số ngặt nghèo cấu trị quái dị ảo diệu unrepresentable values)
C) Phả thả dội móc bung móc ra rập báo `inf`/`-inf` dạng chuẩn hợp mệnh valid results cho hàm đẩy cước nhè báo cho riêng một cặn báo gã `exp` còn bao nhiêu xấp đám tàn toán quân thì dồn nắn báo mảng đùn cấu trượt báo hãm lệnh error OVERFLOW
D) Dạng Định Khác (để mời ghi trạch nháp đắp vào sau đuôi cờ bùa [Answer]: dạt dưới phết nha)

[Answer]: A

## Câu Hỏi 4
Đối chiếu về mảng điểm chuyên phục vụ thao tác cấu đẩy dời mốc chuyển hệ định kích phân cách hoán mảng bộ đơn vị quy đổi chuyển (unit conversions), sớ dự tính vạch rẽ chia cấp riêng từng nhúm cục bảng đơn vị rõ rệt quy thuộc cho đủ mọi ngạch. Liệu có nên cứ táng chốt thả vạch mảng lệnh báo khước dội rũ là `INVALID_INPUT` đập mã số 422 nhồi đẩy đối chọi dập cùng các nhánh hệ biến lòi ra từ bộ phận báo khuyết danh lạ mặt cấu lạ thông số điền ở `from_unit`/`to_unit` nhét vào, hay nên tạc xuất một mã tem báo cấm lỗi cục thiết diện chẻ sát cụ thể hơn?

A) Trả ra vạch ấn chóp đỉnh báo điểm lỗi `INVALID_INPUT` (422) trị phạt giáng đầu đối định mảng các hệ đơn vị xa lắc lơ mù tịt unknown units — duy trì đường cấu tạo phong thái đều nhịp quy chế ráp khuôn chuẩn bọc đồng quy đi sát cùng với các thể ngạch báo lồi lõm lỗi rào bắt dội vạch validation errors còn lại
B) Gõ móc chốt khảm đính đánh cái cấu mã chốt định dành cho lỗi lạch chệch sai lệch unit chệch đơn vị mang ấn `UNKNOWN_UNIT` móc mảng trạm cờ trạm thông trỏ HTTP 400
C) Lệ Khác (kính lưu mời đánh viết điền trích mô cấu tả thêm theo cái dấu đuôi [Answer]: rọc dưới này)

[Answer]: A

## Câu Hỏi 5
Phần tài liệu thiết giới tech-env cắm ấn định hệ thông tuyến khởi điểm bằng gõ pháp lệnh chạy rập bằng móc lệnh báo định cờ nháy `uv run pytest` và `pytest-cov` bọc cọng lệnh đi tháp tùng phải cán dải quy trạm điểm bao vây rốt đỉnh sàn vớt bao vây mã nguồn chạy test dính 90% (line coverage). Vậy có nên để cấu file neo gánh mạng biên chốt file điều trạch lệnh thông đồ thiết giới `pyproject.toml` tự động bám bắt cấu bóp giới kẹp trọn quy ngạch định gông trạm thi hành bắt chóp dội mốc chót chuẩn lót nấc 90% nẹp thềm threshold không (oạch trượt oẳn liền cái chùm dải phái trạm mảng lưới chạy test rớt test run thi điểm dải vớt không níu đạt mốc điểm cắm rớt quá chót đáy tỉ xuất dưới lọt rạch 90%), hay chỉ buông tơ kéo nhẹ xốc điểm liệt lôi màng lưới báo trình dội thông chốt ra tỷ số coverage?

A) Buộc ép siết cổ dội móc vớt đo thềm sào gạt mức minimum sàn 90% — oạch đập gãy rớt thảm tất lảy mọi chùm tệp dàn trải mảng nhắm chạy test nếu báo đo kết nạp hụt điểm cởi dây đo tuột vớt lót sàn tỷ mức xệ đổ < 90%
B) Đùn trút đi vạch kéo báo nộp đống file in gạch danh biểu ghi xuất nấm số liệu bao che lót phủ sóng trạm coverage thôi — chứ không đánh đập oạch cấm nổ tạch rớt đài bắt báo fail rớt sàn dải khâu rải báo lệnh test run nhót dính cớ điểm coverage
C) Tự Định Đặt Khác (dặn nhớ điền mô nháp trỏ ghi sau nếp tem cờ dập nổi [Answer]: phết dưới này nhé)

[Answer]: A

## Câu Hỏi 6
Bảng khát vọng dự định vision biên trỏ ghim chốt `nan` liệt hàng điểm danh là tay chân vào đội hạng định nghĩa mảng biến định danh hằng số cố định bất chuyển. Liệu đối chất với những vòng rập khối toán tính toán khâu hoạt động vạc chóp hứng nhận trúng mấy mảnh vụn đầu luồng rơi nhét vón biến hỏng phi nghĩa đầu cấu dạng mốc `NaN` lọt vô nạp làm dải luồng thềm biến móng input (với ví dụ giáng cho một chiêu kiểu `add(NaN, 5)`), thì nên giáng đòn đáp lễ đùn phun rải trả dội nguyên mẫu rập khuôn đáp rệu y mạo mặt gốc dạng `NaN` răm rắp y xì theo luân lý lan tràn chiếu bộ hệ IEEE 754, hay nên tống khứ xô đuổi dạt đi và trả khước bằng quả chốt cộc lốc báo gán thẻ ném thông vi phạm lỗi lệnh `INVALID_INPUT`?

A) Cho lan lỏi phóng sinh nảy mầm hất đùn dọi dạp biến lọt thả báo thông số hệ NaN trườn dập tuột tuân theo móc bộ luật giới IEEE 754 (trả lại dạt vào gán ô khung thả báo ván kết mốc quả result với điền biến số NaN)
B) Bóp mũi cự đoạn thẳng tay bác cấm đường màng chỏi lấp dội rập cái biến nhiễu thả lọt hệ input loại NaN vào kèm găm tráp đính trả mã điểm hỏng giáng lệnh rào tạc lỗi `INVALID_INPUT` error
C) Kế Hoạch Khác Đề Xuất (xin chiếu cố điền dòng mô phóng chữ mảng lót phái dấu [Answer]: đóng gá giáp dưới)

[Answer]: B
