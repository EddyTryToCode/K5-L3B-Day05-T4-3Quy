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
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.