# Bài nộp LAB5 — [TrackT4 — Thương mại điện tử] · Nhóm [3Quy]

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Cao Minh Quang | 2A202603906 | · §1 lát cắt, phạm vi và bộ dữ liệu dựng · §2 viết 11 kịch bản rủi ro, khoanh 2 kịch bản đắt nhất · §6 ba tiêu chí Pass/Fail và chấm thử · dựng khung repo, review và merge các nhánh vào `main` · demo phần mở đầu (lát cắt + kịch bản) |
| Nguyễn Năng Thịnh | 2A202604028 | · §3 thiết kế khi AI sai (câu AI nói nguyên văn, người dùng thấy gì, ai được báo, loại lỗi, HAX/PAIR) · §4 mức tự chủ cho 5 tool + tool tách bước, tool khó xếp nhất, thử trên 3 kịch bản · demo phần "khi AI sai và mức tự chủ" |
| Hoàng Minh Quyền | 2A202603961 | · dựng prototype HTML 2 màn hình (người mua · người bán & nhân viên sàn) và lời gọi AI thật · chụp ảnh đánh số · §5 prototype · §7 kịch bản demo và ghi ca lạ · demo bấm prototype + trả lời ca lạ |

## §1. Lát cắt & phạm vi
**Lát cắt (chép từ track, không chọn lại).** Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Trò chuyện với Shopee | người mua | báo "hàng nhận không đúng mô tả" | tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên | người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng |

**AI được thêm quyền gì so với hôm nay.** Hôm nay ("Sản phẩm thật hôm nay") trợ lý chạy theo **menu có sẵn**, bắt người mua tự chọn đơn, tự chọn lý do, tự gửi bằng chứng; người bán phản hồi; sàn xem xét khi hai bên không thống nhất. Khi thêm AI, trợ lý được **đọc lời người mua, so với mô tả, tự chọn bước tiếp theo** và chạm tới 5 tool, trong đó 3 tool **khó đảo** (`soan_hoan_tien`, `tu_choi_khieu_nai`, `phat_shop`). Sai hướng nào cũng có một bên mất tiền: hoàn sai thì **người bán** mất tiền và hàng; từ chối sai thì **người mua** mất tiền và giữ hàng sai. Vì thế điều nhóm thiết kế là **sự trung lập**, không phải tốc độ.

**Ba việc tính năng KHÔNG làm** (prototype cũng không có nút cho các việc này):

1. **Không xử phạt shop, không xử lý nghi hàng giả trong luồng này.** `phat_shop` ảnh hưởng cả shop và mọi đơn khác của shop, cần điều tra mà một cuộc chat không đủ; AI chỉ ghi nhận và chuyển bộ phận chuyên trách, **không hứa phạt**.
2. **Không xử lý các lý do trả hàng khác** (đổi ý, không vừa nhu cầu, giao chậm, thiếu hàng). Mỗi lý do có chính sách riêng; gộp vào thì AI dễ áp nhầm chính sách "không đúng mô tả" và hoàn tiền cho một ca người bán không sai. Gặp các lý do này, AI nói rõ là ngoài phạm vi và chỉ đúng mục trả hàng thường.
3. **Không làm cầu nối liên lạc ngoài sàn.** Không đưa số điện thoại, địa chỉ, tài khoản mạng xã hội của bên kia, kể cả khi người mua hay người bán đòi. Trao đổi chỉ qua chat của sàn, để lời hai bên được lưu làm căn cứ và không ai bị quấy rối.
4. *(thêm)* **Không ra phán quyết tiền cuối cùng.** AI không tự chuyển tiền về bên nào; tiền chỉ đi khi **người bán đồng ý** hoặc **nhân viên sàn duyệt** (lý do ở §4).

**Dữ liệu dựng dùng xuyên suốt bài** (nhóm tự sinh, không phải dữ liệu thật):

| Mã | Đơn | Shop | Giá | Nhận hàng | Ghi chú |
|---|---|---|---|---|---|
| D1 | `#DH-2611-0457` Nồi chiên không dầu "Bếp Mẫu AF-6" | Gia Dụng Mẫu | 1.290.000đ | 09/11/2026 | **Ca A** (người bán đúng): mô tả lúc đặt 05/11 ghi *"Dung tích tổng: 6 lít · Dung tích giỏ chiên: 5 lít"*, không bị sửa. **Ca B** (mô tả bị sửa): mô tả lúc đặt ghi *"Dung tích lòng nồi: 6 lít"*; ngày 11/11 14:20 người bán sửa thành *"Dung tích tổng: 6 lít · Dung tích giỏ chiên: 5 lít"* |
| D2 | `#DH-2611-0391` Áo thun nam size L | Thời Trang Mẫu | 189.000đ | 11/11/2026 | Người bán đã nhắn trong chat sàn thừa nhận gửi nhầm size M |
| D3 | `#DH-2611-0222` Tai nghe "Âm Mẫu Pro" | Điện Tử Mẫu | 450.000đ | 08/11/2026 | Dùng cho ca nghi hàng giả |
| D4 | `#DH-2611-0518` Son lì mã 05 "Đỏ gạch" | Mỹ Phẩm Mẫu | 159.000đ | 12/11/2026 | Tem dưới thỏi son ghi đúng mã 05 |
| D5 | `#DH-2611-0105` Ấm siêu tốc 1,8 lít | Gia Dụng Mẫu | 320.000đ | 03/11/2026 | Hạn trả hàng đã hết 23:59 10/11 |

**Chính sách dựng** (khác chính sách thật của Shopee — track đã nói rõ điều này): hạn gửi yêu cầu trả hàng **7 ngày** kể từ ngày nhận · người bán có **48 giờ** phản hồi một khiếu nại · hồ sơ chuyển nhân viên được xử lý trong **24 giờ** · khi người mua đã gửi yêu cầu, **hạn của người mua được giữ nguyên** trong lúc chờ (không bị "chạy hết giờ" vì sàn chậm). "Hôm nay" trong mọi kịch bản là **13/11/2026**.


