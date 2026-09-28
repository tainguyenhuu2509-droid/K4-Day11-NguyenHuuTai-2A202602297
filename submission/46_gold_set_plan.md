# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống giả lập trên slide, không phải 50.000 frame có trong repo.

| camera_id | Normal case cần chọn | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review và xử lý bất đồng |
|---|---|---|---|---|---|
| front | Cảnh giao thông bình thường, vật thể nằm rõ trong vùng quan sát và không bị che khuất đáng kể | Vật ở rìa ảnh, méo fisheye, vật bị cắt và vùng seam phía trước | Hình học bị biến dạng ở vùng rìa; vật có thể bị truncated hoặc xuất hiện đồng thời ở vùng chồng giữa các camera | Giữ hệ tọa độ ảnh gốc của front; lưu calibration/extrinsic tương ứng nếu hệ thống cung cấp; không tự đổi sang hệ tọa độ khác khi chưa có policy | Hai reviewer rà độc lập trước khi đối chiếu. Nếu bất đồng về box, class hoặc attribute, ghi rõ frame/object và lý do; resolve theo guideline/rule đã được thống nhất trước khi đưa vào gold |
| rear | Cảnh giao thông bình thường với object quan sát rõ và ít che khuất | Vật bị che khuất, nằm sát rìa ảnh và xuất hiện trong seam phía sau | Visibility thấp và distortion có thể làm box không ổn định; cùng object có thể được nhìn thấy từ camera khác | Giữ annotation trên ảnh gốc rear và calibration/extrinsic tương ứng; giữ timestamp nếu cần đối chiếu cross-camera | Hai reviewer độc lập; sau đó đối chiếu kết quả. Bất đồng phải được adjudicate và lưu frame, object_ref và lý do quyết định |
| left | Cảnh bình thường với object nằm trong vùng quan sát ổn định của camera trái | Vùng mép fisheye, occlusion và seam bên trái | Góc nhìn rộng gây distortion mạnh; object gần seam có thể tạo box khác nhau giữa left và camera lân cận | Giữ hệ tọa độ ảnh gốc của left và calibration/extrinsic tương ứng; giữ timestamp để đối chiếu với camera khác khi cần | Double-review độc lập; kiểm tra geometry, class và attribute. Nếu không thống nhất, chuyển sang adjudication theo rule thay vì tự chọn một annotation |
| right | Cảnh bình thường với object nằm rõ trong vùng quan sát của camera phải | Vùng mép fisheye, occlusion và seam bên phải | Distortion và che khuất có thể làm thay đổi kích thước/vị trí box; seam có thể khiến cùng object xuất hiện ở hai camera | Giữ hệ tọa độ ảnh gốc của right và calibration/extrinsic tương ứng; giữ timestamp và thông tin seam nếu có | Hai annotator review độc lập; bất đồng phải được ghi nhận và adjudicate trước khi frame được gọi là gold |

## Tiêu chí chọn gold

Mỗi camera cần có cả **normal case** và **hard case**. Hard case không chỉ là các frame có lỗi rõ ràng mà phải bao phủ những điều kiện dễ gây sai annotation hoặc sai quyết định cross-camera như:

- object ở vùng rìa;
- fisheye/distortion;
- occlusion hoặc truncation;
- nhiều object chồng lấn;
- object xuất hiện gần hoặc trong vùng seam;
- trường hợp cần đối chiếu giữa hai camera.

Một frame chỉ được đưa vào gold sau khi annotation đã được review độc lập và các bất đồng đã được giải quyết theo rule/guideline hiện hành.

## Annotation space / calibration

Gold set phải giữ annotation trong **hệ tọa độ ảnh gốc của từng camera**. Đồng thời cần lưu calibration/extrinsic tương ứng nếu hệ thống cung cấp.

Không được tự động biến đổi hoặc ghép annotation giữa các camera chỉ dựa trên vị trí box trong ảnh. Khi cần đối chiếu cross-camera phải có thông tin calibration và timestamp tương ứng.

## Review độc lập và giải quyết bất đồng

Mỗi ca gold được hai reviewer kiểm tra độc lập trước khi được chấp nhận.

Quy trình:

1. Reviewer A và Reviewer B tạo/kiểm tra annotation độc lập.
2. Đối chiếu box, class, attribute và các thông tin liên quan đến seam.
3. Nếu kết quả giống nhau, frame có thể được đưa vào gold sau bước QA.
4. Nếu có bất đồng, ghi lại `frame`, `object_ref`, nội dung bất đồng và lý do.
5. Adjudicator/QA giải quyết theo guideline và rule hiện hành.
6. Chỉ sau khi bất đồng được giải quyết mới coi annotation là gold.

## Khi nào cần refresh gold set

Cần refresh gold set khi có thay đổi có thể làm thay đổi cách annotation hoặc diễn giải dữ liệu, bao gồm:

- thay đổi camera;
- thay đổi calibration/extrinsic;
- thay đổi hệ tọa độ hoặc cách chuyển đổi annotation;
- thay đổi định nghĩa annotation;
- thay đổi guideline/rule ảnh hưởng đến class, attribute, ignore region hoặc seam;
- phát hiện một nhóm lỗi mới có thể làm cho các ca gold hiện tại không còn đại diện.

Khi refresh, ưu tiên review lại các ca bị tác động trực tiếp thay vì mặc định giữ nguyên toàn bộ gold set.

## Seam / cross-camera policy

**Seam** là vùng nhìn chồng giữa hai hoặc nhiều camera.

Một ca seam phải được giữ lại như một ca riêng trong gold set vì cùng một object có thể xuất hiện trên nhiều camera với các box khác nhau.

**Không được tự động ghép hai box hoặc xoá một box chỉ vì chúng có vẻ thuộc cùng một object.** Trước khi thực hiện association/cross-camera tracking cần có:

- timestamp;
- calibration/extrinsic tương ứng;
- thông tin camera nguồn;
- policy xác định khi nào hai observation được xem là cùng một object;
- policy xác định output đích sau khi association.

Nếu thiếu một trong các thông tin trên, ca seam chỉ nên được giữ để review/escalation và chưa dùng làm căn cứ cho việc ghép box hoặc nối track.

## Vì sao quality report trên một camera chưa chứng minh gold set đúng cho cả bốn camera

Peer agreement hoặc quality report trên một camera chỉ phản ánh chất lượng trong phạm vi camera đó. Bốn camera có góc nhìn, distortion, vùng seam và điều kiện quan sát khác nhau.

Do đó, kết quả review của một camera không đủ để suy rộng trực tiếp rằng gold set của cả bốn camera đã đúng hoặc đầy đủ. Gold set cần được xây dựng và kiểm tra theo từng camera, đồng thời có các ca seam để kiểm tra những vấn đề chỉ xuất hiện khi đối chiếu cross-camera.