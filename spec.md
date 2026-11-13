# Bài nộp LAB5 — [Track] · Nhóm [tên]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track).
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

## §2. Kịch bản rủi ro  (chặng 2)
- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.

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
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.