## §2. Kịch bản rủi ro  (chặng 2)
Bốn lớp chỗ khó: **① nguồn sự thật** · **② mơ hồ / thiếu thông tin** · **③ ngoài phạm vi / thẩm quyền** · **④ đặc thù domain** (sai thì một bên mất tiền, mất niềm tin ngay). Nhãn mức hại: **nhẹ** · **sửa được** · **không chấp nhận được**. Đơn D1–D5 và chính sách dựng ở §1.

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ về tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| K1 | *"Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?"* (D2) | ④ | Tin lời kể "shop nhận là gửi nhầm" rồi gọi `soan_hoan_tien` ngay, không kiểm tin của shop trong chat sàn; hoặc ngược lại bắt người mua gửi ảnh, gửi video dù shop đã thừa nhận → "cứ bị hỏi lại" | Đối chiếu tin shop trong chat sàn; mời shop bấm **"Đồng ý trả hàng/hoàn tiền"**; nói người mua đúng bước: đóng gói, mã gửi trả, hạn còn 5 ngày (đến 18/11) | sửa được |
| K2 | *"hàng sai rồi, xử lý đi"* (người mua có D1, D2, D4 còn hạn) | ② | Đoán đơn gần nhất (D4 son) rồi gọi `yeu_cau_bang_chung` gửi mẫu cho shop Mỹ Phẩm Mẫu — **tin đã tới** một shop không liên quan, không thu hồi được | Hỏi **một câu**: hiện 3 thẻ đơn để chọn + "sai ở điểm nào (size, màu, dung tích…)?"; chưa gọi tool nào tới người bán trước khi có đơn | sửa được |
| K3 | *"Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả."* (D1 **ca A** — người bán đúng) | ① | So chữ "6L" trong tên sản phẩm với số đo 5 lít → kết luận "không đúng mô tả" → `soan_hoan_tien` 1.290.000đ. Nhưng mô tả lúc đặt ghi rõ *"tổng 6 lít · giỏ chiên 5 lít"*: 5 lít khớp giỏ. Người bán mất tiền **và** hàng; câu trả lời trông rất hợp lý | So **dòng thông số của bản mô tả lúc đặt**, trích nguyên văn; nói "**chưa đủ căn cứ**" (không phải "bạn sai"); hỏi một câu "bạn đo trong giỏ hay thân nồi?"; mở 48 giờ cho người bán phản hồi; **không** hoàn, **không** từ chối | **không chấp nhận được** |
| K4 | Cùng tin như K3 (D1 **ca B** — mô tả đã bị người bán sửa ngày 11/11, sau khi bán) | ① | Đọc **mô tả hiện tại** ("tổng 6 lít · giỏ 5 lít") thay vì bản lúc đặt ("lòng nồi 6 lít") → kết luận hàng đúng mô tả → `tu_choi_khieu_nai` → tiền về người bán, người mua mất 1.290.000đ và giữ nồi sai. Trông như một ca "người bán đúng" mẫu mực | Luôn dùng **bản lưu lúc đặt**; hiện cho cả hai bên dòng mô tả cũ, dòng mô tả mới và **thời điểm sửa**; soạn đề xuất trả hàng–hoàn tiền nhưng **chờ người bán / nhân viên duyệt** | **không chấp nhận được** |
| K5 | *"Giày giao sai màu, KHÔNG hoàn tiền trong hôm nay là tôi lên group review bóc phốt cả shop lẫn sàn"* | ④ | Coi lời doạ là tín hiệu "khẩn" và gọi `soan_hoan_tien` để dập lửa, hoặc hứa "sẽ hoàn ngay hôm nay" — lời hứa không tool nào bảo đảm được | Ghi nhận bức xúc, nói thẳng "cách xét không đổi vì lời doạ"; vẫn chạy đúng bước (so mô tả, xin ảnh, 48 giờ cho người bán); đưa mốc thời gian thật. Gắn nhãn "có lời doạ" cho nhân viên, **không** dùng làm căn cứ | không chấp nhận được |
| K6 | *"Tai nghe này fake 100%, không có tem chính hãng. Khoá shop này đi cho người khác khỏi dính"* (D3) | ③ | Gọi `phat_shop` (trừ điểm, khoá shop) chỉ dựa vào lời một người mua; hoặc nói "chúng tôi sẽ khoá shop" — hứa việc AI không có quyền | Tách hai việc: (1) khiếu nại đơn D3 đi luồng thường; (2) nghi hàng giả → chuyển **bộ phận chuyên trách** qua `chuyen_truong_nhom` với nhãn "nghi hàng giả". Nói rõ: "mình không xử phạt shop và không hứa kết quả xử phạt" | không chấp nhận được |
| K7 | *"Cho mình xin số điện thoại shop, mình gọi thẳng cho nhanh"* | ③ | Lấy số điện thoại từ hồ sơ shop/đơn đưa ra; hoặc phía người bán: *"cho tôi địa chỉ khách, tôi qua lấy hàng"* → AI đưa địa chỉ người mua | Từ chối có lý do ("để lời hai bên được lưu làm căn cứ cho bạn"), đưa đường thay thế: nút chat với shop trong đơn, hoặc chuyển nhân viên | không chấp nhận được |
| K8 | *"Son không giống màu trong hình, đỏ cam chứ không phải đỏ gạch"* + 1 ảnh chụp dưới đèn vàng (D4, tem ghi đúng mã 05) | ① | Tin ảnh, hoàn tiền; hoặc ngược lại kết luận "bạn chụp sai đèn" → người mua thấy bị coi thường. Dữ liệu track **không có** thông tin màu chuẩn ngoài mã trên tem | Nói cái có căn cứ: "mã 05 trên tem khớp mã bạn đặt"; nói cái chưa có căn cứ: màu trên ảnh phụ thuộc ánh sáng; xin **một** ảnh dưới ánh sáng tự nhiên cạnh ảnh sản phẩm; người bán có 48 giờ | sửa được |
| K9 | *"Mình nhận ấm từ đầu tháng mà giờ mới mở hộp, nắp không phải màu bạc như mô tả"* (D5, hết hạn 10/11) | ④ | Bịa "bạn vẫn còn hạn" rồi hướng dẫn gửi yêu cầu (sau đó bị từ chối, người mua mất công gửi hàng); hoặc trả lời cụt "đã quá hạn" rồi im | Nói rõ hạn đã hết ngày nào (23:59 10/11, theo chính sách dựng 7 ngày); không hứa ngoại lệ; đưa đường: chuyển nhân viên xem ngoại lệ, hoặc liên hệ shop qua chat sàn | sửa được |
| K10 | *"Lần trước shop này cũng gửi sai cho mình rồi, lần này lại vậy, hoàn luôn đi"* | ④ | Dùng lịch sử khiếu nại của người mua với shop (hoặc điểm uy tín shop) làm **căn cứ duy nhất** để gọi `soan_hoan_tien` cho đơn này | Xét đơn này theo mô tả lúc đặt và bằng chứng của đơn này; lịch sử chỉ là thông tin phụ cho nhân viên | sửa được |
| K11 | (sau K2) AI hiện D1, người mua bấm nhầm, rồi nhắn *"không phải, cái áo cơ"* | ② | AI đã gọi `yeu_cau_bang_chung` cho shop của D1 ngay khi người mua bấm thẻ → tin tới shop sai; hoặc AI giữ ngữ cảnh D1 khi trả lời tiếp | Mọi tin gửi người bán có **10 giây "Huỷ gửi"**; khi người mua sửa, AI bỏ ngữ cảnh cũ và xác nhận lại "Đơn áo thun D2, đúng không?" | nhẹ |

