# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | ATTRIBUTE | 1 |
| center | B1 | IGNORE_SCOPE | 1 |
| center | B1 | MISSING | 4 |
| center | B1 | SPURIOUS | 1 |
| center | B1 | WRONG_CLASS | 7 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 2 |
| edge | B1 | ATTRIBUTE | 2 |
| edge | B1 | DUPLICATE | 2 |
| edge | B1 | MISSING | 2 |
| edge | B1 | SPURIOUS | 2 |
| edge | B1 | WRONG_CLASS | 1 |
| mid | B1 | ATTRIBUTE | 1 |
| mid | B1 | DUPLICATE | 1 |
| mid | B1 | MISSING | 3 |
| mid | B1 | SPURIOUS | 2 |
| mid | B1 | WRONG_CLASS | 4 |

## Top defects
- WRONG_CLASS: 12 (ví dụ frame adasind_006840.jpg)
- MISSING: 10 (ví dụ frame adasind_019560.jpg)
- SPURIOUS: 7 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

### Lỗi nổi bật nhất: WRONG_CLASS

Lỗi nổi bật nhất là `WRONG_CLASS` với **12 trường hợp**. Lỗi xuất hiện ở cả ba zone trong block `B1`: **7 trường hợp ở center, 4 ở mid và 1 ở edge**. Một frame tiêu biểu là `adasind_006840.jpg`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**  
  Có hai dạng `WRONG_CLASS` trong findings. Một số trường hợp được chẩn đoán là `E1_annotator_error`, trong đó annotation L và reference R mô tả cùng một object nhưng khác class. Ví dụ, `object_ref=L2+R4` trong frame `adasind_006840.jpg`: L2 được gán `Car` trong khi R4 được gán `ThreeWheeler`. Finding ghi nhận hai box chồng lên cùng một vùng và khác biệt chính là class. Ngoài ra, một số `WRONG_CLASS` khác được chẩn đoán là `E4_model_domain`, ví dụ `M3`, `M6`, `M9` là `Pedestrian` trên vùng `Bike`, và `M8`, `M10`, `M13` là các dự đoán `Truck`/`Car` trên vùng `ThreeWheeler`. Vì vậy, lỗi `WRONG_CLASS` không chỉ đến từ annotation mà còn xuất hiện ở phía model.

- **Cách sửa và ai nhận việc (`owner`):**  
  Với các case `E1_annotator_error`, `owner=annotator`, action=`rework`: mở frame tương ứng trong CVAT, đối chiếu object với ảnh gốc và rule liên quan rồi sửa class của annotation. Cụ thể với `adasind_006840.jpg`, `L2+R4`, cần kiểm tra object tại vùng của hai box và xác định class đúng theo `R04`; không tạo thêm object mới nếu L2 và R4 thực sự là cùng một object.  
  Với các case `E4_model_domain`, `owner=ai_team`, action=`keep_with_reason`: giữ finding để theo dõi lỗi của model, đặc biệt các trường hợp model nhầm `Bike` thành `Pedestrian` và `ThreeWheeler` thành `Car`/`Truck`. Các case này không nên sửa annotation chỉ để làm cho model khớp annotation.

- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**  
  Trong `findings.csv`, frame `adasind_006840.jpg` có:
  - `L2+R4` — `WRONG_CLASS`, `E1_annotator_error`, `P1`, `owner=annotator`, `rule_id=R04`: L2 là `Car`, R4 là `ThreeWheeler` và hai box mô tả cùng vùng.
  - `M3` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `owner=ai_team`, `rule_id=R03`: model nhận `Pedestrian` trên vùng `Bike` `L6/R5`.
  - `M6` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `rule_id=R03`: model nhận `Pedestrian` trên vùng `Bike` `L8/R3`.
  - `M8` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `rule_id=R04`: model nhận `Truck` trên vùng `ThreeWheeler` `L9/R6`.
  - `M9` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `rule_id=R03`: model tiếp tục nhận `Pedestrian` trên vùng `Bike` `L8/R3`.
  - `M10` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `rule_id=R04`: model nhận `Car` trên vùng `ThreeWheeler` `L9/R6`.
  - `M13` — `WRONG_CLASS`, `E4_model_domain`, `P1`, `rule_id=R04`: model nhận `Car` trên vùng `ThreeWheeler` `L4/R1`.

  Trong `annotations-v2.xml`, frame `adasind_006840.jpg` thực sự chứa các annotation `Bike`, `ThreeWheeler`, `Pedestrian`, `Car`,... ở các vùng tương ứng. Tuy nhiên, file XML chỉ cung cấp annotation và không tự xác nhận class nào là đúng khi hai nguồn annotation khác nhau. Vì vậy, ảnh gốc trong `screenshots/` cần được dùng để xác nhận lần cuối trước khi rework.

### Ghi chú về các lỗi còn lại

`MISSING` có **10 trường hợp**, trong đó nhiều case thuộc `E4_model_domain`: cả annotation L/R đều xác nhận object nhưng model không phát hiện. Ví dụ `adasind_006840.jpg` có `L3+R7`, `L4+R1` và `L9+R6` đều được ghi nhận là model bỏ sót.

`SPURIOUS` có **7 trường hợp**. Trong đó có các case `E1_annotator_error`, chẳng hạn `adasind_019560.jpg` với `L4` và `L5`, và các case `E0_reference_defect` như `adasind_056040.jpg` với `L7+M4`. Các case `E0` được đánh dấu `escalate` vì reference có khả năng thiếu object và cần xác minh bằng ảnh, không nên mặc định coi annotation/reference là đúng tuyệt đối.

Hai bảng `Zone × block` và `Top defects` do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật các bảng này và giữ nguyên phần phân tích bên dưới.