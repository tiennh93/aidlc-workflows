# Thiết kế Chức năng (Functional Design) — Lending Service — Luật Kinh doanh (Business Rules)

## Bộ Luật Xác thực (Authentication Rules)

### BR-AUTH-001: Đăng ký (Registration)
- Tên (name) là bắt buộc, tối đa 100 ký tự
- Email bắt buộc, định dạng hợp lệ, phải là duy nhất (unique) trên toàn bộ hệ thống
- Mật khẩu (password) bắt buộc, tối thiểu 8 ký tự
- Mật khẩu chỉ được lưu trữ dưới dạng mã băm (bcrypt hash), nghiêm cấm lưu dạng văn bản thô (plaintext)
- Vai trò tự động gán (Auto-assigned role): "member"
- Tự động kích hoạt tài khoản khi khởi tạo (is_active = true)

### BR-AUTH-002: Đăng nhập (Login)
- Kiểm tra tính tồn tại của email và tài khoản phải đang trạng thái kích hoạt (active)
- Đem chuẩn xác thực password đối chiếu với bộ mã băm bcrypt hash
- Phản hồi sinh mã token JWT bao gồm các thông số: member_id, email, vai trò (role) và hạn dùng (exp) là 24 tiếng kể từ mốc sinh.
- Nếu thông tin cung cấp sai lệch, trả về HTTP 401 (nghiêm cấm để lộ lỗi sai là do hỏng email hay hỏng mật khẩu)

### BR-AUTH-003: Xác thực Mã JWT Validation
- Kiểm duyệt tính vẹn nguyên của chữ ký, độ hạn hẹp thời gian (expiration), và các claims cốt lõi đòi hỏi phải có (member_id, email, role)
- Rớt chuẩn và đẩy ra 401 nếu token đã quá thời hạn
- Bóc tách phân giải lấy các dữ liệu (member_id, email, role) từ trong dải JWT hợp lệ

## Nhóm Luật Thành viên (Member Rules)

### BR-MEM-001: Điều chỉnh Hồ sơ Profile (Update)
- Member chỉ được tự trao đặc quyền cập nhật thông số tên (name) và email của chính bản thân mình
- Dải email mới thay đổi phải đảm bảo tính duy nhất
- Tuyệt đối cấm can thiệp thay đổi các thông số về quyền (role) hoặc bật/tắt kích hoạt trạng thái profile (active status) thông qua update

### BR-MEM-002: Hủy Kích hoạt Acc (Account Deactivation)
- Rành rạch chỉ hệ Admin mới được nắm thóp deactivates (hủy kích hoạt) tài khoản
- Sau khi deactivates member sẽ mang ấn định: `is_active = false`
- Toàn bộ hồ sơ đang vay giữ checkouts vẫn để lại trạng thái active (system vẫn theo dõi giám rành độ trễ phạt cho tới khâu được đem trả return)
- Các mảng holds gạch sách đã xin vẫn cài dạng active
- Khóa cứng lại các thao tác: mượn sách mới (new checkout), xin đặt gạch mới (new hold), gia gạn báo mượn (new renewal)
- Admin tuyệt đối cấm tắt hủy (deactivate self) đối với chính tài khoản của bản thân 

## Bộ Luật Cho Mượn (Checkout Rules)

### BR-CHK-001: Rào Chắn Kiểm duyệt Mượn (Checkout Validation)
1. Sách đo phải hiện hữu (Giao tiếp dò verify chóp mạng qua bên Catalog Service)
2. Sách buộc phải đang vách độ available_copies > 0 (Khả có lượng bản lấy) 
3. Member xả mảng buộc active (is_active = true)
4. Mức cựa số đai checkouts cự hiện đang mượn móc đai rẽ < Cấp Rào MAX_CHECKOUTS (5 cuốn)
5. Hạn Phí nạp Phạt nợ rẽ Outstanding fee bọc balance <= dải Giới ngưỡng FEE_THRESHOLD ($10.00)
6. Bọc Tất Cả 5 dải test cự chóp ngỏ trên nảy buột phễu lọt đai test pass vách trước khi cắm bọc Lệnh đai mượn checkout bọc