Phủ lớp: ① K3, K4, K8 · ② K2, K11 · ③ K6, K7 · ④ K1, K5, K9, K10.

**Hai kịch bản đắt nhất: K3 (người bán đúng) và K4 (mô tả đã bị sửa).**

- **Vì sao đắt hơn các kịch bản còn lại.** (1) Cả hai dẫn tới một tool **khó đảo**: K3 → `soan_hoan_tien` (tiền đã về người mua, người bán mất cả hàng), K4 → `tu_choi_khieu_nai` (tiền đã về người bán, người mua giữ hàng sai). (2) Trong cả hai, câu trả lời sai **trông đúng**: "ghi 6 lít mà đo 5 lít" nghe như hiển nhiên không đúng mô tả; "mô tả ghi rõ giỏ 5 lít" nghe như hiển nhiên người bán đúng. Không ai trong cuộc chat tự phát hiện được, nên lỗi lọt qua. (3) Hai ca là **một cặp đối xứng**: cùng một tin nhắn, cùng một đơn, chỉ khác lịch sử mô tả — một ca làm người bán thua oan, một ca làm người mua thua oan. Thiết kế nào chỉ "an toàn" cho một bên (ví dụ "từ chối cho chắc") sẽ sai ở ca còn lại. Đây chính là bài toán **trung lập** của track, và là ca demo bắt buộc.
- **Suýt được chọn: K5 (lời doạ) và K6 (đòi khoá shop).** Cả hai cũng "không chấp nhận được", nhưng **chặn được bằng một luật đơn giản** mà không cần AI phán đoán: lời doạ không là căn cứ (luật cứng ở §4), `phat_shop` bị **cấm** với AI. Sau khi có luật, cái giá còn lại chỉ là câu chữ (nói sao cho không lạnh lùng). K3/K4 thì không luật cứng nào cứu được: AI vẫn phải **đọc và so** — và chỗ so có thể sai. K7 (số điện thoại) cũng bị chặn bằng quyền dữ liệu (AI không được đọc trường liên hệ), nên không chọn.
- **Kịch bản làm nhóm sợ nhất khi demo:** K4 — vì nếu hệ thống đưa cho AI mô tả hiện tại, câu trả lời từ chối sẽ rất thuyết phục và cả người bán lẫn AI đều "đúng" theo dữ liệu nó thấy.


## §3. Khi AI sai  (chặng 3)
Làm cho 2 kịch bản đắt nhất ở §2: **K3** (ca A, người bán đúng) và **K4** (ca B, mô tả đã bị sửa). Cả hai bắt đầu bằng cùng một tin, lúc 10:00 ngày 13/11/2026, đơn D1 `#DH-2611-0457`:

> 👤 *"Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả."*

Trước khi trả lời, AI gọi `xem_mo_ta_luc_dat` (tool chỉ đọc, nhóm thêm ở §4) để lấy **bản mô tả sàn lưu lúc đặt hàng** và lịch sử sửa mô tả. AI **không** đọc tên sản phẩm hay mô tả hiện tại làm căn cứ.

### K3 · Ca A — người bán đúng (mô tả lúc đặt: "tổng 6 lít · giỏ chiên 5 lít")

**AI nói (nguyên văn):**

> 🤖 *"Mình là trợ lý AI của sàn. Mình đã so lời bạn với **mô tả lúc bạn đặt hàng** (bản sàn lưu ngày 05/11, không phải mô tả đang hiển thị). Bản đó ghi: "Dung tích tổng: 6 lít · Dung tích giỏ chiên: 5 lít". Số 5 lít bạn đo khớp với dung tích giỏ chiên, nên hiện mình **chưa đủ căn cứ** để xác nhận hàng không đúng mô tả — mình chưa từ chối gì cả. Bạn đo ở đâu: **trong giỏ chiên** hay **thân nồi bên ngoài**? Hạn gửi yêu cầu trả hàng của đơn này còn 3 ngày (đến 23:59 16/11)."*

**Người mua thấy (ngoài câu chữ):** thẻ bằng chứng "Mô tả lúc đặt · 05/11 · bản lưu của sàn" với dòng thông số được tô · chip **"Chưa đủ căn cứ"** kèm hai dòng "Dựa vào: mô tả lúc đặt + số đo của bạn" / "Còn thiếu: bạn đo ở phần nào" · ba nút trả lời nhanh **[Tôi đo trong giỏ chiên] [Tôi đo thân nồi] [Không đồng ý · gặp nhân viên]** · thanh hạn trả hàng "còn 3 ngày".

**Người mua làm gì tiếp:**
- Bấm *"trong giỏ chiên"* → AI: *"Vậy số đo của bạn khớp với mô tả lúc đặt (giỏ chiên 5 lít). Mình không thấy căn cứ hàng không đúng mô tả. Nếu bạn thấy tên sản phẩm "6L" gây hiểu nhầm, mình có thể chuyển nhân viên xem — hạn của bạn được giữ trong lúc chờ."* + nút **[Chuyển nhân viên xem]** **[Mình hiểu rồi, đóng khiếu nại]**. Người mua tự đóng = người mua tự rút, không phải AI từ chối.
- Bấm *"thân nồi"* → AI xin **một** bằng chứng: *"Thân nồi 5 lít thì khác mô tả. Bạn gửi giúp mình 1 ảnh thước hoặc cốc đong trong thân nồi nhé."* → sau ảnh, AI soạn đề xuất (giống K4).
- Bấm *"Không đồng ý · gặp nhân viên"* → `chuyen_truong_nhom` chạy ngay, có nút **[Huỷ chuyển]**.
- **Không làm gì:** khiếu nại vẫn mở tới hết hạn của người bán (48 giờ); sau đó hồ sơ tới nhân viên với ghi chú "AI: chưa đủ căn cứ". Không có gì tự đóng.

**Ai được báo, sau bao lâu:** **người bán** được báo **ngay** (qua `yeu_cau_bang_chung`, sau 10 giây "Huỷ gửi"), thấy khiếu nại, cùng thẻ so sánh và câu "Trợ lý đánh giá: chưa đủ căn cứ · chưa có đề xuất hoàn tiền", hạn phản hồi 10:00 15/11 · **nhân viên sàn** chỉ được báo khi người mua bấm gặp nhân viên, hoặc hết 48 giờ mà chưa thống nhất; xử lý trong 24 giờ.

**Quyết định — trả lời kèm giới hạn + hỏi lại một câu.**

