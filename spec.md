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
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

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
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

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