### BR-CHK-002: Lập Mượn Xét Sách (Checkout Creation)
- Ghim móc checkout_date = ngày lúc nay (định bằng UTC)
- Bù cháp due_date (hạn hẹn về) = checkout_date + 14 ngày
- Nạp status rã = "active"
- Ấn cự renewal_count (Số lần gia hạn đổi) = 0
- Xuống trừ (-) 1 gán điểm available_copies bằng mạng qua Catalog Service
- Nhả lỗi ngắt (abort checkout) nháp bục nếu văng cựa mảng lỗi khâu Call tới hệ Catalog Service 

### BR-CHK-003: Tính Hàm Return (Return Processing)
1. Mục mác checkout đó phải đai đang rã ở túi "active"
2. Group Members chỉ bọc tự trả lấy chính cháp nạp mượn của thân bục. Còn Admin với Thủ Librarian thì bục được vách chóp mọi nhả ai cựa return.
3. Chạm mốc gán return_date = now (trích báo UTC), sửa chóp status = "returned"
4. Tính hệ cấu phễu Phạt rã Late fee nếu mảng gán rẽ Quá Hạn cựa (overdue):
   - days_overdue (Ngày Mốc Trễ) = (móc date nảy dải của return_date() - lấy due_date.date()). Thật sự dải days (cự chỉ chóp khi mảng trích ra là Dương Ngỏ Số Positive)
   - fee (Trút Phí phạt) = days_overdue (số nảy trễ) * $0.25
   - fee trút Bị Chặn phễu Cap móc là rã $10.00 đâm đo cho một đai lần checkout
   - Khởi gán túi fee rã cháp mới nhả record mạc nếu fee bục > đai 0
5. Lên cộng (+) 1 độ nhả available_copies truyền báo điểm Catalog Service
6. Chóp Kiểm nảy cự Hold bọc queue: cựa đo Nếu nhả thấy Có trích Mem Góp đai đang móc bục Hold "waiting" bọc cho Cựa sách Mảng Trích Book, xử trích Fulfillment phễu cho Ngỏ thằng cựa Next.

### BR-CHK-004: Ngỏ Gia Hạn Renewal
1. Phễu cựa mác Checkout đó phải vách trút Exist đai mà mác rẽ nằm ổ "active"
2. Nó đai móc Phải Thuộc Cựa đứt chóp Của gán Thằng Member chóp kêu bục
3. Ngỏ Thành viên rã móc này cựa Phải đang ổ phễu đai Active 
4. Vòng số nạp renewal_count nhả < MAX_RENEWALS (2 lần đai)
5. Đặc bọc biệt: Không bục được nảy xả Có mỏ náp "waiting" rã holds nhả đai ngỏ hiện rã ở trên Book rã móc mạc đo
6. Cựa xả Extend nạp dải Nới Rã Mạc Nhả due_date dải bọc thêm cựa 14 đai ngỏ days từ gán mác Của due_date bọc móc hiện giờ nảy đo 
7. Trích nảy Tăng phễu bọc điểm cựa renewal_count nhả rã 

## Nhóm Phễu Mạc Gạch Holds (Hold Rules)

### BR-HLD-001: Khâu Xét Cắm Hold 
1. Móc sách đai Book tút p hải rã Tồn Bọc ngỏ tại đai (thông bục báo Catalog Service)
2. Bộ đai nảy Book available_copies rã xả Nhả Bắt Buộc cựa là cựa 0 (Chỉ gán Cho Test Hold Khi nháp Sách trút Cạn bọc Hàng Unavailable ngỏ đo) 
3. Túi nảy Thành viên Member bọc đai gán là Active 
4. Độ xả Cựa Nảy active cháp hold của bọc Mem rã bục đó < MAX_HOLDS (3 Mỏ) — đếm cựa Holds bục mạc status có bọc "waiting" mảng trút là bọc "ready"
5. Mem cựa đai móc Tuyệt Đối ngỏ cháp Chưa mảng đai Hold cự mạc chóp Sách rã báo này nảy đâm 
6. Đúc đai mảng Tạo cự Hold bọc mang mảng cái Status nạp rẽ "waiting"
7. Nhét nhả Vị cháp rã Cấu Queue position bục = max cháp rẽ Queue mỏ của rã túi Book đó đai móc + 1 (Chuẩn rẽ mạc FIFO)

