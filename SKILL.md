---
name: note-xe
description: Đọc ảnh Phiếu tiếp nhận dịch vụ Auto365 Trụ sở chính và điền 1 dòng xe vào sheet XE_VIDEO (file Cẩm nang sản xuất video) qua Google Sheets. Dùng khi anh Sang gửi ảnh phiếu/bill xe vào xưởng hoặc gọi "Note-xe".
---

# Note-xe – điền phiếu tiếp nhận vào XE_VIDEO

## File đích
- Google Sheets: "Cẩm nang sản xuất video thực tế tại Auto365 trụ sở chính"
- spreadsheetId: `1jBuw2tDKROJJ6_FUdugdVoVRlqUNumEsyfyeS1nvCY0`
- Ghi bằng connector Google Sheets (không điều khiển trình duyệt). Locale vi_VN → công thức dùng dấu `;`.
- Trước khi ghi: đọc lại hàng tiêu đề `XE_VIDEO!A4:W4` để chắc vị trí cột chưa đổi. Nếu đã đổi, map theo tên tiêu đề, không theo chữ cột.

## Cột XE_VIDEO (hàng tiêu đề = 4, dữ liệu từ hàng 5)
A Ngày vào · B Biển số · C Xe (hãng · dòng · đời) · D Dịch vụ · E Brand · F Sản phẩm · G Trạng thái xe · H Người quay · I MC · J Trạng thái quay · K Link source · L Góc nội dung · M Editor · N Trạng thái video · … · U Ghi chú · V HUD (tự động) · W Prompt AI (tự động)

## Bước làm
1. **Đọc phiếu** (ảnh): biển số, hãng/dòng xe, màu, ngày giờ nhận, ngày giao dự kiến, cố vấn dịch vụ (CVDV), nhân viên tiếp nhận, các hạng mục/sản phẩm ở mục 4. Chữ viết tay khó đọc → đánh dấu ⚠ và hỏi lại, không đoán.
2. **Xe (cột C)**: luôn ghi đủ `Hãng + Dòng + Phiên bản · Màu`. Phiếu chỉ ghi mã phiên bản (XLS, XLT, Wildtrak…) → tra web ra hãng + dòng (vd XLS → Ford Ranger XLS). Không bao giờ để trống hãng. Không rõ đời/cầu thì chỉ ghi tới phiên bản.
3. **Dịch vụ (cột D)**: chỉ 1 trong 4: `Đèn` · `Phim cách nhiệt` · `PPF` · `Phụ kiện` (mọi hạng mục không phải đèn/phim/PPF đều là Phụ kiện).
4. **Brand (cột E)**: chọn từ danh sách `DANH_MUC!P2:P80` (vd X-Light, AES, Titan Moto, GTR, 3M, NKD, AX, 70mai, Vietmap…). Nếu phiếu không ghi brand, tra sản phẩm trên auto365.vn để xác định.
5. **Sản phẩm (cột F)**: chỉ ghi **tên mã model, không lặp tên hãng** (vd `F10 Turbo V2 – 5500K`, không phải `X-Light F10…`).
   - Kiểm tra `THU_VIEN_SP!B5:B300` đã có tên này chưa.
   - Chưa có → tìm trang sản phẩm trên **auto365.vn**, lấy thông số và thêm 1 dòng vào THU_VIEN_SP: A Mã SP · B Tên mã · C Dịch vụ · D/E/F Thông số HUD · G Bảo hành · H Link trang sản phẩm · I Ghi chú. Chỉ dùng thông số có trên trang chính hãng, không ước lượng. Web không có SKU → đặt mã nội bộ dạng `XL-F10TV2-5500` và ghi chú "mã nội bộ tạm".
6. **Ngày vào (cột A)**: ghi dạng số serial ngày (vd 17/11/2026 = 46343) để giữ định dạng ngày.
7. **Trạng thái xe (G)**: `Đang thi công` nếu xe đang làm/giao trong ngày, ngược lại `Đã vào`. **Trạng thái quay (J)**: `Chưa quay`.
8. **ĐỂ TRỐNG cột H (Người quay) và I (MC)** – anh Sang / Phương Anh tự phân công. Cảnh báo "🟠 CHƯA PHÂN CÔNG QUAY" là bình thường.
9. **Góc nội dung (L)**: gợi ý 1 trong G1–G4 (G1 Lột xác trước/sau · G2 Quy trình cận cảnh · G3 Xử lý xe lỗi · G4 Xe đặc biệt), ưu tiên khớp với 10 chủ đề gợi ý trong CAM_NANG.
10. **Ghi chú (U)**: `CVDV: … · Tiếp nhận: … · Nhận xe [giờ], giao [ngày/trong ngày]`.
11. **Không ghi vào file**: tên khách, số điện thoại, giá tiền (file cả đội xem; video không cần).
12. **Ghi dòng**: tìm hàng đầu tiên từ hàng 5 có A và B trống, ghi các cột A–G, J, L, U. Không đụng cột công thức (T Cảnh báo, V HUD, W Prompt và nhóm thu gọn).
13. **Kiểm tra sau khi ghi**: đọc lại dòng vừa ghi, xác nhận cột V (HUD) hiện đúng thông số (không phải "⚠ Chưa có SP…") và cột T Cảnh báo hợp lý.

## Báo lại cho anh
Ngắn gọn: bảng 1 dòng đã điền (A–G, J, L) + các điểm ⚠ cần xác nhận (chữ khó đọc, brand/hãng chưa chắc, mã SP tạm) + nhắc phân công Người quay/MC. Nếu xe giao trong ngày → nhắc quay trước khi xe ra để tránh "🔴 BỎ SÓT SOURCE".
