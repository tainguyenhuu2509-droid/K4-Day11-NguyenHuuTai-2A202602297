# Guideline patch

- **Rule mới đề xuất:**  
  **R10 — Reference disagreement / suspected missing reference:** Khi annotation L và model M cùng xác nhận một object trong cùng vùng ảnh, nhưng reference R không có object tương ứng (`LM_noR`), không được tự động kết luận annotation L là `SPURIOUS`. Ca này phải được đánh dấu là **nghi ngờ reference defect** và chuyển sang escalation để kiểm tra ảnh gốc trước khi sửa annotation hoặc reference. Nếu ảnh gốc xác nhận object thực sự tồn tại và thuộc phạm vi gán nhãn, reference cần được rework; nếu ảnh gốc không xác nhận object, mới xem xét lỗi annotation/model.

- **Áp dụng cho:**  
  Các object detection thuộc các class `Pedestrian`, `Bike`, `Car`, `Truck`, `Bus` và `ThreeWheeler`, đặc biệt các ca `LM_noR` trong đó annotation L và model M cùng phát hiện một object nhưng reference R không có object tương ứng. Áp dụng ở mọi `zone` và `block`; không áp dụng cho `ignore_region` nếu object đã được xác định là nằm trong vùng ignore theo rule tương ứng.

- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:**  
  Các finding hiện tại cho thấy trường hợp `LM_noR` có thể dẫn tới việc gán `SPURIOUS` cho annotation L mặc dù model cũng phát hiện cùng vùng. Trong `adasind_056040.jpg`, `L7+M4` được ghi nhận là `SPURIOUS` với `why=E0_reference_defect` và `action=escalate`: annotation L7 và model M4 cùng phát hiện một `Pedestrian`, trong khi reference không có box tương ứng. Tương tự, `LM_noR` cũng xuất hiện trong các finding khác. Vì vậy cần một bước xác minh ảnh gốc trước khi quyết định rằng annotation L là sai. Rule mới nhằm phân biệt **annotation defect** với **reference defect** thay vì tự động coi reference là ground truth tuyệt đối trong trường hợp L và M cùng xác nhận object.

- **`rules_version` mới:**  
  `v1.1.0`

- **Hiệu lực từ:**  
  Round review tiếp theo sau `r3_diag` — đề xuất áp dụng từ `r4_review`. Các finding đã được ghi nhận ở `v1.0.0` không bị thay đổi hồi tố; chỉ các ca mới hoặc ca được re-review từ `r4_review` trở đi áp dụng R10.