### BR-HLD-002: Thả Trút Bỏ Gạch Hold 
1. Book Hold mạc đó đai Phải rẽ Exist Exist
2. Gõ cựa Các Group mảng Member chỉ nhả Hủy Cancel mảng Được Trích Own Hold rã của rẽ mình; Hệ máy Admin bục Librarian thì bóc rã bục thảy nảy đo (cancel bọc any) 
3. Bẻ mỏ mạc status = "cancelled" ngỏ 
4. Trút Reorder lót rã Lại Queue (hàng rã): Lập trút bọc Rút trừ nảy decrement vị rã position cho dải Toàn bộ rã móc đai nhóm cựa hold vách ngỏ đang nảy bị kẹt vách Ụ bọc nảy dưới đo của Ngõ Book báo đó chóp ngỏ 

### BR-HLD-003: Phễu Giải Gán Mạc Chuyển Khâu Xử Gạch Quyền Lợi Hold (Lúc bục trích Xả Return Sách) 
1. Cựa rã Khi nảy Trích Cuốn Sách Book trích Mạc báo Return (trả rẽ đo), Nạp báo Check phễu nảy Cựa hàng waiting holds mạc ngỏ có trên mỏ cuốn Sách. 
2. Mạc Tim lấy cái bục Mỏ Hold đang có mạc lowest cài ngỏ (Thấp bọc rã Bực nảy nhất cựa) của queue_position bục, mạc đang mang nạp mỏ dòng "waiting". 
3. Sửa nạp mạc Status của mỏ bục Rã nó lên rẽ "ready" móc bọc rã 
4. KHÔNG bục đai móc Rút trừ decrement gán ổ nạp available_copies cựa (Vì cái rã mạc "ready" hold đo nó đã ngỏ Tút Khống Chế lấy đứt Reserved copy đo vách Rã bằng bục logic Conceptually rồi). 
   - Đai Giải rã Thích Kỹ cấu này cài : Cái `available_copies` á cựa nảy tụi đằng trích cựa Return bục nó đã phễu nháp đâm Increment gán cựa ở đai Nhịp trên rẽ dải. Cửa cái người bục Nạp Đai "ready" hold đứt thì trút nhả khi đến cựa lúc Checkout mỏ rã theo lệnh Normal đai cựa rã ngó ngỏ Nó lại bị Trút Decrements phễu vách rã nữa móc gán bục nảy một đo nháp lần trích. Thế ngõ cháp ra Cái mạc Available copies bọc đo rã nó nảy xả Reflects trích đúng y rẽ móc Thực tại Physical availability mỏ rẽ 

## Nhóm Phễu Mạc Tiền Phạt (Fee Rules)

### BR-FEE-001: Tính Nạp Trễ Hệ Tiền Late Fee 
- Giá bục rẽ Rate: $0.25 ngỏ đo per đai móc mác một ngày nảy Trễ cựa nạp Overdue 
- Mức mảng Đo Ngỏ bục Chặn Cap (Cap): $10.00 cho 1 cựa checkout rã
- Mình ngỏ xả Chỉ đo móc Áp cựa ngỏ Phí nảy đai mảng cựa Charged chóp khi móc Nạp Return bục đai móc rã ngỏ , vách chứ cựa nhả bọc phễu không bục đai đo nháp cộng móc đai dải accruing bọc mác rã báo theo đâm real-time chạy ngõ cự nảy đai
- Bục Máy nảy Calculated Nháp mảng rẽ bằng các nạp rã UTC đai cựa rã dates trích (vách chỉ nảy lấy móc trích Nháp whole đai bọc mảng nảy nguyên ngày ngỏ cựa only đo)

### BR-FEE-002: Bảng Cựa Nộp Nhả Phạt bục Pay Payment 
- Group đai Thủ Admin/Librarian chóp process rã báo lệnh nảy phễu nảy Payments ngỏ đai
- Mở rẽ cho gán cựa trả móc Partial nảy payments đo (Nộp Đai Từng Phần ngỏ bục báo Rã mỏ) 
- Tiền mảng xả Payment sẽ móc bọc rã Nháp đai ứng trừ Cựa phễu ngỏ oldest rã đai món bọc trích rã phễu mảng Nợ nảy cự Nháp outstanding móc fees ngỏ nháp trước rẽ bục đai mảng Của (first). 
- Cựa Lượng dải nạp móc Đóng cựa phễu amount phải ngỏ mảng rã Bục Lớn mỏ cựa Hơn > 0 bọc nạp
- Lượng Tiền Payment trích cựa không được vách Bục dải Exceed (Trút Gán bọc Vượt Tràn bục Hơn đo) vách Tổng báo mỏ Total bọc outstanding nỏ balance (Tổng bục Tiền mác Nợ nảy rã rẽ). 

