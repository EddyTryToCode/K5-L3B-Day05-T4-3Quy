# Prototype — Trợ lý AI khiếu nại "hàng không đúng mô tả" (T4 · Nhóm 3Q)

**Mở:** tải repo về, mở `index.html` bằng trình duyệt (Chrome/Edge). Không cần cài gì, không backend, không đăng nhập. Dữ liệu dựng.

**Công cụ:** HTML + CSS + JavaScript thuần, một file, dựng bằng coding agent (Claude Code) rồi nhóm sửa tay.

**Hai màn hình**
- **Màn 1 · Người mua** — chat với trợ lý AI về đơn `#DH-2611-0457` (nồi chiên "6 lít").
- **Màn 2 · Người bán & nhân viên sàn** — người bán thấy khiếu nại + hạn phản hồi 48 giờ; nhân viên sàn thấy hồ sơ khi cần người quyết.

**Nút trên đầu trang:** chọn **Ca A** (người bán đúng) hoặc **Ca B** (mô tả đã bị sửa) · chuyển màn · bật/tắt số chú thích · làm lại ca.

## Bấm qua ca khó (demo 4 phút)

1. Chọn **Ca B**, ở Màn 1 bấm **Gửi** (tin có sẵn) → AI trả lời kèm thẻ so sánh mô tả **lúc đặt / hiện tại**, độ chắc, đề xuất "Chưa hoàn tiền".
2. Chờ 10 giây (hoặc bấm **Huỷ gửi** để thấy đường hoàn tác) → người bán được báo.
3. Sang **Màn 2**: người bán thấy cùng bằng chứng, 3 nút. Bấm **Không đồng ý** (hoặc **⏩ Giả lập hết 48 giờ**) → hồ sơ sang nhân viên → **Duyệt hoàn tiền** mở hộp xác nhận "sai thì ai mất gì".
4. **Làm lại ca**, chọn **Ca A**, bấm **Gửi** → AI nói "chưa đủ căn cứ", hỏi một câu. Bấm **Tôi đo trong giỏ chiên** → AI không hoàn tiền, người mua tự đóng hoặc chuyển nhân viên.
5. Đường sửa AI: ở Ca B bấm **Sai rồi, tôi không đo lòng nồi** → AI hạ độ chắc, chuyển nhân viên, có **Huỷ chuyển**.

## Phần nào mock

Toàn bộ câu trả lời trong luồng Ca A / Ca B là kịch bản viết sẵn theo `spec.md` §3 (để demo ổn định). Thời gian 48 giờ / 24 giờ được giả lập bằng nút ⏩; riêng 10 giây "Huỷ gửi" là thật.

## Lời gọi AI thật (bonus)

Ô **⑫ "Lời gọi AI thật"** bên phải gọi Claude API (`claude-opus-5-5`) trực tiếp từ trình duyệt: gửi mô tả lúc đặt, mô tả hiện tại, lời khiếu nại → model trả JSON (`ket_luan`, `do_chac`, `dong_mo_ta_trich`, `con_thieu`, `buoc_tiep_theo`, `cau_tra_loi`). Có key thì tin **khác tin mồi** gõ ở Màn 1 cũng đi qua AI thật — dùng cho **ca lạ** lúc demo.

- **API key không nằm trong code.** Người demo dán key vào ô mật khẩu lúc chạy; trang không lưu key. **Không commit key.**
- Chỉ gửi dữ liệu dựng.
- Log của lời gọi thật: `ai-log.json` (bấm **Tải log** sau khi gọi; trong log, key đã được ẩn) và ảnh `anh/4-ai-that.png` — *bổ sung khi nhóm chạy với key*.

## Ảnh có đánh số

| Ảnh | Nội dung |
|---|---|
| `anh/1-nguoi-mua-ca-B.png` | Màn 1, Ca B — mô tả bị sửa: thẻ so sánh, độ chắc, đề xuất chưa hoàn tiền |
| `anh/2-nguoi-ban-nhan-vien-ca-B.png` | Màn 2, Ca B — người bán (hạn 48 giờ, liên hệ ẩn) + nhân viên (nút Từ chối đang khoá, không có nút phạt) |
| `anh/3-nguoi-mua-ca-A.png` | Màn 1, Ca A — người bán đúng: "chưa đủ căn cứ" + một câu hỏi |
| `anh/4-ai-that.png` | Kết quả lời gọi AI thật *(nhóm chụp khi chạy với key)* |

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| ❶ | Thanh "Trợ lý AI · có thể sai · không tự hoàn tiền, không tự đóng khiếu nại, không xử phạt shop" | **G1** (+ Luật AI 134/2025 Đ.11) | nói rõ việc cấm / cần duyệt |
| ❷ | Hạn yêu cầu trả hàng còn 3 ngày; "đang được giữ" khi chờ | **G2**, rubric riêng #8 | — |
| ❸ | Thẻ bằng chứng: dòng mô tả **lúc đặt**, ngày lưu; Ca B có diff hai cột và giờ sửa | **G11**, PAIR *Explainability + Trust* | `xem_mo_ta_luc_dat` — tự làm |
| ❹ | Chip độ chắc + "Dựa vào / Còn thiếu" | **G2** | — |
| ❺ | Đúng một câu hỏi + nút trả lời nhanh (Ca A) | **G10** | — |
| ❻ | "Sai rồi, tôi không đo lòng nồi" · "Không đồng ý · gặp nhân viên" | **G9**, PAIR *Errors + Graceful Failure* | đường chuyển người |
| ❼ | Thẻ "Đề xuất hoàn tiền · Chưa hoàn tiền · chờ người bán" | **G1/G2** | `soan_de_xuat_hoan_tien` tự làm → `thuc_hien_hoan_tien` **cần duyệt** |
| ❽ | Toast "Đang gửi cho người bán… 10s · Huỷ gửi"; "Đã chuyển nhân viên · Huỷ chuyển" | **G8** | `yeu_cau_bang_chung`, `chuyen_truong_nhom` — **tự làm có hoàn tác** |
| ❾ | Người bán: hạn phản hồi 48 giờ, liên hệ người mua 🔒 ẩn, nút "Đồng ý trả hàng – hoàn tiền" | rubric riêng #3, #6 | người bán **duyệt** `thuc_hien_hoan_tien` |
| ❿ | Nhân viên: "Duyệt hoàn tiền" có hộp xác nhận "sai thì ai mất gì"; "Từ chối" khoá tới khi người bán được nghe; **không có nút phạt shop** | **G11** cho người duyệt | `tu_choi_khieu_nai` **cần duyệt**; `phat_shop` **cấm** |
| ⓫ | "Báo AI so sai chỗ nào?" → 3 lựa chọn | **G15** | — |
| ⓬ | Lời gọi AI thật, trả JSON có trích dòng mô tả | bonus | AI chỉ đề xuất, không tool tiền nào chạy |
