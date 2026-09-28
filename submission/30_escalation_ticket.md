# Escalation ticket

## Ticket 1

- **Frame:** `adasind_056040.jpg` — `object_ref=L7+M4`
  
- **Ảnh chụp:** `submission/screenshots/adasind_056040_L7_M4.png`

- **Expected impact:** Có khả năng reference annotation bị thiếu một `Pedestrian` ở vùng mid. Nếu xác nhận trên ảnh gốc rằng đối tượng thực sự tồn tại, reference cần được bổ sung để tránh đánh giá model là thiếu phát hiện hoặc tạo sai kết quả. Ca này có thể ảnh hưởng đến tính đúng đắn của ground truth và các thống kê lỗi `MISSING/SPURIOUS`.

- **Owner:** `data_ops`

- **Recommendation:** Kiểm tra ảnh gốc `adasind_056040.jpg` tại vùng `L7/M4`. Đối chiếu annotation L với model detection M. Nếu xác nhận có một `Pedestrian` thực sự tồn tại, bổ sung object vào reference và cập nhật lại kết quả compare. Nếu không có đối tượng hợp lệ, giữ nguyên reference và đóng escalation. Không tự động sửa reference chỉ dựa trên model detection.