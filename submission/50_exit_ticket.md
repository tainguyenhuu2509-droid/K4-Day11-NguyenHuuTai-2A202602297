# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?

   Không nên mặc định coi đây là `DUPLICATE`. Khi cùng một vật xuất hiện ở vùng seam, hai box trên hai camera có
   thể là hai quan sát của cùng một object thay vì hai annotation của cùng một object trong cùng camera. Vì vậy
   cần có policy riêng cho cross-camera association trước khi merge hai box hoặc nối track.

   Trước khi quyết định hai box có cùng identity hay không cần đối chiếu ít nhất timestamp, camera/source và
   calibration/extrinsic. Nếu chưa có đủ bằng chứng hoặc policy output đích thì giữ hai observation riêng và
   đưa ca vào review thay vì tự động xoá một box hoặc gán `DUPLICATE`.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.

   Trên cùng một camera, cần giữ cùng track ID khi các observation liên tiếp vẫn có đủ bằng chứng cho thấy đó
   là cùng một object và không có sự kiện làm mất identity. Keyframe nên được giữ khi cần một observation đại
   diện để mô tả thay đổi đáng kể của object/trajectory hoặc làm mốc cho việc kiểm tra track.

   Trạng thái `Outside` cần được dùng khi object thực sự rời khỏi vùng quan sát theo định nghĩa của hệ thống,
   thay vì coi một frame bị mất detection là object mới. Không nên tạo track ID mới chỉ vì một vài frame không
   có box nếu bằng chứng temporal vẫn cho thấy cùng identity.

   Trước khi nối track qua hai camera cần có timestamp đồng bộ, calibration/extrinsic giữa các camera và policy
   xác định điều kiện association/identity. Cũng cần xác định output đích: giữ track riêng theo camera hay tạo
   một cross-camera identity. Nếu thiếu các thông tin này thì không tự động nối track.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/object_ref),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?

   Ca thực tế: `adasind_056040.jpg`, `object_ref=L7`.

   L7 được annotation L và model cùng phát hiện là một `Pedestrian`, nhưng reference không có box tương ứng.
   Vì vậy compare báo `SPURIOUS`. Tuy nhiên findings không kết luận ngay rằng L7 là annotation sai mà ghi
   `why=E0_reference_defect`, `owner=data_ops` và `action=escalate`. Bằng chứng ghi rõ L7 đi cùng M4 trong
   cell `LM_noR`, trong đó annotation L và model cùng thấy object nhưng reference không có box.

   Tôi xử lý bằng cách không tự xoá L7 và cũng không tự sửa reference. Ca được escalate để `data_ops` kiểm tra
   lại ảnh gốc và reference trước khi thay đổi ground truth. Đây là cách tránh coi reference là đúng tuyệt đối
   khi bằng chứng từ annotation và model không phù hợp với reference.

   Nếu làm lại slice này, tôi sẽ kiểm tra reference và vùng ảnh gốc sớm hơn đối với các ca `LM_noR`, đồng thời
   tách rõ ba khả năng: lỗi annotation, lỗi model và lỗi reference. Tôi cũng sẽ giữ frame/object_ref và ảnh
   chụp làm evidence trước khi quyết định rework hoặc escalate.

## Screenshot evidence

- `submission/screenshots/adasind_056040_L7_M4.png` — dùng để đối chiếu ca `L7+M4` trong `adasind_056040.jpg`.
- `submission/screenshots/adasind_006840_L2_R4.png` — dùng để đối chiếu ca `L2+R4` `WRONG_CLASS`.

Các screenshot phải được giữ trong `submission/screenshots/` và được dẫn từ ticket/findings tương ứng trước khi
chuyển sang mục 4 để kiểm và nộp.