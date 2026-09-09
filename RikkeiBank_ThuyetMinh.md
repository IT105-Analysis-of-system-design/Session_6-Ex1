# THỰC HÀNH TỔNG HỢP ACTIVITY DIAGRAM VÀ USE CASE DIAGRAM
## Hệ thống Rút tiền không thẻ tại ATM - RikkeiBank

---

## PHẦN A — ACTIVITY DIAGRAM

### Kịch bản nghiệp vụ
Khách hàng quét mã QR. Hệ thống kiểm tra số dư:
- Nếu **Đủ số dư**: hệ thống vừa **nhả tiền** vừa **gửi SMS báo biến động số dư** cùng lúc (song song), sau đó kết thúc.
- Nếu **Không đủ số dư**: hệ thống hiển thị lỗi rồi kết thúc.

### Bảng phân rã Node — Swimlane

| Loại Node | Tên Node / Tác vụ | Swimlane phụ trách |
|---|---|---|
| Initial Node | Bắt đầu | — |
| Action | Quét mã QR | Khách hàng |
| Decision | Kiểm tra số dư (Đủ / Không đủ) | Hệ thống |
| Action | Hiển thị lỗi *(nhánh Không đủ số dư)* | Hệ thống |
| **Fork** | Tách 2 nhánh song song: (1) Nhả tiền, (2) Gửi SMS báo biến động số dư | Hệ thống |
| Action | Nhả tiền | Hệ thống |
| Action | Gửi SMS báo biến động số dư | Hệ thống |
| **Join** | Gộp lại 2 nhánh song song trước khi kết thúc | Hệ thống |
| Final Node | Kết thúc | — |

### Giải thích logic Fork/Join
Fork/Join được dùng vì "Nhả tiền" và "Gửi SMS báo biến động số dư" là hai hành động **độc lập, không phụ thuộc lẫn nhau**, có thể xảy ra đồng thời. Sau khi cả hai hành động hoàn tất, luồng xử lý được **gộp lại (Join)** trước khi đi đến Final Node, đảm bảo hệ thống chỉ kết thúc giao dịch khi cả tiền đã nhả và SMS đã gửi xong.

### Luồng xử lý tổng quát

```
[Initial] 
   → (Khách hàng) Quét mã QR
   → (Hệ thống) Kiểm tra số dư
        ├── [Đủ số dư] → Fork ─┬─ Nhả tiền ────┐
        │                       └─ Gửi SMS ─────┤→ Join → [Final]
        └── [Không đủ số dư] → Hiển thị lỗi ────────────→ [Final]
```

---

## PHẦN B — USE CASE DIAGRAM

### Kịch bản bổ sung
Khách hàng phải **Đăng nhập** (quét mã QR) trước khi **Rút tiền** (bắt buộc). Khách hàng có thể tùy chọn **In hóa đơn giao dịch** sau khi rút tiền (không bắt buộc). Rút tiền có 2 hình thức: **Rút tiền tiêu chuẩn** và **Rút tiền nhanh** (không cần chọn mệnh giá) — cả hai đều là một dạng của Rút tiền.

### Bảng quan hệ Use Case

| Use Case A | Use Case B | Quan hệ | Giải thích logic |
|---|---|---|---|
| Rút tiền | Đăng nhập | **[include]** | Phải đăng nhập trước khi rút tiền — đây là bước bắt buộc, luôn luôn xảy ra mỗi khi thực hiện Rút tiền |
| Rút tiền | In hóa đơn giao dịch | **[extend]** | In hóa đơn là hành vi tùy chọn, chỉ xảy ra khi khách hàng có nhu cầu sau khi rút tiền thành công — không bắt buộc phải thực hiện mỗi lần |
| Rút tiền | Rút tiền tiêu chuẩn | **[generalization]** | Là một dạng chuyên biệt của Rút tiền (kế thừa hành vi chung, có thêm bước chọn mệnh giá) |
| Rút tiền | Rút tiền nhanh | **[generalization]** | Cũng là một dạng chuyên biệt của Rút tiền, kế thừa hành vi chung nhưng bỏ qua bước chọn mệnh giá |

### Quy tắc chiều mũi tên (ghi nhớ)
- **Include**: nét đứt, mũi tên hở, hướng từ **UC gốc → UC được include** (Rút tiền → Đăng nhập)
- **Extend**: nét đứt, mũi tên hở, hướng từ **UC mở rộng → UC gốc** (In hóa đơn giao dịch → Rút tiền)
- **Generalization**: nét liền, mũi tên tam giác rỗng, hướng từ **UC con → UC cha** (Rút tiền tiêu chuẩn → Rút tiền; Rút tiền nhanh → Rút tiền)

### Cấu trúc sơ đồ
- **Actor**: Khách hàng (ngoài System Boundary)
- **System Boundary**: "Hệ thống ATM RikkeiBank"
- **Use Case bên trong Boundary**: Rút tiền, Đăng nhập, In hóa đơn giao dịch, Rút tiền tiêu chuẩn, Rút tiền nhanh
- **Association**: Khách hàng — Rút tiền

---

*File này đi kèm với `RikkeiBank_Diagrams.drawio` chứa 2 sơ đồ trực quan (Activity Diagram + Use Case Diagram).*
