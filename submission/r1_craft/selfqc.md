# Tự soát

- adasind_036720.jpg L1: chiều cao < H (xem lại phạm vi)
- adasind_036720.jpg L2: chiều cao < H (xem lại phạm vi)
- adasind_036720.jpg L3: chiều cao < H (xem lại phạm vi)
- adasind_036720.jpg L5: truncated khác dự kiến
- adasind_036720.jpg L6: chiều cao < H (xem lại phạm vi)
- adasind_036720.jpg L9: chiều cao < H (xem lại phạm vi)
- adasind_036720.jpg: thiếu ego_body
- adasind_056040.jpg L3: truncated khác dự kiến
- adasind_056040.jpg L5: truncated khác dự kiến
- adasind_056040.jpg: thiếu ego_body
- Tên task thiếu raw_fisheye

> Lưu ý: Các cảnh báo `thiếu ego_body` và `Tên task thiếu raw_fisheye` ở trên thuộc lần chạy `draft` trước. Sau khi kiểm tra và cập nhật annotation, XML mới nhất đã có `ego_body` ở `036720` và `056040`, và tên task hiện tại đã có `raw_fisheye`. Cần chạy lại `draft` để làm mới phần cảnh báo tự động.

## Checklist thủ công

- [x] Phạm vi H=40 và vật cần vẽ

- [x] lens_border và ego_body

- [x] Class sáu nhãn

- [x] Rider và Bike

- [x] Geometry trên ảnh fisheye gốc

- [ ] truncated và occluded

- [x] Vật thiếu hoặc box trùng

- [x] ignore_region có reason

- [x] Tên task raw_fisheye và export CVAT 1.1

## Fill ratio (K12)

- adasind_006840.jpg box 4 mid: 0.518

- adasind_006840.jpg box 1 mid: 0.684

- adasind_006840.jpg box 5 mid: 0.654

- adasind_006840.jpg box 3 center: 0.745

- adasind_006840.jpg box 2 center: 0.702

- mean center: 0.724 (n=2)