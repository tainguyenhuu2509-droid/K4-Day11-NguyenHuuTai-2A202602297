# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 10 | 1 | 1 | 5 | 6 | WRONG_CLASS (1) |
| mid | 7 | 0 | 1 | 3 | 6 | SPURIOUS (1) |
| edge | 3 | 0 | 0 | 2 | 5 | ATTRIBUTE (1) |

## Nhận xét

- Zone có nhiều vấn đề nhất là `center`. Với annotation L, `center` có 1 missing và 1 spurious, cao hơn `mid` (0 missing, 1 spurious) và `edge` (0 missing, 0 spurious). Với model M, `center` cũng có nhiều vấn đề nhất với 5 missing và 6 thừa, tổng cộng 11 trường hợp; `mid` có 3 missing và 6 thừa, tổng cộng 9 trường hợp; `edge` có 2 missing và 5 thừa, tổng cộng 7 trường hợp. Lỗi chính của L tại `center` là `WRONG_CLASS`.

- Giả thuyết ban đầu là các vấn đề có thể liên quan đến hình học fisheye và sự thay đổi hình dạng của đối tượng theo vị trí trong ảnh; một số box ở vùng rìa có thể nhạy với méo hình và thuộc tính `truncated`, trong khi các khác biệt về class có thể xuất hiện khi đối tượng có hình dạng hoặc thành phần khó phân biệt. Các vùng `ego_body` và `lens_border` cũng làm thay đổi phạm vi quan sát hợp lệ và cần được kiểm tra khi phân tích từng ca. Tuy nhiên, đây chỉ là giả thuyết từ slice hiện tại, chưa đủ để khẳng định nguyên nhân.

- Slice này chỉ gồm ba frame (`adasind_006840.jpg`, `adasind_036720.jpg`, `adasind_056040.jpg`), vì vậy số lượng và phân bố lỗi có thể bị ảnh hưởng mạnh bởi nội dung riêng của từng frame. `center`, `mid` và `edge` chỉ biểu thị vị trí tương đối của object so với tâm vòng kính; chúng không cho biết object gần hay xa xe, cũng không cho biết camera là phía trước, phía sau, trái hay phải. Do đó không thể dùng kết quả của slice ba frame này để suy rộng cho toàn bộ hệ thống bốn camera.