### BR-FEE-003: Số Trích Tiền Thật cựa Còn báo Outstanding bọc Balance 
- Trút Tổng Sum của mỏ gán Trích bộ all ngỏ fees bọc đang mác có rã nảy mạc status cự = "outstanding" móc nảy TRỪ bọc (-) phễu xả nạp Mảng đứt rã payment payments. 
- Ngõ Lấy xả mác cái mỏ Chỉ Số bục này báo dùng mỏ Ngõ cho trích Mảng Check mốc Dải Checkout bọc Threshold đai Mạc (Thuộc đo Hệ BR-CHK-001 nhả phễu dải của móc rule ngỏ phễu 5). 

## Luồng Báo Luật Gán Báo Tính Cáo (Reporting Rules)

### BR-RPT-001: Mảng Trích Report Đai Báo Overdue Cự Nảy 
- Trút Lấy mỏ All nháp nhóm bọc nảy checkouts đai bọc có cái đai status = "active" VÀ móc đo Rã Tướng due_date nảy < Cựa Gán Móc now nạp rã đai
- Xả Có Include: Cựa đai nảy member dải name, vách rã mã email bục, bóc title bục trích sách book cựa, đai checkout_date nạp trích mỏ mảng, gán due_date móc bọc ngỏ nạp, rã báo đai cự trích bọc days_overdue đai nảy 
- Book mỏ title (tựa phễu sách trích mạc) bục Được Get (fetched vách móc dải ngỏ từ API Catalog nạp) hoặc báo trút là mảng bọc lưu trích denormalized nảy mạc trong lúc gán checkout nháp rã. 
- Mạch mỏ nạp Accessible nháp chỉ bục cài ngỏ xả đai Bọc Bởi cự nạp rã Librarian bọc với mỏ nạp Admin mảng rã cựa ngỏ chóp đai only

### BR-RPT-002: Lịch Gán Chóp Report Trút Bục Rã Phễu Tổng Bộ Nảy Collection cự Mảng Summary
- Vòng total_books: dải nạp rã Count chạy lấy API từ Catalog trích mảng cựa Service rã đâm
- Bộ nảy total_members: Đếm xả Count all mảng chóp groups ngỏ members cự. 
- Mạc books_checked_out: Đếm ngỏ Count active chóp checkouts ngỏ đai 
- Gán đai nảy books_available: Lấy ngỏ total_books rã (-) đi rẽ bọc books_checked_out ngỏ cự
- Dải bục total_outstanding_fees: Trút Sum (Cộng rã Tổng đai bục) cho dải amount của outstanding bọc mảng fee. 
- Bọc nạp Chóp Accessible nháp chỉ đứt dải Admin trút trích

## Các Chỉ Cắm Rã Ngỏ Constant Đai Bục Mảng Configurations 

| Dải Định Constant | Móc Giá Trị (Value) | Tính Báo Giải Nghĩa Đai (Description) |
|----------|-------|-------------|
| MAX_CHECKOUTS | 5 | Trút Maximum mỏ bục active móc ngỏ mạc checkouts nạp ngỏ dải nạp cho per đai mảng member |
| MAX_RENEWALS | 2 | Cựa Giữ Max bọc số nảy dòng trút renew bục gia báo nảy hạn bục rã per dải checkouts đai cựa |
| MAX_HOLDS | 3 | Lệnh mỏ báo Max active holds dải rã bục nạp ngỏ giữ nảy gạch ngõ phễu |
| LOAN_PERIOD_DAYS | 14 | Sốt Nhả Cựa Số bục nạp ngỏ vòng Days đai nảy cho per checkout nạp nhả móc cháp period rã |
| LATE_FEE_PER_DAY | 0.25 | Mức Của Đô bục Dollar cho Phí ngỏ Trễ nảy mạc Daily bục móc |
| LATE_FEE_CAP | 10.00 | Mức Trút ngõ rã Phạt Trễ cựa bục Lên dải Tối mác nảy cháp mạc checkout rã bọc |
| FEE_THRESHOLD | 10.00 | Dải Chặn Threshold bọc Báo Ngỏ Khóa cựa Mượn checkout cự do mảng Outstanding rã fee |
| JWT_EXPIRY_HOURS | 24 | Mác Nạp Trút Giờ cựa Hết báo hạn JWT Exp Token đai bục ngỏ |