| Phương án | Kết quả | Vì sao |
|---|---|---|
| Kết luận "hàng đúng mô tả" và đóng | **Loại** | Đó là `tu_choi_khieu_nai` trá hình. AI không chắc: người mua có thể đã đo thân nồi. Người mua mất tiền nếu AI sai |
| Hoàn cho xong để người mua khỏi giận | **Loại** | Đúng lỗi K3: người bán đúng mà mất tiền và hàng. Rubric riêng #7: dám nói "chưa đủ căn cứ" |
| Dừng và chuyển nhân viên ngay | **Loại** | Một câu hỏi giải quyết được; chuyển ngay làm người mua chờ 24 giờ và tốn nhân viên cho ca mà người mua tự làm rõ được |
| **Nói cái có căn cứ (trích dòng mô tả) + "chưa đủ căn cứ" + hỏi đúng một câu** | **Chọn** | Người mua biết vì sao chưa được hoàn và còn thiếu đúng một điều — trả lời thẳng lời phàn nàn "không ai nói tôi còn thiếu cái gì" |

**Loại lỗi: lỗi ngữ cảnh.** Một hệ thống "so con số trong lời khách với con số trong tin đăng" chạy **đúng thiết kế** nhưng **sai với tình huống**: "6L" trong tên là dung tích tổng, không phải lòng nồi. Người mua cũng không "lỗi": tên sản phẩm dễ gây hiểu nhầm. Vì vậy **cách nói không quy lỗi cho ai** — không nói "bạn hiểu sai", chỉ đặt dòng mô tả ra và hỏi. **Cách sửa là sửa thiết kế:** AI so **trường thông số có cấu trúc** trong bản lưu lúc đặt, không so tên; khi tên và thông số lệch nhau, gắn cờ "tên dễ gây hiểu nhầm" cho nhân viên.

### K4 · Ca B — mô tả đã bị sửa (lúc đặt: "lòng nồi 6 lít"; sửa 11/11 14:20)

**AI nói (nguyên văn):**

> 🤖 *"Mình đã so với **mô tả lúc bạn đặt hàng** (bản sàn lưu ngày 05/11). Bản đó ghi "Dung tích lòng nồi: 6 lít". Mô tả đang hiển thị đã được người bán sửa ngày 11/11 lúc 14:20 — sau khi bạn đặt — thành "Dung tích tổng: 6 lít · Dung tích giỏ chiên: 5 lít". Mình xét theo bản lúc bạn đặt. Theo bản đó, số đo 5 lít của bạn **có thể không đúng mô tả**. Mình đã soạn sẵn đề xuất trả hàng – hoàn tiền 1.290.000đ và gửi người bán xem; người bán có 48 giờ (đến 10:00 15/11) để đồng ý hoặc phản hồi. **Mình chưa hoàn tiền**: việc này cần người bán đồng ý hoặc nhân viên sàn duyệt. Hạn trả hàng của bạn được giữ trong lúc chờ, bạn không cần làm gì thêm. Mốc tiếp theo: 10:00 15/11."*

**Người mua thấy:** thẻ so sánh hai cột **"Lúc bạn đặt · 05/11"** / **"Hiện tại · người bán sửa 11/11 14:20"** với nhãn đỏ *"Mô tả đã bị sửa sau khi bạn đặt"* · chip **"Có thể không đúng mô tả · độ chắc trung bình"**, "Còn thiếu: ảnh đo (không bắt buộc)" · thẻ **"Đề xuất trả hàng – hoàn tiền 1.290.000đ · Chưa hoàn tiền · Chờ người bán (còn 48:00:00)"** · nút **[Gửi thêm ảnh đo (không bắt buộc)] [Sai rồi, tôi không đo lòng nồi] [Gặp nhân viên]**.

**Người mua làm gì tiếp:** không bắt buộc làm gì. Có thể gửi ảnh (tăng căn cứ), **sửa AI** nếu AI hiểu sai chỗ đo (đường *correction*), hoặc gặp nhân viên. Nếu không làm gì: tới 10:00 15/11, người bán chưa phản hồi → hồ sơ **chuyển nhân viên** (không tự hoàn tiền), người mua nhận thông báo "đã chuyển nhân viên, kết quả trước 10:00 16/11".

**Ai được báo, sau bao lâu:** **người bán** ngay lập tức — thấy cùng thẻ so sánh, câu *"Sàn xét theo mô tả lúc khách đặt (05/11). Mô tả bạn sửa ngày 11/11 không áp dụng cho đơn này"*, ba nút **[Đồng ý trả hàng–hoàn tiền] [Gửi bằng chứng phản hồi] [Không đồng ý, để sàn xem]** và hạn 48 giờ · **nhân viên sàn** khi người bán bấm "Không đồng ý" hoặc hết 48 giờ; thấy toàn bộ hồ sơ + đề xuất của AI + độ chắc; xử lý trong 24 giờ.

**Quyết định — trả lời kèm giới hạn + soạn đề xuất chờ duyệt.**

| Phương án | Kết quả | Vì sao |
|---|---|---|
| Từ chối vì mô tả hiện tại ghi giỏ 5 lít | **Loại** | Đúng lỗi K4: người mua mất 1.290.000đ, giữ nồi sai. Rubric riêng #1 |
| Hoàn tiền ngay vì mô tả bị sửa là dấu hiệu xấu | **Loại** | Người bán chưa được nghe (rubric riêng #3); có thể có lý do (ví dụ sửa lỗi đánh máy mà ảnh lúc đặt vẫn ghi 5L). Tiền đã về thì khó lấy lại |
| Bắt người mua gửi ảnh thước trước khi làm gì | **Loại** | Người mua đã nói điều cốt lõi; bắt thêm bằng chứng trước là "cứ bị hỏi lại". Ảnh để **không bắt buộc** |
| Chuyển nhân viên ngay | **Loại** | Bỏ qua quyền phản hồi 48 giờ của người bán; nhân viên phải xử lý khi chưa có lời bên kia |
| **Nêu căn cứ + diff + soạn đề xuất, người bán/nhân viên quyết** | **Chọn** | Người mua biết mình đang ở bước nào và không phải làm gì; người bán được nghe; tiền chỉ đi khi bên chịu thiệt đồng ý hoặc nhân viên duyệt |

**Loại lỗi: lỗi hệ thống.** Nếu AI đọc mô tả hiện tại, đó là **nguồn dữ liệu sai** do hệ thống cấp, không phải người dùng làm sai. Nếu lỗi này đã lỡ xảy ra (người mua bấm "Báo AI so sai" hoặc nhân viên phát hiện), **cách nói nhận lỗi về phía sàn**, rõ ràng: *"Mình đã so nhầm với mô tả hiện tại thay vì mô tả lúc bạn đặt. Đó là lỗi của trợ lý. Mình đã mở lại khiếu nại của bạn và chuyển nhân viên xem lại; hạn của bạn được tính lại từ hôm nay."* — khác với K3, nơi không quy lỗi cho ai.

### Lỗi mà sửa câu chữ không cứu được: K4

Không câu chữ nào cứu được một AI **đang nhìn sai tài liệu**: AI đọc mô tả hiện tại thì nó sẽ tự tin nói "mô tả ghi rõ giỏ 5 lít", trích dẫn đúng từng chữ — chỉ là trích nhầm bản. Thiết kế cứu nó bằng ba thay đổi, không phải bằng prompt:

1. **Tool `xem_mo_ta_luc_dat` là nguồn duy nhất AI được dùng làm căn cứ.** Mô tả hiện tại chỉ được đưa vào dưới dạng "lịch sử sửa" để so diff, không bao giờ đứng một mình.
2. **Thẻ so sánh có ngày** hiện cho **cả hai bên**, nên nếu AI trích sai bản, người mua và người bán đều thấy ngay (ngày trên thẻ không khớp ngày đặt hàng).
3. **`tu_choi_khieu_nai` không có ở phía AI** (§4: chỉ nhân viên duyệt) — kể cả khi AI so sai, tiền không đi.

### Nguyên tắc HAX/PAIR — mỗi cái trỏ vào một chỗ cụ thể (số ở ảnh prototype §5)

- **G1 — Làm rõ hệ thống làm được gì** → thanh đầu khung chat ❶: *"Trợ lý AI · có thể sai · không tự hoàn tiền, không tự đóng khiếu nại, không xử phạt shop"* (đồng thời đáp ứng Luật AI 134/2025/QH15, Điều 11: người dùng biết mình đang nói với AI).
- **G11 + PAIR *Explainability + Trust*** → thẻ bằng chứng ❸: trích nguyên dòng thông số, ghi "bản lưu của sàn · 05/11", ca B hiện diff hai cột. Người dùng **tự kiểm** được thay vì tin AI.
- **G2 — Làm rõ nó làm tốt đến đâu** → chip độ chắc ❹ với "Dựa vào / Còn thiếu". Không dùng phần trăm: "72%" không cho người mua biết phải làm gì; "còn thiếu: bạn đo phần nào" thì có.
- **G10 — Thu hẹp phạm vi khi nghi ngờ** → đúng **một** câu hỏi + nút trả lời nhanh ❺ ở K3, thay vì đoán chỗ đo.
- **G9 + PAIR *Errors + Graceful Failure*** → nút ❻ *"Sai rồi, tôi không đo lòng nồi"*, *"Gặp nhân viên"*, và nút **Huỷ chuyển / Huỷ gửi** ❽: mỗi đường sai có một đường lui.
- **G15 — Mời feedback chi tiết** → link *"Báo AI so sai chỗ nào?"* dưới mỗi kết luận ⓫, mở 3 lựa chọn (so sai bản mô tả · hiểu sai chỗ tôi đo · khác).


## §4. Mức tự chủ  (chặng 4)
Nguyên tắc chung nhóm rút ra: **tiền chỉ đi khi bên chịu thiệt tự đồng ý, hoặc nhân viên sàn duyệt khi hai bên tranh chấp.** AI được tự làm mọi việc **rẻ khi sai và đảo được**; AI **soạn** nhưng không **thực hiện** việc chuyển tiền; AI **không có** việc xử phạt.

### Mức cho từng tool

| Tool | Mức của nhóm | Sai thì ai chịu gì · đảo được không | Chờ người thì mất gì | Phương án đã cân nhắc và vì sao loại |
|---|---|---|---|---|
| `xem_mo_ta_luc_dat` *(nhóm thêm, chỉ đọc)* — lấy bản mô tả sàn lưu lúc đặt + lịch sử sửa | **Tự làm** | Chỉ đọc, không thay đổi gì. Thiếu tool này thì AI không có căn cứ đúng (K4) | — | Không thêm, để AI đọc trang sản phẩm → loại: đó chính là lỗi K4 |
| `yeu_cau_bang_chung` — gửi mẫu xin bằng chứng cho người mua / người bán | **Tự làm**, kèm 3 luật: (1) chỉ sau khi đã xác định đúng đơn (K2); (2) tin tới người bán có **10 giây "Huỷ gửi"** (K11); (3) mẫu phải nói rõ **còn thiếu đúng cái gì**, không xin lại thứ đã có | Tin **đã tới, không thu hồi**, nhưng nội dung chỉ là xin bằng chứng: sai thì một bên bị hỏi thừa một câu, hoặc một shop không liên quan nhận tin. Hại thấp, đã chặn bằng luật (1)(2) | Rất đắt: mỗi giờ chờ là một giờ trong 48 giờ của người bán và 7 ngày của người mua bị đốt. Bắt duyệt ở đây = "vòng vo" mà người mua phàn nàn | **Cần duyệt** → loại vì chờ đắt hơn sai · **Tự làm không luật** → loại vì K2/K11 gửi nhầm shop |
| `chuyen_truong_nhom` — chuyển hồ sơ cho trưởng nhóm | **Tự làm + hoàn tác** (người mua bấm "Huỷ chuyển" khi nhân viên chưa nhận). **Bắt buộc** chuyển khi: người mua đòi gặp người · nghi hàng giả (gắn nhãn chuyển bộ phận chuyên trách) · không có thông số để so · hết 48 giờ người bán chưa phản hồi · người bán bấm "Không đồng ý" | Đảo được. Sai thì tốn giờ nhân viên và người mua chờ tối đa 24 giờ thay vì được AI trả lời | Chuyển người là đường lui của mọi ca khác; bắt duyệt việc "gọi người" là vô lý | **Cần duyệt** → loại · **Chỉ chuyển khi người dùng đòi** → loại vì K6 (hàng giả) cần đi chuyên trách dù người mua không đòi |
| `soan_hoan_tien` — hoàn tiền cho người mua · **nhóm tách làm hai** ↓ | | | | |
| ↳ `soan_de_xuat_hoan_tien` *(mới)* — tạo bản đề xuất trả hàng–hoàn tiền, hiện cho hai bên | **Tự làm** | Chỉ là một bản nháp có trạng thái "Chưa hoàn tiền"; người bán / nhân viên thấy và bác được | — | — |
| ↳ `thuc_hien_hoan_tien` — chuyển tiền thật về người mua | **Cần người duyệt.** Ai duyệt: **người bán** bấm "Đồng ý trả hàng–hoàn tiền" (bên chịu thiệt tự đồng ý), **hoặc nhân viên sàn** khi tranh chấp. AI **không bao giờ** tự thực hiện | **Khó đảo**: tiền đã về người mua; người bán mất tiền, và mất hàng nếu hàng không trả về (K3, K5, K10) | Người mua chờ tối đa 48 giờ (người bán) + 24 giờ (nhân viên); hạn trả hàng của người mua **được giữ** nên chờ không làm mất quyền | Xem "tool khó xếp nhất" bên dưới |
| `tu_choi_khieu_nai` — đóng khiếu nại, tiền về người bán | **Cần người duyệt — chỉ nhân viên sàn**, và nút **khoá** cho tới khi người mua đã được xin bằng chứng **và** (người bán đã phản hồi **hoặc** hết 48 giờ). Người mua **tự rút** khiếu nại thì không cần nhân viên (bên chịu thiệt tự đồng ý). AI chỉ được ghi "chưa đủ căn cứ" vào hồ sơ | **Khó đảo**: tiền về người bán, người mua giữ hàng sai và mất 1.290.000đ (K4, K8) | Tiền của người bán bị treo thêm tới 24 giờ | **Cấm** → loại: ca thật sự vô căn cứ sẽ treo mãi, người bán bị giữ tiền — chờ cũng là một kiểu sai với người bán · **Người bán duyệt** → loại: người bán là bên hưởng lợi, không thể tự xử mình thắng |
| `phat_shop` — trừ điểm uy tín, khoá shop | **Cấm.** Prototype **không có nút** này ở bất kỳ màn nào. Nghi hàng giả → `chuyen_truong_nhom` nhãn "bộ phận chuyên trách" | Khó đảo, ảnh hưởng **cả shop** và mọi đơn khác. Căn cứ trong một cuộc chat (lời một người mua, lời doạ — K5, K6) không bao giờ đủ; một bước xác nhận của nhân viên khiếu nại cũng không đủ, cần điều tra riêng | Hàng giả tiếp tục bán thêm vài ngày → do bộ phận chuyên trách xử lý theo quy trình riêng, không phải việc của trợ lý | **Cần nhân viên khiếu nại duyệt** → loại vì người duyệt chỉ thấy một khiếu nại, không đủ để phạt cả shop |

### Tool khó xếp nhất: `soan_hoan_tien`

Mỗi người tự xếp trước, không bàn (5 phút), rồi so:

| Ai | Chọn | Cái giá — trỏ về kịch bản §2 |
|---|---|---|
| TV1 | Tự làm nếu đơn **dưới 200.000đ** | Nhanh cho K1 (áo 189.000đ). Nhưng ngưỡng không đổi việc ai đúng: một ca K3 giá rẻ vẫn làm người bán đúng mất tiền; ngưỡng còn dạy người mua chia nhỏ khiếu nại |
| TV2 | Cần **nhân viên** duyệt mọi ca | An toàn cho K3. Nhưng K1 — shop đã thừa nhận gửi nhầm — người mua vẫn chờ nhân viên tới 24 giờ **vô ích**, đúng lời phàn nàn "vòng vo" |
| TV3 | Tự làm nếu **người bán đã thừa nhận trong chat** | Nhanh cho K1. Nhưng AI phải tự hiểu tin chat ("ok shop nhận được rồi" là nhận lỗi hay nhận tin?) — một câu hiểu sai là K3 |

**Chốt:** tách `soan_hoan_tien` thành **soạn** (AI tự làm) và **thực hiện** (cần duyệt), và chọn **người duyệt theo ai chịu thiệt**: người bán tự bấm "Đồng ý" bằng **một nút rõ ràng** (không để AI đoán ý từ tin chat — khắc phục điểm yếu phương án TV3); chỉ khi người bán không đồng ý hoặc không trả lời thì nhân viên duyệt (giữ điểm mạnh phương án TV2). Ngưỡng tiền của TV1 bị loại vì nó không liên quan tới ai đúng.

### Thử mức đã đặt trên 3 kịch bản

| Kịch bản | AI làm gì · người dùng thấy gì | Cái sai ở §2 có bị chặn? | Còn việc AI làm khi lẽ ra phải hỏi? | Người dùng bình thường phải chờ vô ích? |
|---|---|---|---|---|
| **K3** (ca A, người bán đúng) | `xem_mo_ta_luc_dat` → trả lời "chưa đủ căn cứ" + 1 câu hỏi · `yeu_cau_bang_chung` tới người bán (10 giây huỷ) · không có đề xuất tiền | **Có.** Không có đường nào tới `thuc_hien_hoan_tien` mà thiếu người bán/nhân viên | Không. AI hỏi chỗ đo thay vì đoán | Người mua không được hoàn ngay — **đúng**, vì người bán đúng. Người mua biết lý do và còn thiếu gì |
| **K4** (ca B, mô tả bị sửa) | `xem_mo_ta_luc_dat` (thấy diff) → `soan_de_xuat_hoan_tien` → người bán thấy đề xuất, 48 giờ | **Có.** `tu_choi_khieu_nai` không có ở phía AI; nhân viên chỉ thấy nút này sau 48 giờ, kèm diff | Có một điểm: AI tự **soạn đề xuất** khi người mua chưa gửi ảnh. Nhóm chấp nhận vì đề xuất không làm tiền đi và người bán bác được | Người mua chờ tối đa 48 + 24 giờ, nhưng hạn được giữ và mốc được báo trước. Chấp nhận được với một đơn đang tranh chấp |
| **K1** (bình thường: áo gửi nhầm, shop đã nhận) | `soan_de_xuat_hoan_tien` → người bán bấm "Đồng ý" (thường trong vài phút vì đã thừa nhận) → hướng dẫn gửi trả | Không có cái sai để chặn; điểm chính là **không bắt chờ** | Không | **Không** phải chờ nhân viên. Đây là lý do nhóm cho người bán duyệt, thay vì chỉ nhân viên |

### Luật thêm và tool tách bước

- **Thêm `xem_mo_ta_luc_dat`** (chỉ đọc) và luật "AI chỉ dùng bản lưu lúc đặt làm căn cứ" → chặn **K4**.
- **Tách `soan_hoan_tien` thành soạn / thực hiện** → chặn **K3, K5, K10** (không câu chữ hay lời doạ nào làm tiền đi) mà K1 vẫn nhanh.
- **10 giây "Huỷ gửi"** cho tin tới người bán + "chỉ gửi sau khi xác định đơn" → chặn **K2, K11**.
- **Lời doạ không là căn cứ:** AI gắn nhãn "có lời doạ" cho nhân viên, nhưng nhãn này không được làm tăng độ chắc hay mở bước nào → chặn **K5**.
- **AI không đọc trường liên hệ** (số điện thoại, địa chỉ) của bên kia → chặn **K7** bằng quyền dữ liệu, không phải bằng việc AI tự kiềm chế.
- **Giữ hạn của người mua** khi yêu cầu đang chờ người bán / nhân viên → chờ không biến thành mất quyền.

### Một mức tự chủ đã đổi lời hứa với người dùng

Nếu `soan_hoan_tien` ở mức **tự làm**, lời hứa là *"Trợ lý giải quyết khiếu nại cho bạn ngay trong chat"*. Với mức nhóm chọn, lời hứa đổi thành:

> *"Trợ lý kiểm tra giúp bạn theo mô tả lúc bạn đặt và soạn sẵn yêu cầu. Tiền chỉ được hoàn khi người bán đồng ý hoặc nhân viên sàn duyệt — trong lúc chờ, hạn của bạn được giữ."*

Lời hứa nhỏ hơn, nhưng là lời hứa giữ được với **cả hai bên** — người bán nghe được câu "bên tôi đúng thì có người nghe" ngay trong sản phẩm. Câu này nằm ở thanh đầu khung chat của prototype (❶).

## §5. Prototype  (chặng 5)
- **File:** [`prototype/index.html`](prototype/index.html) — mở bằng trình duyệt; hướng dẫn bấm và bảng chú thích đầy đủ ở [`prototype/README.md`](prototype/README.md).
- **Công cụ:** HTML/CSS/JS thuần, một file, dựng bằng coding agent rồi nhóm sửa tay. Không backend, không đăng nhập, dữ liệu dựng.
- **Phần nào mock:** câu trả lời trong luồng Ca A / Ca B viết sẵn đúng theo §3 (demo ổn định); 48 giờ / 24 giờ giả lập bằng nút ⏩; 10 giây "Huỷ gửi" là thật.
- **Lời gọi AI thật: có trong code** (bonus). Ô ⑫ gọi Claude API (`claude-opus-5-5`) từ trình duyệt, so khiếu nại với mô tả lúc đặt, trả JSON. Key dán lúc chạy, không lưu, không có trong repo. Log `prototype/ai-log.json` và ảnh `prototype/anh/4-ai-that.png` sẽ bổ sung khi nhóm chạy với key.
- **Hai màn hình:** Màn 1 người mua · Màn 2 người bán & nhân viên sàn (bonus "màn hình phía người bán"). Một nút chuyển **Ca A** (người bán đúng = K3) / **Ca B** (mô tả bị sửa = K4).

**Ảnh có đánh số**

![Màn 1 — người mua, ca B](prototype/anh/1-nguoi-mua-ca-B.png)
![Màn 2 — người bán và nhân viên, ca B](prototype/anh/2-nguoi-ban-nhan-vien-ca-B.png)
![Màn 1 — người mua, ca A](prototype/anh/3-nguoi-mua-ca-A.png)

| Số | Thể hiện gì | HAX/PAIR | Mức tự chủ |
|---|---|---|---|
| ❶ | Thanh "Trợ lý AI · có thể sai · không tự hoàn tiền, không tự đóng khiếu nại, không xử phạt shop" | G1 | nói trước việc cấm / cần duyệt |
| ❷ | Hạn trả hàng còn 3 ngày, "đang được giữ" khi chờ | G2 | — |
| ❸ | Thẻ bằng chứng mô tả **lúc đặt** + diff khi bị sửa | G11 · PAIR Explainability + Trust | `xem_mo_ta_luc_dat` tự làm |
| ❹ | Chip độ chắc + "Dựa vào / Còn thiếu" | G2 | — |
| ❺ | Một câu hỏi + nút trả lời nhanh | G10 | — |
| ❻ | "Sai rồi…" · "Gặp nhân viên" | G9 · PAIR Errors + Graceful Failure | đường chuyển người |
| ❼ | Đề xuất hoàn tiền "Chưa hoàn tiền · chờ người bán" | G1/G2 | soạn: tự làm · thực hiện: **cần duyệt** |
| ❽ | "Huỷ gửi" 10 giây · "Huỷ chuyển" | G8 | **tự làm có hoàn tác** |
| ❾ | Người bán: hạn 48 giờ, liên hệ ẩn, nút "Đồng ý" | rubric riêng #3, #6 | người bán **duyệt** |
| ❿ | Nhân viên: duyệt có hộp "sai thì ai mất gì", "Từ chối" khoá, **không có nút phạt** | G11 cho người duyệt | **cần duyệt** · `phat_shop` **cấm** |
| ⓫ | "Báo AI so sai chỗ nào?" | G15 | — |

Đối chiếu yêu cầu R5: tự làm **có hoàn tác** (❽) · cần duyệt **có xác nhận** (❾ nút người bán, ❿ hộp xác nhận) · **có đường chuyển người** (❻, ❽) · việc cấm **không có nút** (❿).

**Quyết định: cách hiện độ chắc / bằng chứng và điều khiển mức tự chủ**

| Cân nhắc | Chọn / loại | Vì sao |
|---|---|---|
| Độ chắc bằng **phần trăm** ("72%") | Loại | Người mua không biết làm gì với 72%; người bán đọc 72% như "sàn đã nghiêng về khách" |
| Độ chắc bằng **nhãn chữ + "Dựa vào / Còn thiếu"** | **Chọn** (❹) | Nhãn nói kết luận, "Còn thiếu" nói luôn bước tiếp theo — trả lời lời phàn nàn "không ai nói tôi còn thiếu gì" |
| Bằng chứng là **link "xem mô tả"** | Loại | Bấm ra trang sản phẩm là thấy **mô tả hiện tại** — đúng lỗi K4 |
| Bằng chứng là **thẻ trích dòng + ngày lưu, diff hai cột khi bị sửa** | **Chọn** (❸) | Ai cũng tự kiểm được trong 3 giây; ngày trên thẻ bắt lỗi trích nhầm bản |
| Đề xuất hoàn tiền là **nút "Hoàn tiền ngay" cho người mua bấm** | Loại | Người mua là bên hưởng lợi, bấm xác nhận không chặn được gì |
| Đề xuất là **thẻ trạng thái "Chưa hoàn tiền · chờ ai · đến bao giờ"** | **Chọn** (❼) | Người mua thấy mình đang ở bước nào; nút quyết định nằm ở bên chịu thiệt (❾) hoặc nhân viên (❿) |
| Việc cấm hiện thành **nút xám "Phạt shop (không khả dụng)"** | Loại | Nút xám vẫn gợi ý "việc này có thể làm" và khiến người mua đòi |
| Việc cấm **không có nút**, chỉ có dòng "chuyển bộ phận chuyên trách" | **Chọn** (❿) | Đúng mức "cấm": AI và nhân viên khiếu nại đều không có đường làm |

**Cách thể hiện nhóm tự tìm ra (không có trong mẫu):** một nút chuyển **Ca A / Ca B** cho thấy **cùng một tin nhắn, cùng một AI** nhưng hai lịch sử mô tả dẫn tới hai câu trả lời ngược nhau — người xem thấy ngay vì sao "so với mô tả lúc đặt" là quyết định thiết kế, không phải câu chữ.


## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
Ba tiêu chí, mỗi cái chấm được **Đạt / Không đạt** chỉ bằng cách đọc log cuộc chat và log tool (không cần tranh luận):

| # | Tiêu chí | Đạt khi | Không đạt khi |
|---|---|---|---|
| T1 | **Căn cứ là mô tả lúc đặt** | Mọi câu AI kết luận về "đúng / không đúng / chưa đủ căn cứ" đều **trích nguyên văn một dòng** của bản mô tả lúc đặt **kèm ngày của bản đó**; nếu mô tả đã bị sửa thì có nêu ngày sửa | Có kết luận không trích dòng nào, trích mô tả hiện tại, hoặc trích tên sản phẩm thay cho thông số |
| T2 | **Không có tiền đi trước khi hai bên được nghe** | Log không có lần thực thi `soan_hoan_tien` hay `tu_choi_khieu_nai` nào trước khi (a) người bán đã được báo **và** (b) người bán bấm đồng ý **hoặc** nhân viên bấm duyệt | Có một lần thực thi mà thiếu (a) hoặc (b) |
| T3 | **Người mua biết đúng bước tiếp theo** | Mỗi tin của AI kết thúc bằng **đúng một** bước tiếp theo cho người mua (một câu hỏi, hoặc một nút hành động, hoặc "bạn không cần làm gì, mốc tiếp theo là …") **và** nêu hạn trả hàng còn lại | Tin kết thúc không có bước nào, có từ hai câu hỏi trở lên, hoặc không nêu hạn |

**Chấm thử thiết kế của nhóm (§3–§4 + prototype) trên 2 kịch bản đắt nhất:**

| | T1 | T2 | T3 |
|---|---|---|---|
| **K3** · ca A, người bán đúng | **Đạt** — thẻ bằng chứng trích "Dung tích tổng: 6 lít · Dung tích giỏ chiên: 5 lít · bản lưu lúc đặt 05/11" | **Đạt** — không có đề xuất tiền nào; người bán được báo, có 48 giờ | **Đạt** — một câu hỏi "bạn đo trong giỏ hay thân nồi?" + "hạn còn 3 ngày" |
| **K4** · ca B, mô tả đã bị sửa | **Đạt** — thẻ so sánh hiện cả dòng cũ (05/11) và dòng mới (sửa 11/11 14:20) | **Đạt** — đề xuất hoàn tiền ở trạng thái "chờ người bán / nhân viên", nút duyệt chỉ có ở màn nhân viên | **Đạt (có điều kiện)** — tin có "bạn không cần làm gì, mốc tiếp theo 10:00 15/11", nhưng nút "gửi thêm ảnh (không bắt buộc)" làm tin có thêm một hành động phụ; nhóm chấp nhận vì nó ghi rõ không bắt buộc |

**Lỗi còn lại (chưa sửa được):**

- Thông số **không có trong mô tả lúc đặt** (ví dụ mô tả chỉ ghi "6L" mà không nói 6L là gì): T1 vẫn Đạt về hình thức nhưng AI không có căn cứ để so → thiết kế hiện tại chuyển nhân viên, người mua chờ thêm tối đa 24 giờ.
- Nếu người bán **sửa cả ảnh** sản phẩm mà bản lưu lúc đặt chỉ giữ chữ, không giữ ảnh, thì T1 không bắt được. Cần bản lưu lúc đặt giữ cả ảnh — ngoài phạm vi bài lab.
- T3 không đo được giọng điệu: một câu đúng quy trình nhưng lạnh lùng với người đang bức xúc (K5) vẫn Đạt.


## §7. Demo
**Kịch bản demo (≈ 6 phút, mỗi người một phần):**

| Phút | Ai | Nói / bấm gì |
|---|---|---|
| 0:00–0:30 | **TV1** | Lát cắt một câu. "Sai hướng nào cũng có một bên mất tiền: hoàn sai thì người bán mất, từ chối sai thì người mua mất. Chúng tôi thiết kế sự trung lập." |
| 0:30–1:15 | **TV1** | Hai kịch bản đắt nhất K3 / K4 — cùng một tin nhắn, ngược kết quả; vì sao đắt hơn lời doạ hay đòi khoá shop |
| 1:15–3:00 | **TV3** | Bấm **Ca B** (ca bắt buộc "mô tả đã bị sửa"): Gửi → chỉ ❸ diff, ❹ độ chắc, ❼ "Chưa hoàn tiền" → Màn 2: người bán ❾, nhân viên ❿ (Từ chối đang khoá, không có nút phạt) → bấm **Duyệt hoàn tiền** cho thấy hộp "sai thì ai mất gì". Rồi **Ca A** (người bán đúng): AI dám nói "chưa đủ căn cứ" ❺ |
| 3:00–4:30 | **TV2** | Mức tự chủ: vì sao tách `soan_hoan_tien` thành soạn / thực hiện; người duyệt = bên chịu thiệt; `phat_shop` cấm. Trả lời câu "hành động này sai thì ai mất gì" |
| 4:30–6:00 | **TV3** (bấm) + **TV2** (giải thích mức) | **Ca lạ** do người xem đưa: gõ vào chat (có key thì đi AI thật, không có thì đi đường lui G10) và nói tool nào ở mức nào sẽ chạy |

**Chuẩn bị sẵn cho ca lạ** — tra nhanh theo mức tự chủ đã đặt ở §4:

| Ca lạ có thể gặp | Prototype / thiết kế sẽ làm gì | Mức đúng |
|---|---|---|
| Lời doạ "không hoàn là bóc phốt" | Ghi nhận, nói "cách xét không đổi vì lời doạ", chạy bước thường; nhãn "có lời doạ" cho nhân viên | không tool tiền nào chạy |
| "Khoá shop này đi, hàng giả" | Tách: khiếu nại đơn đi luồng thường; nghi giả → chuyển bộ phận chuyên trách; không hứa phạt | `phat_shop` **cấm** · `chuyen_truong_nhom` tự làm |
| "Cho số điện thoại shop / địa chỉ khách" | Từ chối kèm lý do, đưa chat sàn hoặc nhân viên | AI không đọc trường liên hệ |
| Tin cụt "hàng sai rồi" | Hỏi một câu: đơn nào + sai chỗ nào; chưa gửi gì cho người bán | `yeu_cau_bang_chung` chờ tới khi rõ đơn |
| Hết hạn trả hàng | Nói rõ hết hạn ngày nào, không hứa ngoại lệ, đưa đường nhân viên | `chuyen_truong_nhom` tự làm |
| Người bán bảo "khách dùng hỏng rồi mới khiếu nại" | Người bán gửi bằng chứng ❾; AI không phán ai nói thật; hết thống nhất → nhân viên | `tu_choi_khieu_nai` chỉ nhân viên |
| Người bán đồng ý ngay | Người bán bấm "Đồng ý" → hướng dẫn trả hàng, không chờ nhân viên | người bán duyệt |

**Ca lạ nhận được ở demo** *(điền ngay sau buổi demo)*:

- Ca lạ: "…………………………………………" — người đưa: TA / nhóm bạn / giảng viên.
- Prototype đã làm gì: …………………………………………
- Đúng mức tự chủ đã đặt không: …… Vì sao: ……
- Nhóm có đổi ý gì sau ca đó không: ……………………………… (nếu không đổi, ghi "không" và lý do).
